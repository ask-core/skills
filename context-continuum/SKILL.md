---
name: context-continuum
description: "Preserves durable, project-local engineering intelligence across coding sessions. Use proactively at the start of project work to recover relevant decisions, procedures, pitfalls, and facts; search again when a concrete error, path, command, or symbol appears. Capture or revise knowledge only when the current user explicitly asks to remember an experience, summarize learning, or update project knowledge; ordinary task completion never permits writes. The self-contained runtime requires only Node.js 22.13+."
---

# Context Continuum

Maintain continuity with a deterministic, project-local knowledge store. Perform all semantic judgment yourself; the bundled runtime never calls a model or observes agent sessions.

## Use the bundled runtime

Resolve `<skill-directory>` as the directory containing this file. Run every command through:

```bash
node "<skill-directory>/scripts/continuum.mjs" <command> ... --json
```

Never call a global command or ask the user to install a separate package. The wrapper verifies and caches its bundled runtime. On `BOOTSTRAP_*`, report the error; Node.js 22.13+ is the only prerequisite.

## Follow the trigger boundary

| Situation | Initiator | Action |
| --- | --- | --- |
| A project task starts | Agent | Run `recall` |
| A concrete problem appears | Agent | Run `search`, then `get` when needed |
| The current user explicitly requests knowledge persistence | User | Run the refinement workflow |

Reads are agent-initiated. Writes are user-initiated. Never suggest, request, or remind the user to capture knowledge.

Do not treat task completion, a useful discovery, or an earlier standing instruction as write permission. A request for an ordinary conversational summary does not permit a write unless it explicitly asks to learn, remember, persist, or update project knowledge.

## Read proactively

Before planning or implementation, run once from the current project:

```bash
node "<skill-directory>/scripts/continuum.mjs" recall "<current task>" --json
```

Apply relevant results according to `when`, `why`, and `exceptions`. On `MEMORY_NOT_INITIALIZED`, continue without memory; initialize only when the user requests setup.

When an error, path, command, or symbol becomes concrete, narrow the query:

```bash
node "<skill-directory>/scripts/continuum.mjs" search "<problem>" --json
node "<skill-directory>/scripts/continuum.mjs" get <knowledge-id> --json
```

Read [references/runtime.md](references/runtime.md) only when command options, response envelopes, or error recovery are needed.

## Refine only after an explicit request

Treat phrases such as “总结学习”, “记住这次经验”, “沉淀经验”, “capture this learning”, or “更新项目知识” as permission for the current request.

After permission:

1. Read [references/knowledge-schema.md](references/knowledge-schema.md) and [references/refine-protocol.md](references/refine-protocol.md).
2. Extract only durable project knowledge that would change a future decision or prevent repeated work.
3. Search each candidate before choosing `create`, `update`, `supersede`, or `deprecate`.
4. Get the latest revision before changing an existing knowledge record.
5. Submit one change set. On `REVISION_CONFLICT`, reread, reconsider, and rebuild it; never retry blindly.

## Preserve storage boundaries

- Never edit `.memory/` directly.
- Treat `memories/*.md` as truth, `history/refinements.jsonl` as append-only, and `index.sqlite` as rebuildable.
- Use `doctor` for integrity failures and `reindex` only to rebuild the search index.
