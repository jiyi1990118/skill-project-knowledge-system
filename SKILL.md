---
name: skill-project-knowledge-system
description: Build and maintain a reusable project knowledge system, including memory rules, project memory definitions, human-facing docs or repowiki, and AI Agent navigation aligned with source truth.
---

# Project Knowledge System Builder / 项目知识系统构建师

Use this skill when the user wants to create, improve, review, or align a project knowledge system that serves both humans and AI Agents.

This skill covers four related outcomes:

1. Project memory rules and reusable memory definitions.
2. Project-level AI Agent instructions, such as `CLAUDE.md`, `AGENTS.md`, or equivalent files.
3. Human-facing `Docs/` knowledge base, including repowiki-style documents stored under the project root.
4. AI Agent navigation and documentation alignment with current source truth.

The goal is not only to generate documentation. The goal is to build a maintainable knowledge system that helps future humans and AI Agents understand the project faster, avoid stale assumptions, reduce token consumption, and make safer changes.

## When to use

Use this skill for requests like:

- “帮我给这个项目建立记忆规范”
- “帮我定义项目记忆、用户记忆、反馈记忆应该怎么写”
- “帮我在项目根目录生成 AI Agent 使用规范”
- “帮我在项目根目录 `Docs/` 下生成类似 repowiki 的项目知识库结构”
- “帮我让 repowiki 和项目现状对齐”
- “帮我优化项目文档，方便后续 AI Agent 开发”
- “帮我总结这个项目的架构并生成知识库入口”
- “帮我把自动生成百科改得更适合人和 AI 一起用”
- “帮我整理 Claude/Agent 使用这个项目的开发指南”
- “帮我检查文档里哪些地方和源码不一致”

Do not use this skill for ordinary feature implementation unless the task includes documentation, project memory, repowiki, or AI Agent knowledge-system work.

## When NOT to use

Decline or redirect when the request is actually about:

- Implementing business features, fixing runtime bugs, or shipping product changes.
- Large-scale refactors or architectural rewrites of source code.
- Generating npm scripts, test suites, CI pipelines, or build capabilities that do not exist in the project.
- Dumping entire source structure, file lists, or generated wiki bodies into memory.
- Replacing project documentation or source verification with memory recall.
- Treating an auto-generated wiki as proof of runtime behavior.
- Deleting existing human encyclopedia-style docs to make room for AI-only notes.
- Replacing the human knowledge base entirely with AI Agent navigation files.

If the user's request mixes documentation work with one of the above, isolate the documentation portion and surface the rest as out-of-scope before proceeding.

## Non-negotiable rules

Always apply these rules, even before reading optional modules:

1. Source code is the source of truth.
   - Use current source files, package scripts, build configs, route/API configs, runtime entrypoints, and CI files as authority.
   - Existing docs and generated wiki pages are secondary input and may be stale.
   - Repowiki-style output must be based on the current project's verified facts, not templates or assumptions.

2. Cover only real engineering norms.
   - Summarize frontend, backend/API, data/storage, testing, CI/CD, deployment, operations, monitoring, security, and maintenance norms only when source/config evidence supports them.
   - If a domain is absent or unverified, say so explicitly.

3. Preserve human knowledge-base value.
   - Do not collapse a useful wiki into a short AI-only note.
   - Preserve diagrams, explanations, troubleshooting, conclusions, and appendices when still useful.

4. Add token-saving Agent navigation.
   - Use task routing, canonical docs, and source verification entrypoints so future Agents avoid loading the whole wiki.
   - Canonical Agent index path depends on output mode: full repowiki uses `Docs/repowiki/zh/content/AI-Agent索引.md`; lightweight Docs uses `Docs/AI-Agent索引.md`; a root `Docs/AI-Agent索引.md` may be only a short pointer when full repowiki exists.
   - Keep indexes short and task-oriented.

5. Prefer narrow reading.
   - Read the smallest relevant docs/source files first.
   - Do not bulk-read a generated wiki unless the user explicitly asks for a broad audit.

6. Plan before multi-file writes.
   - For non-trivial documentation generation, wiki migration, or restructuring, produce a plan and wait for approval.
   - Small single-file edits can proceed directly when explicitly requested.

7. New knowledge-base output goes to root `Docs/` by default.
   - Concise project docs and Agent entrypoints go under `Docs/`.
   - Full repowiki-style topic encyclopedias default to `Docs/repowiki/zh/content/`.
   - Existing `.qoder/repowiki/`, `docs/`, or `wiki/` trees may be read as sources.
   - Do not create new output in legacy locations unless the user explicitly requests that location.

