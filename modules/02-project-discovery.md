# 02 Project Discovery / 项目事实发现

Use this module when analyzing a project, planning a knowledge system, aligning docs with source, or generating repowiki-style content.

## Anti-hallucination gates

Before generating repowiki-style docs or making source-alignment claims, force explicit evidence boundaries.

Track these during discovery:

- files/configs already inspected
- relevant files/configs not yet inspected
- domains absent in inspected evidence
- domains plausible but unverified
- claims that must not be made without additional source/config checks

Rules:

- Do not infer backend, CI/CD, monitoring, tests, native, deployment, or security behavior from project type alone.
- Do not treat dependency presence as runtime usage without checking registration/call sites.
- Do not treat old docs, generated wiki, build artifacts, or commented code as current behavior without source confirmation.
- Prefer `not found in inspected source/config` over filling blanks with best practices.
- If a domain was not inspected in the current run, mark it as `not assessed in inspected scope`; do not summarize it as a project norm or generate a topic doc for it.

## Project type decision tree

Before choosing canonical docs, identify project type from source/config evidence. Do not force every project into the same full wiki shape.

| Project type | Signals | Default docs |
| --- | --- | --- |
| Frontend SPA | `vite.config.*`, `vue.config.js`, `src/main.*`, `src/App.*`, client routes | frontend architecture, routing, state, components, build/deploy |
| Full-stack web | frontend entrypoints plus server routes/controllers/API handlers | frontend, backend services, API contracts, data/storage, build/deploy |
| Backend service | server entrypoint, controllers/routes, services, database/migrations | backend services, API contracts, data/storage, testing, ops |
| Monorepo | workspaces, multiple packages/apps/services | repo map, package ownership, cross-package build/test, per-app docs |
| Mobile/hybrid | `android/**`, `ios/**`, `cordova/**`, `capacitor.config.*`, native bridge | mobile adaptation, native bridge, build/package, release/ops |
| Mini Program | `app.json`, `pages/**`, mini-program config | page routing, API/service, state/cache, release/testing |
| Infra/devops | Terraform/K8s/Docker/CI-heavy, little app code | infrastructure, CI/CD, deployment/ops, observability, security |
| Library/package | package exports, src library code, examples, tests | API surface, build/release, tests, compatibility |

Rules:

- Choose docs from actual project shape and user goals, not from a universal checklist.
- For mixed projects, create an ownership map first so Agents know which app/service/package owns a task.
- For small projects, prefer one concise guide plus an Agent index over a full encyclopedia.

## MCP availability and fallback

This skill uses three MCP servers for enhanced analysis. Each is optional; the skill degrades gracefully when unavailable.

**Detection**: Call the MCP tool and handle errors gracefully. If tool returns "not found" or "server not available", the MCP is not installed.

**Installation**: MCP servers are configured in Claude Code settings. If an MCP is missing:
- User must install and configure it in settings
- Skill cannot auto-install (requires user permissions and settings changes)
- Refer user to MCP documentation or Claude Code MCP setup guide

**Fallback strategies**:

| MCP | Purpose | Fallback when unavailable |
| --- | --- | --- |
| Codegraph | Semantic code analysis | Use `grep` + manual file reading (slower, less accurate) |
| Context7 | Library documentation queries | Skip library verification, or use web search as last resort |
| Memory | Knowledge graph construction | Skip graph construction (only impacts large projects) |

**Rule**: Always check MCP availability before using. Never assume an MCP is present.

## Code intelligence approach

When codegraph MCP server is available, prefer semantic code analysis over manual file inspection.

### Check codegraph availability

Before starting manual discovery, check if codegraph is available:

```
Call `codegraph_status` to verify the index exists and is ready.
```

If codegraph is available, use the code-intelligence-first workflow below. If not, fall back to the standard manual workflow.

### Code-intelligence-first workflow

When codegraph is available, use this approach before reading files manually:

1. **Project structure overview**: Call `codegraph_files` to see the full file tree with symbol counts
2. **Symbol discovery**: Use `codegraph_search` to locate key symbols (API handlers, components, stores, routes)
3. **Architectural understanding**: Use `codegraph_context` with task descriptions to get entry points and related symbols
4. **Trace execution flows**: Use `codegraph_trace from→to` to understand how data flows through the system

