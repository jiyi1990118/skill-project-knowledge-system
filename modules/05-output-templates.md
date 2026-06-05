# 05 Output Templates / 输出模板

Use this module when producing plans, audits, impact reports, source-alignment reviews, or fact inventories for user approval.

> **Defining rules live in other modules; this module only holds reusable templates.** When a template references a concept (engineering fact inventory, change impact, citation discipline, verification level, memory sync, etc.), the canonical rule lives in:
>
> - 01 (`modules/01-core-principles.md`): source-truth priority, evidence/claim labels, output scale, documentation layering.
> - 02 (`modules/02-project-discovery.md`): project type, engineering fact inventory.
> - 03 (`modules/03-agent-accuracy.md`): development task routing, change impact, caller enumeration, verification level gradient.
> - 04 (`modules/04-repowiki-style.md`): repowiki structure, citation discipline, multi-branch disambiguation, AI Agent index.
> - 07 (`modules/07-memory-policy.md`): memory scope verification and sync body templates.
>
> If a template and its defining module disagree, the defining module wins; update the template here.

## Knowledge-system plan template

Use this before multi-file writes, wiki migration, repowiki-style generation, or documentation restructuring.

```md
## 目标

- What knowledge system outcome is being built or aligned.

## 当前观察

- Existing docs / wiki / Agent instructions.
- Source/config files already checked.
- Known stale or missing areas.

## 项目事实盘点

| Domain | Present? | Verified evidence | Current fact | Unknown/gap | Suggested doc |
| --- | --- | --- | --- | --- | --- |
| Frontend | ... | ... | ... | ... | ... |
| Backend/API | ... | ... | ... | ... | ... |
| Data/storage | ... | ... | ... | ... | ... |
| Testing | ... | ... | ... | ... | ... |
| CI/CD | ... | ... | ... | ... | ... |
| Deployment/ops | ... | ... | ... | ... | ... |
| Observability | ... | ... | ... | ... | ... |
| Security/privacy | ... | ... | ... | ... | ... |
| Maintenance | ... | ... | ... | ... | ... |

## 拟新增文件

- `Docs/...` — purpose.

## 拟修改文件

- `CLAUDE.md` — purpose.
- `Docs/...` — purpose.

## canonical 文档入口

- Topic -> doc path.

## repowiki 输出结构

- 输出模式：轻量 `Docs/` / 完整 `Docs/repowiki/zh/content/` / 对齐已有文档。
- 项目类型 preset：Frontend SPA / Backend service / Full-stack web / Mobile-hybrid / Mini Program / Monorepo / Infra / Library / custom.
- 拟生成目录树：只列与源码/config 证据匹配的文件和目录。
- 不生成或降级描述的专题：列出缺少证据的 backend/test/CI/deploy/ops/monitoring/security/native 等领域。

## AI Agent 导航设计

- Canonical Agent index path: `Docs/repowiki/zh/content/AI-Agent索引.md` for full repowiki, or `Docs/AI-Agent索引.md` for lightweight Docs.
- Optional root pointer scope when both paths exist.
- Task routes and source entrypoints.
- Token-saving boundaries.

## 证据来源

- Source/config files to verify.
- Existing docs/wiki files to inspect as secondary input.

## 记忆同步

- 是否需要同步轻量文档索引到 project/reference memory。
- Memory scope check: confirm the memory belongs to the project being documented; skip if unavailable or ambiguous.
- Canonical Agent entrypoint: `Docs/repowiki/zh/content/AI-Agent索引.md` for full repowiki, or `Docs/AI-Agent索引.md` for lightweight Docs.
- Memory should store only pointer + purpose + freshness reminder, not doc body or architecture snapshot.

## 风险与边界

- What will not be rewritten or deleted.
- Claims needing conservative wording.
- Domains absent or unverified in inspected source/config.

## 验证方式

- Check real commands.
- Check links and source citations.
- Confirm docs are proportional to project size.
- Confirm no fake backend/test/CI/deploy/ops/monitoring/security claims.
- Confirm lightweight memory pointer is created when memory is available, or explicitly marked skipped.
```

