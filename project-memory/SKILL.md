---
name: project-memory
description: "Uses a deterministic, project-local Memory CLI to retrieve and maintain reusable coding knowledge in .memory. Use proactively during project tasks: recall before planning, and search or get when a concrete error, path, command, or symbol may have prior knowledge. Use the write workflow only when the current user explicitly asks to summarize learning, remember an experience, refine memory, or update project memory; ordinary task completion never permits a write. The bundled runtime installs itself, so never ask the user to install the CLI."
---

# Project Memory

Use the CLI for storage and retrieval. Perform all semantic judgment yourself; the CLI never calls a model or interprets an agent session.

## Run the CLI

Resolve `<skill-directory>` as the directory containing this file. Run every command through:

```bash
node "<skill-directory>/scripts/memory.mjs" <command> ... --json
```

Never call a global `memory` command or ask the user to install the CLI. The wrapper verifies and caches its bundled runtime. On `BOOTSTRAP_*`, report the error; Node.js 22.13+ is the only prerequisite.

## Follow the trigger boundary

| Situation | Initiator | Action |
| --- | --- | --- |
| A project task starts | Agent | Run `recall` |
| A concrete problem appears | Agent | Run `search`, then `get` when needed |
| The current user explicitly requests learning persistence | User | Run the refinement workflow |

Reads are agent-initiated. Writes are user-initiated. Never suggest, request, or remind the user to save a memory.

Do not treat task completion, a useful discovery, or an earlier standing instruction as write permission. A request for an ordinary conversational summary does not permit a write unless it explicitly asks to learn, remember, persist, or update project memory.

## Read proactively

Before planning or implementation, run once from the current project:

```bash
node "<skill-directory>/scripts/memory.mjs" recall "<current task>" --json
```

Apply relevant results according to `when`, `why`, and `exceptions`. On `MEMORY_NOT_INITIALIZED`, continue without memory; initialize only when the user requests setup.

When an error, path, command, or symbol becomes concrete, narrow the query:

```bash
node "<skill-directory>/scripts/memory.mjs" search "<problem>" --json
node "<skill-directory>/scripts/memory.mjs" get <memory-id> --json
```

Read [references/cli.md](references/cli.md) only when command options, response envelopes, or error recovery are needed.

## Refine only after an explicit request

Treat phrases such as “总结学习”, “记住这次经验”, “沉淀经验”, “refine memory”, or “更新项目记忆” as permission for the current request.

After permission:

1. Read [references/memory-schema.md](references/memory-schema.md) and [references/refine-protocol.md](references/refine-protocol.md).
2. Extract only durable project knowledge that would change a future decision or prevent repeated work.
3. Search each candidate before choosing `create`, `update`, `supersede`, or `deprecate`.
4. Get the latest revision before changing an existing memory.
5. Submit one change set. On `REVISION_CONFLICT`, reread, reconsider, and rebuild it; never retry blindly.

## Preserve storage boundaries

- Never edit `.memory/` directly.
- Treat `memories/*.md` as truth, `history/refinements.jsonl` as append-only, and `index.sqlite` as rebuildable.
- Use `doctor` for integrity failures and `reindex` only to rebuild the search index.
