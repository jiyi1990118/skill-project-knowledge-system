[English](README.md) | [中文](README.zh.md)

# Skill Project Knowledge System / 项目知识系统构建师

> This README is for skill maintainers and human readers — it is not the runtime entry point.
>
> - Skill runtime entry: [`SKILL.md`](./SKILL.md)
> - Modules: [`modules/`](./modules/)
> - Runtime rules (request routing, module index, glossary, hard rules, minimum workflow) all live in `SKILL.md` and are deliberately not repeated here.

This skill builds or optimizes a "project knowledge system" that serves both human developers and the AI Agents that work on the project.

When generating a repowiki-style knowledge base, you must ground every statement in the project's real source code, configuration, scripts, and runtime entry points. Do not borrow from other project templates, stale READMEs, or unverified auto-generated content. The output should help future AI Agents develop and maintain the project more accurately, while reducing token consumption through task navigation, canonical docs, and source-verified entry points.

## MCP Integration (Optional)

The skill integrates three MCP servers to improve efficiency and accuracy. All MCPs are optional enhancements and fall back gracefully when unavailable.

- **Codegraph MCP** — semantic code analysis (symbol search, call relationships, impact analysis).
- **Context7 MCP** — real-time library documentation lookup (saves 10–50K tokens per query, avoids outdated APIs).
- **Memory MCP** — knowledge graph construction (structured navigation for large projects; saves hundreds of K tokens over time).

See `SKILL.md` §Module index and `modules/02-project-discovery.md` for implementation details.

## Current Structure

```text
skill-project-knowledge-system/
├── SKILL.md          # Runtime entry: trigger conditions, hard rules, glossary, module index, minimum workflow
├── README.md         # Maintainer entry (this file)
├── README.zh.md      # Chinese version of this file
└── modules/
    ├── 01-core-principles.md
    ├── 02-project-discovery.md
    ├── 03-agent-accuracy.md
    ├── 04-repowiki-style.md
    ├── 05-output-templates.md
    ├── 06-review-checklists.md
    └── 07-memory-policy.md
```

For each module's purpose, when to read it, and section-level routing, see `SKILL.md` §Module index and §Section-level routing. Do not repeat them in this README.

## When to Use

Use this skill when you want to do any of the following on a new or existing project:

- Create project-level AI Agent instructions such as `CLAUDE.md` / `AGENTS.md`.
- Create project memory rules such as `AI-MEMORY.md`.
- Generate or organize documentation under the project's root `Docs/`.
- Generate or optimize a repowiki-style knowledge base under `Docs/`.
- Add an `AI-Agent索引.md` entry point to large documents.
- Add "AI Agent usage notes" to canonical documents.
- Add an `AI-Agent开发指南.md` to existing projects.
- Check whether the README, docs, or repowiki disagree with the current source code.
- Refactor existing docs into a dual-purpose "human knowledge base + AI Agent navigation" structure.
- Reorganize, migrate, or rename existing docs and keep indexes, links, and memory pointers in sync.

## Recommended Usage

After launching Claude Code in the target project, you can say:

```text
Please use the skill-project-knowledge-system skill to analyze the current project and build a project knowledge system.

Goals:
1. Generate project memory rules
2. Generate project-level AI Agent collaboration notes
3. Plan the project root `Docs/` and repowiki-style knowledge base structure, then fully build the documentation
4. Add AI Agent task navigation entry points
5. Verify whether existing documentation matches source-code facts

Requirements:
- Ground everything in source code and real configuration
- Do not make assumptions from READMEs or old docs
- Do not generate script commands that do not exist
- Preserve the human knowledge base while making AI Agent development faster and safer
```

If the project already has a repowiki:

```text
Please use the skill-project-knowledge-system skill to align the project's repowiki with the current source code, output the aligned knowledge base to the project root `Docs/`, and add an AI Agent navigation layer.

Focus:
- Do not rewrite the entire repowiki
- Keep the auto-generated encyclopedia structure
- Place newly created or aligned repowiki-style documents under `Docs/`
- Add `Docs/AI-Agent索引.md`
- Add "AI Agent usage notes" to canonical documents
- Verify that package scripts, architecture descriptions, test/lint status, and build/deploy instructions are accurate
```

If the project has no AI documentation yet:

```text
Please use the skill-project-knowledge-system skill to build an AI-Agent-usable project knowledge system from scratch.

Please analyze the source code and configuration first, then suggest and generate:
- CLAUDE.md or AGENTS.md
- AI-MEMORY.md
- Docs/AI-Agent索引.md
- Docs/项目概述.md
- Docs/开发者指南.md

Give a plan first, wait for my confirmation, then write the files.
```

## Common Output Files

Depending on the project, this skill may generate or update the following files:

```text
CLAUDE.md
AGENTS.md
AI-MEMORY.md
Docs/README.md
Docs/AI-MEMORY.md
Docs/AI-Agent索引.md
Docs/AI-Agent开发指南.md
Docs/项目概述.md
Docs/开发者指南.md
Docs/<topic-dir>/<topic>.md
```

Not every project needs all of these. For simple project docs and Agent entry points, default to placing them under the project root `Docs/`; for full repowiki-style topic encyclopedias, default to placing them under `Docs/repowiki/zh/content/`. Existing `.qoder/repowiki/`, `docs/`, or `wiki/` may be used as sources for read / alignment / migration, but not as new output locations unless the user explicitly requests.

## Module Maintenance Rules

When maintaining this skill:

- Keep `SKILL.md` short and operational: only triggers, hard rules, glossary, module index, minimum flow, and section-level routing.
- Put long specs, templates, and checklists into `modules/`.
- When adding a new capability, first decide which module it belongs to; do not stuff long content back into `SKILL.md`.
- If you add a new module, update `SKILL.md` §Module index and the module list in the "Current Structure" section of this README.
- Module content must also follow "source-of-truth first, never invent nonexistent capabilities".
- When changing a section title in any module, grep the project for references and update them in sync (see `modules/04-repowiki-style.md` §Documentation move and rename).
- Do not repeat `SKILL.md`'s request-routing table, module-description table, or rule body in this README. This README is only for maintainers / humans; point back to `SKILL.md` for rules.

## What This Skill Is Not For

Do not use this skill to:

- Directly implement business features
- Perform large-scale code refactors
- Generate nonexistent tests or build capabilities
- Dump the entire source structure into memory
- Replace project documentation or source verification with memory recall
- Treat an auto-generated wiki as proof of runtime behavior
- Delete existing human encyclopedia-style documentation
- Completely replace the human knowledge base with AI Agent documents

## Current Skill File Paths

```text
~/.claude/skills/skill-project-knowledge-system/SKILL.md
~/.claude/skills/skill-project-knowledge-system/modules/
```