## Documentation-system audit template

Use this when the user asks to analyze the documentation system without necessarily editing files.

```md
## 总体判断

- Healthy / usable with issues / needs restructuring.

## 已检查范围

- Docs inspected.
- Source/config inspected.
- Relevant but not inspected.
- Domains not assessed in this run; do not treat them as project facts.

## 已确认事实

- `[source-confirmed]` claim — evidence.

## 问题清单

| 优先级 | 问题 | 证据 | 建议 |
| --- | --- | --- | --- |
| High | ... | file/source | ... |

## 文档分层评估

| Layer | Current state | Issue | Recommendation |
| --- | --- | --- | --- |
| `CLAUDE.md` / `AGENTS.md` | ... | ... | ... |
| Canonical Agent index (`Docs/repowiki/zh/content/AI-Agent索引.md` or `Docs/AI-Agent索引.md`) | ... | ... | ... |
| Canonical docs | ... | ... | ... |
| Subtopic docs | ... | ... | ... |
| Memory policy | ... | ... | ... |

## 建议改动

- Must change.
- Should change.
- Optional enhancement.

## 不建议改动

- Existing useful docs/diagrams/sections to preserve.
```

## Change-impact report template

Use this when documenting how a proposed documentation or Agent-guidance change affects future development.

```md
## 变更目标

- What behavior or docs would change.

## 影响范围

- Direct files/modules.
- Callers/consumers (grep'd, counted, listed — not estimated).
- State/cache/persistence.
- Routes/API/build/runtime variants.
- Documentation/navigation/memory effects.

## 调用方枚举

- Symbol(s) being changed: ...
- grep command(s) used: ...
- Caller count: N
- High-risk callers (cross-domain, hot path, dynamic refs): ...

## 风险等级

- Low / Medium / High, with reason.
- Tie-in to caller count: 0–3 low, 4–10 medium, >10 or cross-domain high.

## 相邻模式

- At least two neighboring implementations checked: ...
- If neighbors diverge, how the pattern was disambiguated: ...

## 验证计划

- Commands or manual flows to run.
- Link/citation checks.
- Fallback if unavailable.

## 验证级别声明

For each verification claim, state the level reached:

- `read-only` — inspected source only
- `traced` — followed call graph across files
- `executed` — actually ran the command and observed output
- `user-verified` — human confirmed runtime behavior

Never claim higher than performed.

## 未验证事项

- Anything that must not be claimed as verified.
```

## Source-alignment report template

Use this when checking whether docs, README, generated wiki, or repowiki content matches current source.

```md
## 对齐范围

- Docs inspected.
- Source/config inspected.
- Generated wiki used as secondary input.

## 对齐结果

| 文档说法 | 可信度 | 当前源码事实 | 建议处理 |
| --- | --- | --- | --- |
| ... | `[stale-or-conflicting]` | ... | 修正/降级/删除 |

## 高风险过时点

- Scripts/commands.
- Runtime entrypoints.
- API/service behavior.
- Test/lint status.
- CI/CD/deploy/ops/monitoring/security claims.

## 后续动作

- Immediate fixes.
- Needs user decision.
- Can defer.
```

## Engineering fact inventory output template

Use this in plans or audit reports when the user asks for broad knowledge-system generation.