8. Sync a lightweight documentation index to memory when available.
   - After generating/aligning docs or Agent instructions, store a short pointer (entrypoint + purpose + freshness reminder) in the target project's memory; verify memory scope first.
   - Detailed rules and body templates: see `modules/07-memory-policy.md`.

## Module index

This skill is modular to reduce default token/context cost. Read only the modules needed for the current request.

| Module | Read when |
| --- | --- |
| `modules/01-core-principles.md` | Always for substantive tasks; contains source-truth, request routing, evidence, output scale, and layering rules. |
| `modules/02-project-discovery.md` | Analyzing project facts, identifying project type, creating fact inventory, or planning canonical docs. Includes code intelligence (codegraph) and library documentation verification (Context7) workflows. |
| `modules/03-agent-accuracy.md` | Creating guidance that helps future Agents safely develop or maintain an existing project. Includes code intelligence for impact analysis, caller enumeration, and Context7 for library API verification. |
| `modules/04-repowiki-style.md` | Creating or aligning `Docs/`, repowiki-style topic docs, AI Agent index, or canonical doc usage notes. |
| `modules/05-output-templates.md` | Producing plans, audits, source-alignment reports, impact reports, or reusable instruction templates. |
| `modules/06-review-checklists.md` | Final review before finishing documentation, repowiki, alignment, or knowledge-system work. |
| `modules/07-memory-policy.md` | Creating project memory rules, `AI-MEMORY.md`, or memory guidance. |

## Request routing

Choose the output mode based on the user's request instead of always generating files.

| User request | Primary output | Modules |
| --- | --- | --- |
| “分析这个项目文档体系” | Documentation-system audit report | 01, 02, 05, 06 |
| “生成类似 repowiki” | Knowledge-base plan + repowiki-style topic docs + memory index pointer | 01, 02, 04, 05, 06, 07 |
| “更新 CLAUDE.md / AGENTS.md” | Project-level Agent instructions + memory index pointer | 01, 02, 03, 05, 06, 07 |
| “建立 memory 规范” | Memory policy or `AI-MEMORY.md` | 01, 07, 05, 06 |
| “检查文档是否过时” | Source-alignment audit | 01, 02, 05, 06 |
| “学习这个 repowiki 风格” | Style summary or skill-improvement suggestion | 01, 04 |
| “让 AI Agent 开发已有项目更准确” | Agent development guide / task routing / safety guidance | 01, 02, 03, 05, 06 |
| “整理 / 迁移 / 重命名文档” | Reorganization plan + index/link sync + memory pointer update | 04, 06, 07 |

## Minimal workflow

1. Determine the user's requested output mode.
2. Read only the necessary module files (and only the relevant sections — see section-level routing below).
3. Inspect current project source/config/docs narrowly and record evidence boundaries.
4. For broad work, complete or summarize an engineering fact inventory.
5. Produce a plan first when the work affects multiple docs or restructures a wiki.
6. After approval, write or update files with source-backed claims only.
7. If docs or Agent instructions were generated/aligned, sync a lightweight pointer to the available project memory.
8. Run the review checklist before finishing.

## Section-level routing

Each request mode usually needs only specific module sections — not full modules. Prefer these entry sections, then expand only if needed.

| Mode | Read these sections first |
| --- | --- |
| Audit existing docs | 02 §Anti-hallucination gates, 02 §Engineering fact inventory, 05 §Documentation-system audit template, 06 §Source-alignment checklist |
| Generate repowiki | 02 §Code intelligence approach, 02 §Project type decision tree, 02 §Engineering fact inventory, 04 §Project-type repowiki presets, 04 §Repowiki-style document template, 04 §Preferred repowiki style, 05 §Knowledge-system plan template, 06 §Repowiki-style checklist |
| Update CLAUDE.md / AGENTS.md | 02 §Code intelligence approach, 02 §Standard workflow, 03 §Development task routing, 03 §Code intelligence for impact analysis, 03 §High-impact area detection, 05 §Project-level Agent instruction template |
| Build Agent development guide | 02 §Code intelligence approach, 03 (all), 05 §Change-impact report template, 06 §AI Agent development-accuracy checklist |
| Memory norms | 07 (all), 06 §Final review checklist (memory parts) |
| Source-alignment check | 01 §Source-of-truth priority, 02 §Anti-hallucination gates, 05 §Source-alignment report template, 06 §Source-alignment checklist |
| Reorganize / move / rename docs | 04 §Documentation move and rename: index/link sync, 06 §Documentation reorganization checklist, 07 §Scope verification |