Fall back to manual file reading only when:
- Codegraph index doesn't exist or is out of sync
- Need to verify specific implementation details not captured in the graph
- Reading config files (package.json, build configs, CI files) that codegraph doesn't index
- Examining comments, documentation, or non-code content

## Library documentation verification

When dependencies are detected during project discovery, verify current API usage against official docs using Context7 MCP.

### Context7 workflow

1. Extract major dependencies from `package.json`, `requirements.txt`, `pom.xml`, `go.mod`, etc.
2. For each critical dependency (frameworks, core libraries):
   - Call `resolve-library-id` with library name and project context
   - If library ID found, call `query-docs` with specific question about API usage patterns
3. Compare documented API patterns against actual project usage (use codegraph to find usage)
4. Flag version mismatches or deprecated API usage

### When to use Context7

Use for:
- Framework version verification (React, Vue, Next.js, Express, Django, Spring)
- Core library API patterns (axios, prisma, sequelize, mongoose)
- Build tool configuration (vite, webpack, rollup, esbuild)

Skip for:
- Internal modules
- Small utilities
- Deprecated packages with no active docs

### Token efficiency

- Query only critical dependencies (≤5 libraries per project)
- Ask specific questions about actual usage patterns found via codegraph
- Cache resolved library IDs to avoid repeated resolution calls

## Knowledge graph construction (optional)

For large or long-term maintenance projects, optionally build a persistent knowledge graph using Memory MCP after project discovery.

**When to use**:
- Project has >100 files
- Multiple domains (frontend + backend + shared)
- Long-term maintenance expected
- Multiple AI Agents will work on the project

**When to skip**:
- Small projects (<50 files)
- Short-term/prototype projects
- Memory MCP not available

**Workflow**:

1. Extract entities: Use `codegraph_files` and `codegraph_search` to identify key symbols (components, services, routes, stores)
2. Create entities: `create_entities` with types (Module, Component, Service, Route, API, Store, Config) and observations (path, purpose)
3. Extract relations: Use `codegraph_callers` and `codegraph_callees` to identify call/dependency relationships
4. Create relations: `create_relations` with types (depends-on, calls, uses, renders, contains, imports)

**Token budget**: 20-25K tokens for initial construction (recovers after 2+ graph queries vs repeated file reads)

## Standard workflow

### 1. Understand project shape

First identify:

- package manager and available scripts
- frontend framework, app entrypoint, routing model, state model, component/layout conventions, and runtime plugins
- backend framework, server entrypoint, API routes/controllers/services, database/migration layer, job/queue layer, and auth/session model when present
- API/service layer, request signing, generated clients, schemas/contracts, and environment switching
- test/lint/typecheck status, real scripts, test directories, fixtures, and whether the suite appears maintained or scaffold-only
- CI/CD, build/deploy pipeline, release scripts, environment files, container/infrastructure config, and rollback path when present
- operations and maintenance signals: logging, metrics, tracing, error reporting, monitoring dashboards, health checks, runbooks, and on-call docs when present
- security and privacy norms: auth, permissions, secrets handling, data protection, dependency scanning, and sensitive configuration boundaries when present
- existing wiki or knowledge-base location, and whether it should be migrated/aligned into project-root `Docs/`
- existing project instructions or memory files
- token-saving navigation opportunities: canonical docs and source entrypoints that let future Agents avoid loading broad docs

### 2. Inspect source and config entrypoints

Examples of files to inspect when they exist:

- package/build:
  - `package.json`
  - lockfiles
  - `pyproject.toml`
  - `go.mod`
  - `pom.xml`
  - `build.gradle*`
  - `Makefile`
- docs/instructions:
  - `README.md`
  - `CLAUDE.md`
  - `AGENTS.md`
  - `.cursor/rules/**`
  - `.github/copilot-instructions.md`
  - `.qoder/**`
- framework configs:
  - `vue.config.js`
  - `vite.config.*`
  - `next.config.*`
  - `nuxt.config.*`
  - `angular.json`
  - `webpack.config.*`