```md
## 工程事实盘点

| 领域 | 是否存在 | 已验证证据 | 当前事实 | 未确认/缺口 | 建议文档 |
| --- | --- | --- | --- | --- | --- |
| 前端 | 是/否/部分 | 源码/配置 | 框架、入口、路由、状态、构建 | ... | ... |
| 后端/API | 是/否/部分 | 源码/配置 | 服务入口、Controller、Service、接口契约 | ... | ... |
| 数据/存储 | 是/否/部分 | 源码/配置 | 数据库、migration、缓存、持久化 | ... | ... |
| 测试 | 是/否/部分 | 脚本/测试目录 | unit/integration/e2e/lint/typecheck 状态 | ... | ... |
| CI/CD | 是/否/部分 | workflow/脚本 | 构建、发布、部署链路 | ... | ... |
| 部署/运维 | 是/否/部分 | infra/runbook | 环境、回滚、健康检查 | ... | ... |
| 监控/日志 | 是/否/部分 | 日志/指标配置 | 日志、指标、链路追踪、告警 | ... | ... |
| 安全/隐私 | 是/否/部分 | 鉴权/配置 | 权限、密钥、敏感数据处理 | ... | ... |
| 维护边界 | 是/否/部分 | 文档/源码 | 高风险区域、升级说明、归属边界 | ... | ... |
```

## Project-level Agent instruction template

Use this when creating or updating `CLAUDE.md`, `AGENTS.md`, or equivalent project-level instructions.

```md
# AI Agent Instructions

## Runtime and toolchain

- Framework and major versions.
- Required runtime version.
- Package manager.
- Commands that actually exist.
- Commands that are commonly assumed but currently missing.

## Common commands

- install
- local development
- build
- test/lint, if actually available
- packaging/deploy, if relevant

## Big-picture architecture

- app entrypoint
- root shell
- routing model
- API/service layer
- state management model
- component/layout conventions
- plugin/runtime behavior
- build/runtime behavior

## Task-oriented entrypoints

Map common tasks to **concrete source anchors** (real paths or grep patterns) plus optional doc cross-references. Every row must point at a verifiable code location, not a generic doc reference.

| Task | Source anchor (path / grep) | Cross-ref doc | High-impact note |
| --- | --- | --- | --- |
| auth/login | `<auth-module-path>` | ... | ... |
| orders/checkout/payment | `<order-dir>` + `grep '<submitFn>'` | ... | high-impact |
| product/menu/cart | `<product-dir>` | ... | ... |
| routing/navigation/cache | `<router-config-dir>` + router plugin | ... | high-impact |
| API/request/env/signing | `<service-layer>` + `<api-map-dir>` | ... | high-impact |
| components/layout/UI | `<shared-components-dir>` | ... | ... |
| native/mobile/hybrid | `<native-bridge-dir>` + build config | ... | high-impact |
| build/deploy | `package.json` scripts + CI workflows | ... | high-impact |
| tracking/analytics | `<tracking-plugin>` + `grep '<eventName>'` | ... | ... |

Replace placeholders with real paths/symbols inspected from the actual project. Do not ship the table with generic placeholders.

## Development accuracy

- Prefer neighboring source patterns over generic best practices; verify **at least two** neighbors before generalizing.
- Enumerate callers (grep, count, list) for any shared symbol before changing it.
- Verify ownership, callers, state/cache effects, environment variants, and smallest safe change before implementation.
- State the verification level reached for each claim (`read-only` / `traced` / `executed` / `user-verified`); never upgrade the label without performing the action.
- Do not claim tests, builds, or manual verification ran unless they actually ran.

## High-impact areas

List files or modules where changes are cross-cutting and require extra caution.

## Verification strategy

Map common change areas to realistic verification commands or manual flows, and note fallbacks when verification is unavailable.

## Repository-specific notes

- stale README warnings
- generated docs caveats
- testing coverage caveats
- custom framework-like plugins
```

Rules:

- Keep project-level Agent instructions concise and operational.
- Do not copy an entire wiki into Agent instructions.
- Use exact commands from real config.
- Mention missing scripts explicitly.
- Prefer source paths, task routes, high-impact files, neighboring-pattern guidance, and verification strategy over long prose.

## Final response template

Use a short final response after editing documentation:

```md
已完成：
- ...
- ...

验证：
- ...

未验证/需注意：
- ...

记忆同步：
- 已同步到 project/reference memory: ...
- 或：memory 不可用/未配置，本次未同步。
```

Keep it concise. Do not paste large generated docs into the final response unless the user asked for full content.
