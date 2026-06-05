# 03 Agent Accuracy / AI Agent 开发准确性

Use this module when the knowledge system should help future AI Agents safely develop or maintain an existing project.

The goal is to document how to change code accurately, not just what the project is.

## Development task routing

Include a task-to-source routing matrix in `CLAUDE.md`, `AGENTS.md`, the canonical Agent index (`Docs/repowiki/zh/content/AI-Agent索引.md` for full repowiki, or `Docs/AI-Agent索引.md` for lightweight Docs), or optional `Docs/AI-Agent开发指南.md` when useful.

Each row must include a concrete source anchor (file path or grep pattern), not a generic doc reference. Anchors let future Agents skip wiki reading and jump straight to source.

| Task type | Source anchor (path / grep) | Read first | Then verify | Common risks |
| --- | --- | --- | --- | --- |
| Add page/view | route config dir + sibling page file | routing docs, neighboring pages | route config, layout, data fetching, navigation | wrong layout, cache mismatch, missing auth |
| Add API call | API map dir + `grep '<apiName>'` for callers | service/API docs, existing domain API map | endpoint config, caller pattern, error handling | bypassing service layer, duplicate API names |
| Change state | store dir + `grep '<storeName>'` for consumers | state docs, neighboring store/model | persistence, login/logout cleanup, consumers | stale cache, duplicated state source |
| Change component | component dir + `grep '<ComponentName>'` for usages | component docs, similar components | props/events/slots/styles/global registration | global side effects, style leakage |
| Change build/deploy | `package.json` scripts + CI workflow files | build docs, package/config/CI files | scripts, env vars, output dirs | fake commands, wrong environment |
| Change auth/security | auth middleware/guard files | auth/security docs, guards/middleware | token/session lifecycle, redirects, storage | weakening auth, leaking sensitive data |
| Change analytics | tracking plugin/dir + `grep '<eventName>'` | tracking docs, existing events | route metadata, trigger timing, duplication | double tracking, missing source code |

Rule: when generating this matrix for an actual project, replace placeholders with real paths discovered during source inspection. Do not ship the table with generic placeholders.

## Pre-development source checklist

Before implementing changes in an existing project, future Agents should verify:

- Which existing module owns this behavior?
- Is there a neighboring implementation to copy?
- Is there an existing service/API/store/component pattern?
- Which route/page/action triggers this behavior?
- Which state, cache, or persistence layer is affected?
- Which environment/build/runtime variants may behave differently?
- Which files are high-impact and require minimal edits?
- What is the smallest verifiable change?

## Change impact analysis

Teach future Agents to describe impact before code changes in existing projects.

> Output form: `modules/05-output-templates.md` §Change-impact report template (covers caller enumeration + verification level declaration). Review form: `modules/06-review-checklists.md` §AI Agent development-accuracy checklist.

Impact analysis should cover:

- Direct files/modules likely to change.
- Callers and consumers that may observe behavior changes.
- State, cache, persistence, and migration effects.
- Routing/navigation/auth/permission effects.
- API contracts, backend assumptions, and error behavior.
- Build/runtime/environment variants affected.
- User-visible flows and edge cases to test.
- Rollback or containment strategy for high-risk changes.

Risk rubric:

| Risk | Signals | Guidance |
| --- | --- | --- |
| Low | isolated copy/text/style/doc change | small direct edit, light verification |
| Medium | feature-local behavior, one domain, clear neighboring pattern | inspect callers and run targeted checks |
| High | cross-cutting service/router/auth/state/build/payment/security changes | plan first, minimize edits, verify with strongest available path |

## Verification strategy

Document realistic verification paths. Do not claim runtime verification was completed unless the command or manual flow actually ran.

| Change area | Preferred verification | Fallback if unavailable |
| --- | --- | --- |
| UI/page | run app and manually exercise golden path | inspect route + component render path and say not runtime-tested |
| API/service | run existing tests or local request flow | verify config and caller pattern, mark untested behavior |
| State/store | run affected UI flow or store tests | trace actions/mutations/consumers manually |
| Build/config | run target script | dry-read package/config and explain unverified risk |
| Docs only | check links, commands, source citations | note docs-only validation |

When generating Agent development guidance, derive verification examples from the actual project type and real scripts/configs:

- Frontend SPA: dev server, browser golden path, route/state/API flow checks.
- Backend service: unit/integration tests, local endpoint smoke checks, contract/schema checks.
- Mobile/hybrid: web debug path, native shell/device path, packaging script, bridge capability checks.
- Mini Program: platform devtools flow, page routing/config checks, platform API permission checks.
- Monorepo: affected package/workspace scripts, dependency graph, cross-package consumers.
- Infra/DevOps: plan/diff/dry-run, environment targeting, rollback path, secret boundary checks.
- Library/package: public API examples, build output, compatibility matrix, release artifact checks.

