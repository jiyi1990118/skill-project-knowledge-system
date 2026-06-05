# 01 Core Principles / 核心原则

Use this module for every project-knowledge-system task. It contains the non-negotiable rules that must shape all outputs.

## Source code is the source of truth

- Prefer current source files, package scripts, build configs, route configs, API configs, runtime entrypoints, and CI files over existing docs.
- Existing docs may be stale, generated, incomplete, or overly broad.
- Repowiki-style output must be based on the current project's actual facts, not generic templates, assumptions from another project, or old README text.
- Generated wiki content is an input, not authority. Do not cite generated docs as evidence for source behavior; verify claims against source or soften them.

## Cover the project's real engineering norms

When the user asks for a repowiki or project knowledge system, inspect and summarize only the norms that actually exist in the project:

- frontend architecture and runtime entrypoints
- backend/API/service architecture
- data/storage/migration/cache behavior
- testing, lint, typecheck, fixture, and mock status
- CI/CD, build, release, deploy, rollback paths
- operations, health checks, runbooks, on-call signals
- observability: logs, metrics, tracing, alerts, error reporting
- security/privacy: auth, permissions, secrets, data handling, dependency scanning
- maintenance boundaries, high-risk areas, ownership signals

If a domain is absent or unverified, say so explicitly instead of inventing a convention.

## Separate memory from documentation

- Memory stores durable, non-obvious context that helps future conversations.
- Documentation stores project facts, architecture, workflows, commands, and implementation guidance.
- Do not put large architecture snapshots into memory when they can be derived from code or docs.
- Do not use memory as a substitute for `Docs/` or source verification.

## Preserve human knowledge-base value

- Do not aggressively delete generated encyclopedia-style content.
- Do not collapse all docs into one short AI-only file.
- Keep diagrams, explanations, domain summaries, troubleshooting sections, conclusions, and appendices when they are still useful.
- Add AI Agent navigation on top of human-facing docs instead of replacing human docs.

## Add an AI Agent navigation layer that reduces token use

- Large wikis are useful for humans but expensive for Agents.
- Add short task-routing pages and `AI Agent 使用建议` to canonical docs.
- Help future Agents know what to read first, what source files to verify, and what commands are actually available.
- Prefer concise indexes, routing tables, and source pointers so future Agents can read narrow topic docs instead of loading the whole wiki.

## Avoid unverified claims

Never claim these exist unless confirmed by current source/config/scripts/runtime entrypoints:

- scripts, lint commands, test commands, package/build commands
- backend services, APIs, migrations, queues, cron jobs
- PWA, service worker, offline mode
- native bridge, mobile packaging, app-store release behavior
- CI/CD, deployment, rollback, operations, health checks
- observability, monitoring, logging, tracing, alerts
- auth, permissions, encryption, sensitive-data handling

If evidence is partial, use conservative language:

- `current source does not confirm...`
- `not found in inspected source/config...`
- `verify runtime registration before relying on...`

If a domain was not inspected in the current run, mark it as `not assessed in inspected scope` rather than `unverified`. Do not produce recommendations, topic docs, or engineering norms for not-assessed domains unless the user asks for follow-up discovery.

## Prefer narrow reading

- Start from the smallest relevant source files and docs.
- Avoid bulk-reading an entire wiki unless the user explicitly asks for a broad documentation audit.
- When a generated repowiki exists, read only the relevant topic file first, then verify claims against source.

## Sync documentation entrypoints to memory

After generating or aligning docs / Agent instructions, sync a lightweight pointer (entrypoint + purpose + freshness reminder) to the target project's memory when memory is available. Verify memory scope belongs to the target project, not this skill repository or another working directory; skip and report if scope is ambiguous.

Detailed scoping rules, body templates, and skip conditions: see `modules/07-memory-policy.md`.

## Source-of-truth priority

When documentation and code conflict, use this order:

1. Current source code and actual runtime entrypoints.
2. Package/build/config files:
   - `package.json`
   - lockfiles
   - build configs
   - framework configs
   - environment files
   - CI configs
3. Project-level Agent instructions:
   - `CLAUDE.md`
   - `AGENTS.md`
   - `.cursor/rules/**`
   - `.qoder/**`
   - `.github/copilot-instructions.md`
4. Existing canonical docs.
5. Generated wiki/background docs.
6. README files, if they appear stale.

## Request routing

Choose output mode by user request instead of always writing files.

