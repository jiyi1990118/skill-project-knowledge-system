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