Authoritative homes for cross-cutting rules: source-of-truth priority lives in `modules/01`; fact boundaries live in `modules/02`; final validation lives in `modules/06`; memory sync details live in `modules/07`; path/index sync after doc moves lives in `modules/04` §Documentation move and rename.

## Glossary

Terms used across modules. Definitions live here so the meaning does not drift as modules grow.

| Term | Meaning |
| --- | --- |
| Knowledge system | The combined output this skill produces: project docs + AI Agent navigation + lightweight memory pointer. |
| Canonical doc | The authoritative doc for one topic; other docs link to it instead of repeating its content. |
| Topic doc | A repowiki-style专题文档. Often serves as the canonical doc for that topic. |
| Subtopic doc | A doc that expands one part of a topic doc; lives one level deeper. |
| AI Agent index | The navigation file at `Docs/AI-Agent索引.md` (lightweight) or `Docs/repowiki/zh/content/AI-Agent索引.md` (full repowiki). |
| Engineering fact inventory | The fact matrix defined in `modules/02-project-discovery.md` §Engineering fact inventory. |
| Repowiki-style output | The Chinese topic-encyclopedia style defined in `modules/04-repowiki-style.md` §Preferred repowiki style. |
| Source anchor | A concrete file path, codegraph query, or grep pattern, not a doc reference. Required in task routing rows. |
| Code intelligence | Semantic code analysis using codegraph MCP server (faster, more accurate than grep). |
| Codegraph-first | Workflow pattern: check codegraph availability, use it when present, fall back to grep/Read only when needed. |
| Library documentation verification | Using Context7 MCP to query current library/framework documentation instead of relying on training data. |
| Context7-verified | Confidence label: pattern confirmed against current library documentation via Context7 query. |
| version-specific | Confidence label: pattern valid for detected library version, may differ across versions. |
| docs-unavailable | Confidence label: Context7 returned no results; fell back to code inspection or training data. |
| Knowledge graph | Optional persistent project structure model built with Memory MCP (entities + relations). |
| Entity type | Classification for knowledge graph nodes: Module, Component, Service, Route, API, Store, Config, Test. |
| Relation type | Classification for knowledge graph edges: depends-on, calls, uses, renders, contains, imports. |
| Read-before-cite | Every file inside a `<cite>` block must have been opened (Read tool) in the current session. |
| Defining module / Enforcement module | The module that owns a rule (e.g. 04 owns citation discipline) vs. the module that checks it at review time (e.g. 06). |

## Module dependency map

Maintainer reference. When you change a defining module, update the enforcement/template modules that pull from it.

| Defining module owns | Enforcement / template modules referencing it |
| --- | --- |
| 01 §Source-of-truth priority, §Evidence citation levels, §Claim confidence labels | 03 §Verification level vs claim confidence label; 04 §Citation discipline; 06 §Final review, §Source-alignment checklist |
| 02 §Engineering fact inventory, §Anti-hallucination gates | 05 §Engineering fact inventory output template; 06 §Final review checklist |
| 03 §Verification level gradient, §Change impact analysis | 05 §Change-impact report template; 06 §AI Agent development-accuracy checklist |
| 04 §Repowiki output location, §Citation discipline, §AI Agent index, §Documentation move and rename | 05 §Knowledge-system plan template; 06 §Repowiki-style checklist, §Documentation reorganization checklist |
| 07 §Scope verification, §Recommended memory types | SKILL Rule 8; 01 §Sync documentation entrypoints to memory; 04 §Memory sync after docs generation; 06 §Final review checklist (memory parts) |

When in doubt: rules live in 01–04 + 07; templates live in 05; checklists live in 06. Touch the defining module first; let 05/06 follow.

## Heading-language convention

Module headings switch language by purpose. **Do not "unify" them** — the mix is intentional.

- **English headings** (`## Citation discipline`, `## Verification level gradient`) — skill-internal rules, read by AI Agents executing the skill.
- **Chinese headings** (`## 已完成`, `## 任务入口速查`, `## AI Agent 使用建议`) — template body meant to be copied verbatim into the target project's documentation.

If you find yourself translating one set to match the other, stop. The English rule layer and the Chinese template layer serve different readers.
