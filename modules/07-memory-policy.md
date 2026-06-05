# 07 Memory Policy / 记忆规范

Use this module when the user asks to create project memory norms, memory guides, or reusable memory definitions.

> **Invoked from these authoritative homes** (this module holds the body templates and scope-verification steps they reference):
>
> - SKILL Rule 8 (memory sync after docs generation) — calls into §Scope verification and §Recommended memory types.
> - 01 §Sync documentation entrypoints to memory — short statement of the rule; full detail lives here.
> - 04 §Memory sync after docs generation — repowiki-specific application of the same rule.
>
> Keep wording consistent across these references; if the rule changes, update SKILL Rule 8 and the two short pointers in 01/04 in the same commit.

## Memory vs documentation

Memory is for durable, non-obvious context that helps future conversations. Documentation is for project facts, architecture, workflows, commands, and implementation guidance.

Do not put large architecture snapshots, file lists, or generated wiki summaries into memory. Put those in project docs instead.

Do put a lightweight pointer in memory when this helps future Agents find the canonical docs quickly. The pointer should identify the documentation entrypoint and its purpose, not duplicate the docs.

## Documentation index memory sync

After generating or aligning a project knowledge system, repowiki-style docs, `Docs/AI-Agent索引.md`, `Docs/repowiki/zh/content/AI-Agent索引.md`, `CLAUDE.md`, `AGENTS.md`, or `AI-MEMORY.md`, update the available project memory with a concise documentation index pointer when memory is available.

### Scope verification (run before writing)

Memory is keyed to a specific project/working directory. Writing the wrong scope pollutes an unrelated project's memory and is hard to detect later.

Before syncing, confirm all three:

1. The active memory scope matches the project being documented — not this skill repository, not a parent directory, not a sibling project opened in the same session.
2. The canonical entrypoint path (`Docs/AI-Agent索引.md` or `Docs/repowiki/zh/content/AI-Agent索引.md`) exists under the project being documented, not under a different cwd.
3. The memory system is actually available in this environment (not disabled, not read-only, not pointing at the wrong project).

If any check fails or scope is ambiguous:

- Skip the sync.
- State in the final response: "documentation created/aligned; memory sync skipped because <reason>."
- Do not retry against a different scope without user confirmation.

If the user is working on multiple projects in one session, ask which project's memory should receive the pointer rather than guessing.

Use this for:

- remembering that the project has a source-backed knowledge system
- pointing future Agents to the canonical entrypoint: `Docs/repowiki/zh/content/AI-Agent索引.md` for full repowiki, or `Docs/AI-Agent索引.md` for lightweight Docs
- noting which docs are authoritative navigation layers versus human background docs
- reminding future Agents that source/config remains the authority and docs require re-verification after relevant source changes

Do not use memory sync for:

- full generated document contents
- architecture snapshots
- exhaustive file lists
- route/API/store/component inventories
- generated wiki summaries
- stale source facts that should be read from current code

Recommended memory type:

- `project` memory when the docs live in this project and should guide future work in this repo.
- `reference` memory when the docs live in an external system or shared documentation location.

Recommended memory body for lightweight Docs:

```md
This project has a source-backed AI/project knowledge system. Canonical Agent entrypoint: `Docs/AI-Agent索引.md`; detailed development guidance: `Docs/AI-Agent开发指南.md` when present; topic docs live under `Docs/`.

**Why:** Future Agents should start from the task index instead of reading the whole wiki or relying on stale README/generated docs.
**How to apply:** For future development tasks, open the Agent index first, follow the task route to the smallest canonical doc/source entrypoint, then verify claims against current source/config before editing.
```

Recommended memory body for full repowiki:

```md
This project has source-backed repowiki-style docs. Canonical Agent entrypoint: `Docs/repowiki/zh/content/AI-Agent索引.md`; topic docs live under `Docs/repowiki/zh/content/`; a root `Docs/AI-Agent索引.md` may exist only as a short pointer.

**Why:** Future Agents should start from the canonical task index instead of loading the whole wiki or relying on stale generated/background docs.
**How to apply:** Open the Agent index first, follow the task route to the smallest canonical topic/source entrypoint, then verify claims against current source/config before editing.
```

If memory tooling or storage is unavailable, say explicitly that docs were created/aligned but memory sync was not performed.

## Recommended memory types

### User memory

Purpose: durable information about the user’s role, preferences, responsibilities, and collaboration style.

Save when:

- the user explains their role or expertise
- the user gives durable collaboration preferences
- the user repeatedly works in a specific project/domain context

