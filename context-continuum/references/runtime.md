# Runtime reference

Invoke commands as `node "<skill-directory>/scripts/continuum.mjs" <command> ... --json`. JSON mode emits one envelope and no explanatory text.

## Commands

| Command | Purpose | Default |
| --- | --- | --- |
| `info` | Report product, version, and protocol | — |
| `init [directory]` | Initialize `.memory/` | `.` |
| `recall <task>` | Return relevant active knowledge with content | 5 results, 12000 chars |
| `search <query>` | Return ranked active summaries | 8 results |
| `get <id>` | Return one complete knowledge record | — |
| `refine --stdin` | Apply one change set | stdin ≤ 1 MiB |
| `list` | List summaries | all statuses |
| `history` | Return newest refinement records | 50 records |
| `doctor` | Validate storage and index integrity | — |
| `reindex` | Rebuild SQLite from Markdown | — |

Options:

- `search`: `--limit`, `--status`, `--type`
- `list`: `--status`, `--type`
- `recall`: `--limit`, `--max-chars`

## Envelopes

```json
{"ok":true,"data":{}}
```

```json
{"ok":false,"error":{"code":"ERROR_CODE","message":"Safe message","details":{}}}
```

## Recovery

- `MEMORY_NOT_INITIALIZED`: Continue without stored project knowledge unless the user requested setup.
- `MEMORY_NOT_FOUND`: Search again or discard the stale ID.
- `REVISION_CONFLICT`: Get the latest record and rebuild the change.
- `DUPLICATE_KEY`: Search the existing key and choose the correct operation.
- `SCHEMA_INVALID` / `INVALID_CHANGESET`: Correct the input; do not weaken it.
- `INDEX_ERROR`: Run `reindex`, then retry the read.
- `REFINE_IN_PROGRESS`: Wait, reread affected records, then reconsider the change.
- `BOOTSTRAP_NODE_UNSUPPORTED`: Require Node.js 22.13+.
- `BOOTSTRAP_PLATFORM_UNSUPPORTED` / `BOOTSTRAP_ARCH_UNSUPPORTED`: Require macOS, Windows, or Linux on x64 or arm64.
- `BOOTSTRAP_ASSET_MISSING` / `BOOTSTRAP_ASSET_INVALID`: Reinstall the Skill.
- `BOOTSTRAP_INSTALL_INVALID` / `BOOTSTRAP_LOCK_TIMEOUT` / `BOOTSTRAP_EXEC_FAILED`: Report the error; never install an unrelated global package.