Only include examples that are supported by inspected source/config; otherwise mark them unavailable or not assessed.

## Verification level gradient

Agents and docs must not conflate reading code with running it. State the actual level reached, never claim higher.

| Level | What it means | When to use the label |
| --- | --- | --- |
| `read-only` | Inspected source/config files only; no execution | Most static doc generation, audits, and impact analyses |
| `traced` | Followed call graph, route, or data flow across files; still no execution | Cross-cutting impact reports, dependency analysis |
| `executed` | Ran a real command (test, build, script) and observed its output | Only when the command actually ran in this session |
| `user-verified` | A human confirmed runtime behavior in app/browser/device | Only after the user explicitly reports verification |

Rules:

- Default to `read-only` unless a stronger level is genuinely reached.
- Never upgrade the label based on confidence — only on actions performed.
- In the done statement, list the level per claim or area, not just at the top.
- If the user later reports a failure, downgrade affected claims and note the discrepancy.

### Read-only is the expected default for this skill

For the core scenarios of this skill (generating/aligning docs, audits, planning), `read-only` is the *correct terminal state* for almost every claim — documentation work inspects source but does not execute it. Do not feel pressured to upgrade the label or to run commands purely to claim a higher level. State `read-only` plainly and call out remaining `executed` / `user-verified` work as follow-up the user can perform.

Upgrade only when:

- The user explicitly asks for runtime verification.
- A command is needed to disambiguate a doc claim (e.g., does `npm run lint` actually exist).
- The skill task is reviewing an Agent guide whose recommended commands need confirmation.

### Verification level vs claim confidence label

Two label sets coexist; they describe different things and should not be substituted for each other:

| Label set | Where defined | Describes | Use in |
| --- | --- | --- | --- |
| Verification level gradient (`read-only` / `traced` / `executed` / `user-verified`) | this module | what the Agent actually did | done statements, change-impact reports, Agent-facing guidance |
| Claim confidence labels (`[source-confirmed]` / `[partially-verified]` / `[docs-only]` / `[stale-or-conflicting]` / `[unverified]`) | `modules/01-core-principles.md` | how trustworthy a written claim is | audit reports, source-alignment tables, temporary review output |

Pair them when needed: a claim can be `[source-confirmed]` while the verification level for that finding is `read-only` — that combination simply means "the source clearly says X; I inspected but did not execute." Do not collapse the two sets into one label.

## Caller and consumer verification

Before changing any shared symbol (API name, store action, exported component, public function), enumerate callers.

1. Grep the entire project for the symbol — do not estimate from memory or docs.
2. Count callers and list the files.
3. Risk threshold:
   - 0–3 callers: low risk if the change is local-compatible.
   - 4–10 callers: medium risk; inspect each caller for behavioral assumptions.
   - >10 callers, or callers across multiple domains: high risk; plan first and prefer additive changes.
4. If the symbol is dynamically referenced (string keys, reflection, route names), grep for the string form too.
5. Record the caller count and high-risk callers in the change-impact report.

## Maintenance task playbooks

For existing projects, add concise playbooks to `Docs/AI-Agent开发指南.md` or relevant canonical docs when useful.

Recommended playbook format:

```md
### <task type>

1. Read first: docs and source entrypoints.
2. Confirm ownership: module/package/team boundary when knowable.
3. Check neighboring pattern: similar implementation to copy.
4. Impact analysis: callers, state/cache, routes/API, build/runtime variants.
5. Implementation boundary: what not to refactor or modernize.
6. Verification: real command or manual flow; fallback if unavailable.
7. Done statement: what was verified and what remains unverified.
```

Common playbooks:

| Task | Must include |
| --- | --- |
| Fix bug | reproduction path, owning module, neighboring behavior, regression check |
| Add feature | route/API/state/component ownership, user flow, validation path |
| Change API | contract, caller list, backend assumptions, error behavior, compatibility |
| Change data/storage | schema/cache/persistence impact, migration/rollback, data safety |
| Add/fix tests | real test runner, fixture/mock pattern, test scope, flaky risks |
| Change build/CI | target script/workflow, env vars, artifacts, rollback |
| Deploy/release/rollback | release path, artifact source, environment, smoke checks |
| Investigate production issue | logs/metrics/tracing, health checks, recent deploy/config changes |
| Upgrade dependency/runtime | lockfile, compatibility notes, affected packages, verification matrix |
| Security/privacy change | auth/permission boundary, secrets, sensitive data, abuse cases |