Do not save:

- negative judgments
- transient task state
- secrets or credentials
- information already obvious from current repo files

### Feedback memory

Purpose: durable guidance about how the user wants the Agent to work.

Save when:

- the user corrects the Agent’s approach
- the user says to avoid or prefer a workflow
- the user confirms a non-obvious approach worked well

Recommended structure:

```md
Rule or preference.

**Why:** Reason or incident behind the preference.
**How to apply:** When this preference should affect future work.
```

### Project memory

Purpose: non-obvious project context not derivable from code or git history.

Save when:

- the user explains why a project decision exists
- there is a deadline, release freeze, compliance reason, stakeholder constraint, or migration context
- there is temporary but important initiative context

Do not save:

- code architecture that can be read from source
- exhaustive file paths as a substitute for documentation
- recent git changes
- fix recipes already captured by code or commits

Good project memory exception:

- a short pointer to canonical project docs, such as `Docs/AI-Agent索引.md`, plus why and how future Agents should use it

Recommended structure:

```md
Project fact or decision.

**Why:** Motivation, constraint, deadline, or stakeholder reason.
**How to apply:** How this should affect future suggestions.
```

### Reference memory

Purpose: pointers to external systems where current information lives.

Save when:

- bugs are tracked in a specific external project
- docs live in a known external location
- dashboards, Slack channels, dashboards, or issue trackers matter for future tasks

Do not save:

- secrets
- private URLs unless the user explicitly provided them and they are safe to retain
- copied content from external systems when a pointer is enough

## Project memory guide output

If the project should contain a reusable memory guide, create a short file such as:

- `Docs/AI-MEMORY.md` at the project root
- `AI-MEMORY.md` at the project root, if the user wants the guide at top level
- `.claude/memory-guide.md`, only if the project already standardizes on a `.claude/` directory

Recommended template:

```md
# AI Memory Guide

This project uses memory to preserve durable, non-obvious context across AI Agent sessions.

## What memory is for

- User collaboration preferences.
- Durable feedback about how Agents should work.
- Project context that is not obvious from code or git history.
- References to external systems where current information lives.

## What memory is not for

- Architecture snapshots that can be derived from source.
- Full documentation snapshots or generated wiki summaries.
- Exhaustive file lists or recent git changes.
- Temporary task state.
- Secrets, tokens, credentials, or private personal information.
- Bug fix recipes already captured in code or commits.

## Documentation index memory

When project docs are generated or aligned, store only a short memory pointer to the canonical documentation entrypoint, such as `Docs/AI-Agent索引.md` for lightweight Docs or `Docs/repowiki/zh/content/AI-Agent索引.md` for full repowiki, and remind future Agents to verify docs against source before editing.

## Memory types

### user

Stores durable information about the user’s role, goals, responsibilities, and preferences.

### feedback

Stores durable guidance about how Agents should approach work.

Use:

`Rule. Why. How to apply.`

### project

Stores non-obvious project context, decisions, constraints, and deadlines.

Use:

`Fact or decision. Why. How to apply.`

### reference

Stores pointers to external systems and where to find current information.

## Before using memory

- Verify code/file/function claims against current source.
- If memory conflicts with source, trust source and update stale memory.
- Do not use memory if the user says to ignore it.
```

## Before using memory in recommendations

A memory that names a file, function, flag, or process may be stale.

Before recommending from memory:

- If memory names a file path, check the file exists.
- If memory names a function, config flag, command, or script, verify it exists in current source/config.
- If memory summarizes repo state, treat it as frozen in time and prefer current source or git history for recent/current state.
- If memory conflicts with current source, trust source and update or remove stale memory.

## What not to save

Do not save:

- code patterns, conventions, architecture, file paths, or project structure that can be derived from source
- git history, recent changes, or who-changed-what
- debugging solutions or fix recipes captured by code or commits
- activity logs or PR lists unless the surprising/non-obvious lesson is identified
- secrets, credentials, tokens, cookies, private keys, session data
- temporary task state

## Memory documentation checklist

When creating memory docs:

- Does it distinguish memory from project documentation?
- Does it explain user/feedback/project/reference memory?
- Does feedback/project memory include `Why` and `How to apply`?
- Does it warn against storing source-derivable architecture snapshots?
- Does it require verifying memory against current source before acting?
- Does it explain when to sync a lightweight documentation index pointer to memory?
- Does it exclude full doc snapshots, generated wiki summaries, secrets, and private data?