| User request | Primary output | Write files? |
| --- | --- | --- |
| “分析这个项目文档体系” | documentation-system audit report | No, unless requested |
| “生成类似 repowiki” | knowledge-base plan + repowiki-style topic docs | Plan first |
| “更新 CLAUDE.md / AGENTS.md” | project-level Agent instructions | Read current file first |
| “建立 memory 规范” | memory policy or `AI-MEMORY.md` | Usually after plan |
| “检查文档是否过时” | source-alignment audit | No direct rewrite unless requested |
| “学习这个 repowiki 风格” | style summary or skill-improvement suggestion | No project docs by default |

## Planning and approval rule

For non-trivial documentation generation, wiki migration, or multi-file restructuring, first produce a plan and wait for user approval before writing files.

A plan should include:

- files to create
- files to update
- existing docs to read, preserve, align, or migrate
- canonical docs to establish
- source files and configs that will be used as evidence
- output location
- validation checklist

Small single-file edits can proceed directly when the user explicitly asks for that edit.

## Output scale

Choose the smallest useful knowledge system for the project size.

### Small project

- `CLAUDE.md` or `AGENTS.md`
- `Docs/AI-Agent索引.md`
- `Docs/开发者指南.md` or a concise project overview

### Medium project

- Add canonical topic docs for routing, API/service, state, components, build/deploy, or equivalent core domains.
- Add source-backed diagrams only for real cross-cutting systems.
- Keep the Agent index short and task-oriented.

### Large project

- Use a full repowiki-style topic encyclopedia.
- Add AI Agent index, canonical topic docs, subtopic docs, troubleshooting, appendices, and evidence links.
- Preserve human-facing explanations while adding Agent navigation and source-verification guidance.

## Documentation layering and compression

Put information at the smallest durable layer that future Agents need.

| Layer | Purpose | Keep short? | Should contain | Should avoid |
| --- | --- | --- | --- | --- |
| `CLAUDE.md` / `AGENTS.md` | operational instructions for Agents | yes | commands, high-impact areas, task routes, repo-specific caveats | encyclopedia prose, duplicated topic docs |
| `Docs/AI-Agent索引.md` | task navigation and token-saving entrypoint | yes | task -> doc -> source maps, verification reminders | long architecture explanations |
| Canonical docs | authoritative topic overview | medium | source-backed architecture, AI quick path, key diagrams, freshness triggers | unrelated subtopic detail |
| Subtopic docs | deep domain detail | flexible | focused flows, troubleshooting, source citations | repeated global overview |
| Appendices | reference material | flexible | command tables, config tables, field maps | narrative required for first read |
| Memory | durable non-code context | very short | user/feedback/project/reference context not derivable from source | architecture snapshots or file lists |

Rules:

- If content helps every Agent immediately, put a short pointer in `CLAUDE.md` or the Agent index.
- If content explains a system, put it in one canonical doc and link to it.
- If content is rarely needed or large, move it to a subtopic or appendix.
- Prefer links and source pointers over copying content between layers.

## Evidence citation levels

Use the narrowest useful evidence for each claim.

1. File-level evidence:
   - Use when describing a module, directory, or broad responsibility.
   - Example: `[src/router/index.ts](file://src/router/index.ts)`.
2. Line-level evidence:
   - Use for concrete behavior, commands, exported functions, route rules, API maps, config values, or runtime branching.
   - Example: `[package.json:12-20](file://package.json#L12-L20)`.
3. Partial or unverified evidence:
   - Mark conservatively instead of presenting as fact.
   - Example: `current source suggests...`, `verify runtime registration before relying on this`.

For repowiki-style output, this section sets the abstract levels; the strict claim-type-to-citation-form mapping (commands, functions, hardcoded values, branches → required `file://#Lx-Ly`) lives in `modules/04-repowiki-style.md` §Citation discipline. The read-before-cite rule (every `<cite>` entry must be an actually-opened file) also lives there.

## Claim confidence labels

Use these labels in audit reports, migration notes, and temporary review output when useful:

- `[source-confirmed]` — verified against current source, config, package scripts, CI, or runtime entrypoints.
- `[partially-verified]` — supported by some evidence, but scope, runtime registration, or environment behavior still needs checking.
- `[docs-only]` — found in existing docs or generated wiki but not yet verified against source.
- `[stale-or-conflicting]` — conflicts with current source/config or appears outdated.
- `[unverified]` — plausible but not backed by inspected evidence.

Avoid cluttering final human-facing docs with labels unless uncertainty matters for readers.

These labels describe **how trustworthy a written claim is** — distinct from the **verification level gradient** (`read-only` / `traced` / `executed` / `user-verified`) in `modules/03-agent-accuracy.md`, which describes **what the Agent actually did**. The two sets are complementary, not substitutes; see 03 §Verification level vs claim confidence label for how to pair them.