### Upgrade playbook detail

Dependency, runtime, and framework upgrades touch large surface area and frequently break in non-obvious places. Expand the row above into:

1. Identify exact upgrade scope: package name(s), from-version, to-version, transitive ranges.
2. Read the upstream changelog or migration guide for **every** version between current and target — breaking changes often live in intermediate versions.
3. Build a compatibility matrix: peer dependencies, Node/runtime versions, framework versions, build tool versions. Confirm all rows are satisfiable.
4. Grep the project for symbols that the changelog flags as removed, renamed, or behavior-changed. Use `grep -rn` across source, not just docs.
5. Check for multiple toolchain branches (e.g., dev uses Node 14, packaging uses Node 20). Upgrade implications may differ per branch.
6. Identify the smallest upgrade increment that still resolves the driving requirement. Prefer minor over major; prefer one major step at a time.
7. Verification path:
   - Run install with frozen-lockfile to confirm dependency graph resolves.
   - Run build, tests, and lint if available; mark each as executed or unavailable.
   - For runtime upgrades, run dev server and smoke-test golden paths.
   - For framework upgrades, verify routing/state/SSR/build-output behavior, not just compile success.
8. Rollback strategy: keep the previous lockfile, document the revert command, confirm cache invalidation steps.
9. Done statement must list per-component verification level (read-only / traced / executed / user-verified) and call out any unverified runtime behavior.

## High-impact area detection

Mark files or modules as high-impact when they are:

- app/runtime entrypoints
- router/guards/middleware layers
- request/service clients
- auth/session/token handling
- payment/checkout/order submission
- global state or persistence
- build/deploy/CI scripts
- shared layout or globally registered components
- analytics/security/privacy infrastructure

High-impact guidance:

- Plan first for high-risk changes.
- Minimize edits.
- Avoid opportunistic refactors.
- Prefer strongest available verification path.
- State unverified behavior explicitly.

## Neighboring pattern first

For existing projects, prefer nearby source patterns over generic best practices.

Before proposing or implementing a change:

1. Find **at least two** neighboring features in the same domain. One example may be an anomaly; two or more confirm a real pattern.
2. If the two neighbors diverge, inspect a third or ask the user which pattern is current — do not pick arbitrarily.
3. Reuse the confirmed layout, service, state, validation, naming, and error-handling style.
4. Only introduce a new pattern when no suitable local pattern exists.
5. Do not modernize or refactor unrelated code just to match generic best practices.

## Optional AI Agent development guide

For medium or large existing projects, consider adding `Docs/AI-Agent开发指南.md` when the user wants future Agents to implement code more accurately.

Recommended sections:

```md
# AI Agent开发指南

## 开发前检查
## 常见任务路线
## 变更影响分析
## 高影响区域
## 相邻模式优先
## 验证策略
## 完成标准
## 不要做什么
## 变更后总结要求
```

This file should focus on safe code changes, while the canonical Agent index (`Docs/repowiki/zh/content/AI-Agent索引.md` for full repowiki, or `Docs/AI-Agent索引.md` for lightweight Docs) focuses on where to read.

## Definition of done for Agent-facing guidance

When writing development guidance, include a practical done standard:

- The affected ownership area is identified.
- Neighboring implementation patterns are checked.
- High-impact files are called out.
- Verification commands or manual flows are listed, or marked unavailable.
- Known unverified behavior is explicitly stated.
- No unrelated modernization or cleanup is recommended as part of normal feature work.

## Documentation freshness

When creating or updating project knowledge systems, include freshness rules so future Agents know when documentation must be re-verified.

Freshness guidance should cover:

- Source-backed claims expire when the referenced source file, package script, route/API config, build config, or runtime entrypoint changes.
- Generated wiki or repowiki pages should not be treated as current state unless their claims are re-checked against source.
- Canonical docs should state which files or config categories are authoritative for the topic.
- AI Agent indexes should route readers to source verification points, not only to other docs.
- If a doc has not been reviewed after major framework, routing, service, store, build, or deployment changes, mark affected claims conservatively.
- Do not preserve stale commands, obsolete architecture descriptions, or old test/lint status for compatibility; update or downgrade the claim.

## Done statement template

Use this style when documenting or finishing guidance work:

```md
## 已完成

- Updated/created: ...
- Source facts verified against: ...
- Agent navigation added/updated: ...

## 已验证

- Commands/links/source citations checked: ...

## 未验证或需人工确认

- Runtime behavior not executed: ...
- External systems not checked: ...
```
