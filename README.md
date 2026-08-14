# ask-core skills

面向工程 Agent 的高可信能力仓库，可直接通过 [`skills`](https://skills.sh/) CLI 安装。

## 可用 Skill

| Skill | 用途 |
| --- | --- |
| `context-continuum` | 为编码 Agent 提供跨会话延续的项目智能，沉淀决策、流程、规则与陷阱。 |

## 安装

列出仓库中的 Skill：

```bash
npx skills add https://github.com/ask-core/skills --list
```

安装 `context-continuum`：

```bash
npx skills add https://github.com/ask-core/skills --skill context-continuum
```

全局安装并跳过确认：

```bash
npx skills add https://github.com/ask-core/skills --skill context-continuum --global --yes
```

`context-continuum` 需要 Node.js 22.13 或更高版本。Skill 自带经过校验的知识运行时，不需要另外安装任何 npm 包。

## 使用

安装完成后，在目标项目中通过兼容的编码 Agent 使用 `$context-continuum`。

### 1. 初始化项目知识库

首次使用时明确要求初始化：

```text
使用 $context-continuum 为当前项目初始化知识库。
```

初始化是显式操作。仅安装 Skill 或开始普通任务不会自动创建项目文件。

### 2. 开始任务前恢复知识

```text
使用 $context-continuum，先恢复与“重构工作流执行器”相关的项目知识，再开始工作。
```

支持隐式调用的 Agent 也会在项目任务开始时主动恢复相关知识。读取操作不会修改知识库。

### 3. 完成任务后沉淀知识

```text
使用 $context-continuum，总结这次实现中可复用的决策、流程和陷阱，并更新项目知识。
```

只有用户明确要求“记住”“总结学习”“沉淀经验”或“更新项目知识”时，Skill 才会写入知识库。普通任务完成、对话总结或 Agent 自己发现了有用信息，都不会触发写入。

## 会产生哪些文件

### 安装 Skill

项目级安装通常会产生：

```text
.agents/skills/context-continuum/   # Skill 的自包含文件
skills-lock.json                    # 安装来源和版本锁定信息
```

具体安装目录会随目标 Agent 和是否使用 `--global` 而变化。全局安装保存在用户级 Skill 目录，不会在每个项目中重复复制完整运行时。

### 初始化项目知识库

初始化及后续使用会在项目根目录形成：

```text
.memory/
├── config.json                     # 存储格式版本和创建时间
├── memories/
│   └── mem_<ULID>.md               # 首次沉淀后生成的项目知识源文件
├── history/
│   └── refinements.jsonl           # 追加写入的变更历史
└── index.sqlite                    # 首次检索后生成的全文搜索索引
```

初始化本身只创建配置、空的 `memories/` 目录和历史文件。写入期间可能短暂出现 `.memory/.refine.lock` 锁目录；操作完成后会自动移除。

### 是否提交到 Git

团队需要共享项目知识时，建议提交以下内容：

```text
.memory/config.json
.memory/memories/
.memory/history/
```

搜索索引和临时锁无需提交，可加入 `.gitignore`：

```gitignore
.memory/index.sqlite
.memory/.refine.lock
```

如果知识库仅供个人本地使用，可以忽略整个 `.memory/` 目录。

## 仓库布局

每个 Skill 都直接位于仓库根目录，目录名与 `SKILL.md` 中的 `name` 一致：

```text
context-continuum/
├── SKILL.md
├── agents/
├── assets/
├── references/
└── scripts/
```

每个 Skill 均以自包含形式发布，可直接安装使用，无需额外配置开发源码、测试环境或构建工具链。

## License

[MIT](LICENSE)
