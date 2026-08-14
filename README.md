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
