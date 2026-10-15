# discovery_path vs next_actions vs attestation

## Research menu (`discovery_path`)

Optional. Recommend ways to deepen judgment before a consequential step.

| | `discovery_path` | `next_actions` |
|--|------------------|----------------|
| Role | Optional research menu | Follow-ups needed for correctness |
| Skip menu items? | Yes | These are the real next calls |
| Contents | `think` then `tool` items | Registered tools + usually `args_hint` |

Rules:

- Not required on every success
- Think items ≤5; short `why` / `look_for`
- Tool names must be real catalog tools; `available: false` + hint when capped
- Do **not** fold discovery into `next_actions`
- No resource-specific `args_hint` on discovery tool items
- Menu is **not** a lock — following items is optional

## Discovery attestation (`discovery` param)

On the **next consequential tool call** (same tool vocabulary — **no new ack tool**), require attestation when:

1. The prior response attached a `discovery_path`, **or**
2. The write is **high-blast** (live apply, delete-with-dependents, merge, publish, new taxonomy/relation type, …)

```json
{ "status": "completed", "items_used": ["think:peer-names", "tool:list_entity"] }
```

```json
{ "status": "skipped", "reason": "already sampled peers this session" }
```

- `skipped` **must** include a short `reason`
- Silent omit → `actionRequired` / `fail` with code like `discovery_attestation_required`
- Explicit skip satisfies the requirement
- Low-blast writes without a prior menu: no attestation param

## Mental model

```text
response may include discovery_path (optional menu)
  → agent may think/tools or skip research content
  → consequential call MUST pass discovery: completed | skipped+reason
  → then gate / mutate as usual
```
