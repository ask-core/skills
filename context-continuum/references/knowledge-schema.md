# Knowledge record schema

## Create input

```ts
interface CreateKnowledgeInput {
  key: string;
  type: "fact" | "rule" | "procedure" | "pitfall" | "decision";
  title: string;
  when: string[];
  refs?: {
    paths?: string[];
    symbols?: string[];
    commands?: string[];
  };
  confidence?: "observed" | "verified";
  content: string;
  why?: string;
  exceptions?: string;
}
```

- `key`: Use a stable lowercase semantic key containing letters, digits, dots, underscores, or hyphens; prefer `<domain>.<behavior>`.
- `when`: Write concrete future retrieval cues.
- `content`: Make the knowledge actionable and self-contained.
- `why`: Record rationale only when it changes interpretation.
- `exceptions`: Record real boundaries, not caveats by default.
- `confidence`: Use `observed` unless tests, authoritative project configuration, or repeatable behavior verify the claim.

## System fields

Never send `schema`, `id`, `status`, `revision`, `supersededBy`, `createdAt`, or `updatedAt` in create or patch data. The runtime owns them.

## Update patch

Send a non-empty subset of create-input fields. Omit unchanged fields.
