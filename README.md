# Project Knowledge System Builder / 项目知识系统构建师

> 本 README 面向 skill 维护者与人类读者，不是 skill 的运行时入口。
>
> - Skill 运行入口：[`SKILL.md`](./SKILL.md)
> - 模块目录：[`modules/`](./modules/)
> - 运行时规则（请求分流、模块索引、术语表、不可违反规则、最小工作流）全部在 `SKILL.md` 内，不在本文件复述。

这个 skill 用于给一个项目建立或优化"项目知识系统"，让项目同时适合人类开发者阅读，也适合 AI Agent 后续快速、安全地开发。

生成 repowiki-style 知识库时，必须基于当前项目的真实源码、配置、脚本和运行入口；不能套用其它项目模板、旧 README 或未经验证的自动生成内容。输出应让未来 AI Agent 能更准确地开发和维护项目，同时通过任务导航、canonical 文档和源码验证入口减少不必要的 token 消耗。

## 当前结构

```text
project-knowledge-system-builder/
├── SKILL.md          # 运行入口：触发条件、非违规则、术语表、模块索引、最小工作流
├── README.md         # 维护者入口（本文件）
└── modules/
    ├── 01-core-principles.md
    ├── 02-project-discovery.md
    ├── 03-agent-accuracy.md
    ├── 04-repowiki-style.md
    ├── 05-output-templates.md
    ├── 06-review-checklists.md
    └── 07-memory-policy.md
```

每个模块的用途、何时读取、章节级路由请看 `SKILL.md` §Module index 和 §Section-level routing；不要在本 README 复述。

## 适合什么时候用

当你在新项目或已有项目中想做这些事情时使用：

- 建立类似 `CLAUDE.md` / `AGENTS.md` 的项目级 AI Agent 说明
- 建立类似 `AI-MEMORY.md` 的记忆规范
- 在项目根目录 `Docs/` 下生成或整理文档结构
- 在项目根目录 `Docs/` 下生成或优化 repowiki-style 知识库
- 给大型文档增加 `AI-Agent索引.md`
- 给 canonical 文档增加 `AI Agent 使用建议`
- 给已有项目补充 `AI-Agent开发指南.md`
- 检查 README / docs / repowiki 中是否有与源码不一致的内容
- 把已有文档改造成"人类知识库 + AI Agent 导航层"的双用途结构
- 整理、迁移、重命名已有文档并同步索引/链接/记忆指针

## 推荐使用方式

在目标项目中启动 Claude Code 后，可以直接这样说：

```text
请使用 project-knowledge-system-builder skill，帮我分析当前项目，并建立一套项目知识系统。

目标：
1. 生成项目记忆规范
2. 生成项目级 AI Agent 协作说明
3. 规划项目根目录 `Docs/` 和 repowiki-style 知识库结构及文档完整构建
4. 增加 AI Agent 任务导航入口
5. 检查现有文档是否和源码事实不一致

要求：
- 以源码和真实配置为准
- 不要凭 README 或旧文档做假设
- 不要生成不存在的脚本命令
- 保留人类知识库价值，同时让 AI Agent 后续开发更快更安全
```

如果项目已经有 repowiki，可以说：

```text
请使用 project-knowledge-system-builder skill，帮我让当前项目的 repowiki 与源码现状对齐，并把对齐后的知识库输出到项目根目录 Docs/ 下，同时补充 AI Agent 导航层。

重点：
- 不重写整个 repowiki
- 保留自动生成百科结构
- 将新建或对齐后的 repowiki-style 文档放到项目根目录 Docs/ 下
- 新增 Docs/AI-Agent索引.md
- 给 canonical 文档添加 AI Agent 使用建议
- 校验 package scripts、架构描述、测试/lint 状态、构建/部署说明是否准确
```

如果项目还没有任何 AI 文档，可以说：

```text
请使用 project-knowledge-system-builder skill，从零为当前项目建立 AI Agent 可用的项目知识系统。

请先分析源码和配置，然后建议并生成：
- CLAUDE.md 或 AGENTS.md
- AI-MEMORY.md
- Docs/AI-Agent索引.md
- Docs/项目概述.md
- Docs/开发者指南.md

先给计划，等我确认后再写文件。
```

## 常见输出文件

根据项目情况，这个 skill 可能生成或更新这些文件：

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
Docs/<专题目录>/相关专题.md
```

不一定每个项目都需要全部文件。简洁项目文档和 Agent 入口默认放在项目根目录 `Docs/` 下；完整 repowiki-style 专题百科默认放在 `Docs/repowiki/zh/content/` 下。已有 `.qoder/repowiki/`、`docs/` 或 `wiki/` 可作为读取、对齐或迁移来源，但不要作为新建输出位置，除非用户明确要求。

## 模块维护规范

维护这个 skill 时：

- `SKILL.md` 保持短而操作化，只放触发、硬规则、术语表、模块索引、最小流程、章节级路由。
- 大段规范、模板、清单放入 `modules/`。
- 新增能力时优先判断属于哪个模块，不要把长正文重新塞回 `SKILL.md`。
- 如果新增模块，必须同步更新 `SKILL.md` §Module index 和本 README 当前结构小节中的模块清单。
- 模块内容也要遵守"源码事实优先、不要制造不存在能力"的原则。
- 修改任何模块的章节名时，grep 项目内引用并同步更新（见 `modules/04-repowiki-style.md` §Documentation move and rename）。
- README 不要复述 `SKILL.md` 的请求分流表、模块说明表、规则正文；只放维护者/人类入口信息，规则点回 `SKILL.md`。

## 不适合做什么

不要用这个 skill 来：

- 直接实现业务功能
- 大规模重构代码
- 生成不存在的测试或构建能力
- 把所有源码结构塞进 memory
- 用 memory 替代项目文档或源码验证
- 把自动生成 wiki 当作源码行为证据
- 删除已有百科式文档
- 用 AI Agent 文档完全替代人类知识库

## 当前 skill 文件路径

```text
~/.claude/skills/project-knowledge-system-builder/SKILL.md
~/.claude/skills/project-knowledge-system-builder/modules/
```