- common frontend entrypoints:
  - `src/main.*`
  - `src/App.*`
  - `src/router/**`
  - `src/store/**`
  - `src/config/**`
  - `src/services/**`
- build/deploy:
  - `build/**`
  - `.github/workflows/**`
  - `Dockerfile`
  - deployment scripts
- generated or legacy docs:
  - `.qoder/repowiki/**`
  - `docs/**`
  - `wiki/**`

Default output location for concise project docs is project-root `Docs/**`.

Default output location for full repowiki-style topic encyclopedias is:

```text
Docs/repowiki/zh/content/**
```

Choose the actual topic directories from inspected project facts and the project-type preset in `modules/04-repowiki-style.md`.

## Ecosystem-specific entrypoints

Use these only when the matching project type is present.

| Ecosystem | Entry points |
| --- | --- |
| React/Vite | `src/main.*`, `src/App.*`, `src/routes/**`, `vite.config.*` |
| Next.js | `app/**`, `pages/**`, `src/app/**`, `src/pages/**`, `next.config.*` |
| Nuxt | `nuxt.config.*`, `app.vue`, `pages/**`, `server/**` |
| Vue/Vue CLI | `src/main.*`, `src/App.*`, `src/router/**`, `vue.config.js` |
| Node/Express | `server.*`, `app.*`, `routes/**`, `controllers/**`, `services/**` |
| NestJS | `src/main.ts`, `src/app.module.ts`, `src/**/module.ts`, `src/**/controller.ts`, `src/**/service.ts` |
| Java/Spring | `pom.xml`, `build.gradle*`, `src/main/java/**`, `src/main/resources/**`, controller/service/repository packages |
| Go | `go.mod`, `cmd/**`, `internal/**`, `pkg/**`, route/server initialization |
| Python | `pyproject.toml`, `requirements*.txt`, `manage.py`, `app.py`, `src/**`, `api/**`, `routers/**` |
| Mini Program | `app.json`, `app.js`, `pages/**`, `project.config.json` |
| Mobile/hybrid | `android/**`, `ios/**`, `capacitor.config.*`, `cordova/**`, `config.xml` |
| Infra | Terraform, Kubernetes manifests, Docker, CI workflows, deployment scripts |

## Engineering fact inventory

Before generating or aligning a repowiki-style knowledge base, produce or internally complete a fact inventory so missing domains are not invented.

> Output form: `modules/05-output-templates.md` §Engineering fact inventory output template. Final review: `modules/06-review-checklists.md` §Final review checklist (fact inventory completion check).

Recommended matrix:

```md
| Domain | Present? | Verified evidence | Current fact | Unknown/gap | Suggested doc |
| --- | --- | --- | --- | --- | --- |
| Frontend | yes/no/partial | source/config | framework, entrypoint, routing, state, build | ... | ... |
| Backend/API | yes/no/partial | source/config | server, controllers, services, contracts | ... | ... |
| Data/storage | yes/no/partial | source/config | database, migrations, cache, persistence | ... | ... |
| Testing | yes/no/partial | scripts/tests | unit/integration/e2e/lint/typecheck status | ... | ... |
| CI/CD | yes/no/partial | workflows/scripts | build, release, deploy pipeline | ... | ... |
| Deployment/ops | yes/no/partial | infra/runbooks | environment, rollback, health checks | ... | ... |
| Observability | yes/no/partial | logging/metrics | logs, metrics, tracing, alerts | ... | ... |
| Security/privacy | yes/no/partial | auth/config | auth, permissions, secrets, data handling | ... | ... |
| Maintenance | yes/no/partial | docs/source | ownership, high-risk areas, upgrade notes | ... | ... |
```

Rules:

- If a domain is absent, say `not found in inspected source/config` instead of generating generic guidance.
- If evidence is partial, mark it partial and route future Agents to the verification source.
- Use this inventory to decide canonical docs and to keep final wiki size proportional to project size.

## Discovery output for plans

When asking approval for a knowledge-system plan, include:

- project type and evidence
- inspected files/configs
- not-yet-inspected but relevant files/configs
- fact inventory summary
- recommended output scale
- proposed canonical docs
- proposed Agent navigation entrypoints
- risks and anti-hallucination boundaries
