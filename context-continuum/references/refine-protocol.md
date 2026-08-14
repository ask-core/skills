# Refinement protocol

Use this protocol only after the current user explicitly requests learning persistence.

## Select knowledge

Keep a candidate only when knowing it would change a future project decision or prevent repeated work. Exclude task status, TODOs, raw logs, transcripts, one-off requests, temporary paths, chronological attempts, generic programming knowledge, trivial failures, and unverified guesses.

Search each candidate, then choose one operation:

| Condition | Operation |
| --- | --- |
| No equivalent knowledge record exists | `create` |
| The same knowledge gains useful detail | `update` |
| New knowledge replaces old knowledge | `supersede` |
| A knowledge record is obsolete without a replacement | `deprecate` |

## Build the change set

```ts
interface RefineChangeSet { schema: 1; reason: string; changes: Change[]; }
```

Unknown fields are rejected. Target an existing knowledge record at most once per change set.

### Create

```json
{
  "schema": 1,
  "reason": "Workflow changes established a reusable validation rule.",
  "changes": [{
    "op": "create",
    "memory": {
      "key": "workflow.validate-after-change",
      "type": "procedure",
      "title": "Validate workflow after structural changes",
      "when": ["changing workflow nodes", "changing node connections"],
      "refs": {"paths": ["src/workflow/**"], "commands": ["workflow validate"]},
      "confidence": "verified",
      "content": "Run `workflow validate` after structural changes.",
      "why": "Structural edits can leave invalid references.",
      "exceptions": "Display-only changes do not require full validation."
    }
  }]
}
```

### Update

```json
{
  "op": "update",
  "id": "mem_01ABC...",
  "expected_revision": 2,
  "patch": {"content": "Run structural validation and targeted workflow tests."}
}
```

### Supersede

Use a distinct key for the replacement. The old record remains with `status: superseded`.

```json
{
  "op": "supersede",
  "id": "mem_01ABC...",
  "expected_revision": 1,
  "replacement": {
    "key": "workflow.validation.current",
    "type": "procedure",
    "title": "Use the current workflow validation sequence",
    "when": ["validating workflow changes"], "confidence": "verified",
    "content": "Run the current validation sequence documented by the project."
  }
}
```

### Deprecate

```json
{"op":"deprecate","id":"mem_01ABC...","expected_revision":3}
```

## Submit once

POSIX:

```bash
printf '%s' '<change-set-json>' | node "<skill-directory>/scripts/continuum.mjs" refine --stdin --json
```

PowerShell:

```powershell
$changeSetJson | node "<skill-directory>/scripts/continuum.mjs" refine --stdin --json
```

The runtime prechecks all changes and rolls back the whole refinement on failure. On `REVISION_CONFLICT`, fetch the latest record, reconsider the operation, and build a new change set.
