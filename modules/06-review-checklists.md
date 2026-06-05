# 06 Review Checklists / 审查清单

Use this module before finishing any project knowledge system, repowiki generation, source-alignment audit, or documentation restructuring task.

> **Each checklist enforces rules defined elsewhere; this module is the enforcement layer, not the rule source.** When a check fails and you need the underlying rule:
>
> - Final review checklist → 01 (source priority, evidence levels, output scale, layering) and 02 (engineering fact inventory).
> - Source-alignment checklist → 01 §Source-of-truth priority and §Claim confidence labels.
> - Repowiki-style checklist → 04 (output location, citation discipline, multi-branch disambiguation, AI Agent index, freshness header).
> - AI Agent development-accuracy checklist → 03 (task routing source anchors, ≥2 neighbors, caller enumeration, verification level gradient, upgrade playbook).
> - Security and privacy checklist → 01 §Avoid unverified claims and SKILL Rule 1.
> - "What not to do" → SKILL Rules 1–8 and 01 (all core principles).
>
> Treat this index as the navigation back to authoritative wording when a reviewer asks "where does this rule come from?".

## How to run these checklists

Review against **actual generated content**, not intent.

1. After writing/editing files, Re-open each generated or modified file (Read tool, not from memory) and re-run the relevant checklist line by line.
2. For Mermaid blocks, citations, and `<cite>` lists, spot-check the referenced paths/lines against current source — not against the plan.
3. Self-evaluating from intent ("I meant to do X, so item X passes") will miss the most common failure modes: copied-wrong path, wrong line range, stale section title, broken cross-link, Mermaid node referencing wrong file.
4. If a check fails, fix the file before reporting done; do not file the failure as "follow-up" unless the user explicitly opts in.

## Final review checklist

Before finishing, verify:

- Does every recommended command exist?
- Are missing commands clearly marked as unavailable?
- Are framework versions current and source-confirmed?
- Are source paths real?
- Did you complete an engineering fact inventory for frontend, backend/API, data/storage, testing, CI/CD, deployment/ops, observability, security/privacy, and maintenance where relevant?
- Are absent or unverified domains explicitly marked instead of filled with generic conventions?
- Are generated docs proportional to project size?
- Are source citations specific enough for important claims?
- Are generated wiki claims verified against source rather than copied as authority?
- Does the doc distinguish source-confirmed behavior from assumptions?
- Are PWA/offline/native/test/CI/CD/ops/observability/backend claims backed by source?
- Does the AI Agent index reduce wiki-reading scope and let an Agent reach the smallest useful source entrypoint in 1–2 hops?
- Is a lightweight pointer to the canonical documentation entrypoint synced to project/reference memory when memory is available?
- Are canonical docs linked from the index?
- Does each canonical doc state authoritative sources and freshness triggers when useful?
- Did you avoid repeating the same long architecture explanation across multiple docs?
- Did you preserve human-readable encyclopedia sections?
- Did you separate AI quick paths from human background reading in long docs when useful?
- Did you avoid copying an entire project instruction file into the wiki?
- Did you ask for approval before multi-file writes or restructuring?
- Are private URLs, tokens, credentials, and internal secrets excluded?
- For development guidance, did you include impact analysis, high-impact areas, neighboring patterns, and verification paths?
- Did you avoid making source-code changes unless requested?
- If you edited the skill's own `README.md` or `SKILL.md`: does the README avoid duplicating SKILL.md's module index, request routing, glossary, or rule statements (and instead point to SKILL.md)?
- If you edited or added a module file: do `SKILL.md` §Module index and the README's "当前结构" file list both match the actual `modules/` directory contents?

## Source-alignment checklist

When checking existing docs or generated wiki against source:

- Compare documented scripts against actual package/build config.
- Compare documented architecture against current runtime entrypoints.
- Compare route/API/config descriptions against current config files.
- Compare testing/lint claims against real scripts and test directories.
- Compare CI/CD/deploy claims against workflows, build scripts, infra, or release docs.
- Compare monitoring/ops/security claims against real config, code, or external references supplied by the user.
- Downgrade or mark claims when only docs support them.
- Do not delete useful human docs just because some sections are stale; fix or mark the stale sections.

## Repowiki-style checklist

For generated or aligned repowiki-style docs:

- Is the output primarily Chinese when the user wants this repo's repowiki style?
- Does each canonical topic doc have a `<cite>` block?
- Does **every file** inside `<cite>` correspond to a file actually opened (Read) in this session? (read-before-cite)
- Does each canonical topic doc include `AI Agent 使用建议` (≤6 bullets, task-oriented)?
- Does each canonical topic doc carry the `> 最后基于源码核对：YYYY-MM-DD（commit 短哈希）` header?
- Does each canonical topic doc include a `## 未发现/未确认` section listing absent / partially-confirmed / out-of-scope items for that topic?
- **Citation discipline**: do all concrete claims (commands, npm scripts, exported functions, route rules, hardcoded values, version numbers, conditional branches) carry `file://path#Lx-Ly` line-range citations? Are file-only citations limited to module-level/overview claims?
- Are Mermaid diagrams backed by source and followed by `图表来源`?
- Do key sections include `章节来源` when claims are concrete?
- **Multi-branch toolchain**: if multiple runtime branches exist (e.g., dev vs packaging Node versions), are they listed in a table with anchor command + purpose rather than collapsed into a single claim?
- Is the output root correct: `Docs/repowiki/zh/content/` for full repowiki-style docs, or root `Docs/` for lightweight docs?
- Does the generated topic tree match the verified project type preset instead of blindly copying the dominos-mobile tree?
- Were unsupported or unverified topics omitted, or clearly marked as not found/unverified instead of creating empty fake directories?
- Is the canonical Agent index path correct: `Docs/repowiki/zh/content/AI-Agent索引.md` for full repowiki, or `Docs/AI-Agent索引.md` for lightweight Docs?
- Is the AI Agent index short and task-oriented?
- Is only the doc entrypoint/purpose/freshness reminder synced to memory, not the wiki body?
- Does the wiki preserve human explanations, diagrams, troubleshooting, conclusions, and appendices?
- Are large details moved into subtopics or appendices instead of duplicated?
- Are source-of-truth warnings and freshness rules present where useful?

## AI Agent development-accuracy checklist

For docs intended to help future Agents develop or maintain the project:

- Does the doc route common tasks to the smallest useful docs and source files?
- Does the task routing matrix carry a **source anchor** (concrete path or grep pattern) for each row, not just a generic doc reference?
- Does it tell Agents to verify **at least two** neighboring implementation patterns (not one), and how to handle divergent neighbors?
- Does it identify high-impact files/modules?
- Does it include change impact analysis guidance, including **caller enumeration** for shared symbols (grep, count, list, threshold)?
- Does it include the **verification level gradient** (read-only / traced / executed / user-verified) and prohibit claiming a higher level than actually performed?
- Does it include realistic verification commands or manual flows?
- For upgrade tasks, does it require per-version changelog reading, a compatibility matrix, and a rollback path?
- Does it tell Agents not to claim verification they did not run?
- Does it discourage unrelated modernization and broad refactors?
- Does it state known unverified behavior or absent evidence?

## Documentation reorganization checklist

Run this when the task moved, renamed, split, merged, or recategorized one or more docs under `Docs/`, repowiki, or legacy wiki trees. Skip when the task only edited content in place.

> Rule source: `modules/04-repowiki-style.md` §Documentation move and rename: index/link sync.

Pre-move:

- Did you grep for the old filename, old path, and old section anchors before moving?
- Did you list all reference surfaces (Agent index, canonical docs, subtopics, `CLAUDE.md`/`AGENTS.md`/`README.md`, memory)?
- For 5+ reference surfaces, did you produce a short plan before executing?

Move execution:

- Did you preserve filename and numeric prefixes when only the location changed?
- Are cross-doc links using relative paths within `Docs/`?
- Was the canonical Agent index updated first, before leaf docs?

Post-move verification (read the actual files, not the plan):

- Zero matches when grepping the project for the old filename and old path?
- Every reference to the new filename resolves to an existing file?
- Every updated link renders correctly (path + anchor)?
- `CLAUDE.md` task-oriented entrypoints, if they referenced the moved doc, point at the new location?
- Anchors referenced from other docs still exist (section heading not silently renamed)?

Stragglers and edge cases:

- If external references may still point at the old name, is there a redirect note or "Renamed/moved docs" line in the index?
- If a doc was retired (not replaced), is it listed under "Retired docs" so future Agents skip it?
- Did you avoid keeping the old path alive just because some reference still uses it?

Memory sync:

- Was the memory pointer updated **after** the new path was final?
- Was the existing memory entry edited in place (path/name only), not duplicated?
- If memory was unavailable, did the done statement record "memory sync pending"?

## Security and privacy checklist

- Do not include secrets, tokens, credentials, private keys, cookies, or session data.
- Do not publish internal private URLs unless the user explicitly provided them and they are safe to retain in project docs.
- Avoid copying sensitive external system content; link or describe at a high level when appropriate.
- For auth/security/privacy docs, distinguish current implementation from recommended improvements.
- Do not invent security controls. If scanning, encryption, RBAC, or audit logging is not found, say so.

## What not to do

- Do not rewrite the entire wiki unnecessarily.
- Do not delete generated docs just because they are verbose.
- Do not replace source verification with documentation assumptions.
- Do not cite generated docs as proof of source behavior.
- Do not create fake scripts, fake test status, fake CI status, or fake runtime capabilities.
- Do not claim “best practice” as current architecture unless the code actually follows it.
- Do not put large architecture snapshots into memory when they belong in docs.
- Do not store documentation snapshots in memory as a substitute for `Docs/`.
- Do not skip lightweight memory index sync when generated docs should guide future Agents and memory is available.
- Do not use memory to replace project docs or source verification.
- Do not make code changes as part of documentation alignment unless the user explicitly asks.

## Suggested commit message style

For documentation and knowledge-system work, use a clear documentation commit message, for example:

```text
[AI Docs] 建立项目知识系统与Agent导航

新增项目记忆规范、AI Agent协作入口和知识库导航，并对齐项目文档与当前源码事实。
```

Use the repository's actual commit style when available.
