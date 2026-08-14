# ask-core skills

可直接通过 [`skills`](https://skills.sh/) CLI 安装的 Agent Skills。

## 可用 Skill

| Skill | 用途 |
| --- | --- |
| `project-memory` | 在项目内检索和维护可复用的长期知识；读取可主动执行，写入仅在用户明确要求时执行。 |

## 安装

列出仓库中的 Skill：

```bash
npx skills add https://github.com/ask-core/skills --list
```

安装 `project-memory`：

```bash
npx skills add https://github.com/ask-core/skills --skill project-memory
```

全局安装并跳过确认：

```bash
npx skills add https://github.com/ask-core/skills --skill project-memory --global --yes
```

`project-memory` 需要 Node.js 22.13 或更高版本。Skill 自带经过校验的运行时，不需要另外安装 `memory-cli` 或 npm 依赖。

## 仓库布局

每个 Skill 都直接位于仓库根目录，目录名与 `SKILL.md` 中的 `name` 一致：

```text
project-memory/
├── SKILL.md
├── agents/
├── assets/
├── references/
└── scripts/
```

仓库只保存可直接安装的自包含 Skill 产物；开发源码、测试和构建工程留在各自的上游项目中。

## License

[MIT](LICENSE)
