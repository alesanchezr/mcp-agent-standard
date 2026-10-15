# Standards 1–8 (distilled)

Portable summary. No product CMS appendix.

## 1 — Response envelope

Mutates share one contract: success (`ok`), hard error (`fail`), soft gate (`actionRequired`). Always include `warnings` and `next_actions` arrays on success/gate paths. Optional `side_effects`. Non-effects go in `warnings` with stable `code`s. Reads: `next_actions: []` unless follow-up is required. One shared helper module.

## 2 — Dense education

Same facts as human UI/docs, denser: structured fields over essays. Deleting a non-effect to “save tokens” is lying by omission.

## 3 — Guide, don’t silently fan out

No silent sibling / neighbor / locale / environment updates unless the contract makes blast radius explicit in `side_effects` / `warnings`. One tool vocabulary + scope param. Wide blast radius must be said out loud.

## 4 — Gates over opaque errors

Use `actionRequired` for destructive/live confirm, ambiguous target, missing tenant, new taxonomy, missing discovery attestation. Permission to write ≠ permission to invent taxonomy.

## 5 — Discover before mutate

Playbooks + bootstrap (+ work session when UI links exist). Happy path: scope → work session → contract → playbook → sample peers → mutate (with attestation when required) → verify.

**`discovery_path`:** optional research menu (`goal`, `items[]` think-then-tool, `non_effects`). Not `next_actions`. Following items is optional.

**`discovery` attestation** (param on consequential tool): required when prior menu **or** high-blast write. `{ status: "completed", items_used }` or `{ status: "skipped", reason }` (reason required). Silent omit → gate/fail. No separate ack tool.

## 6 — Exact paths and real tool names

Validation: exact property/resource paths when known. `next_actions[].tool` must be registered. Prefer `args_hint`. Use priorities deliberately.

## 7 — Read hygiene

Small defaults; filters; light vs heavy views; don’t fill context with unused data.

## 8 — Shared domain API

One domain core → MCP + HTTP (live or stub). Same ops/ids/codes. API read-first; high-blast writes prefer MCP (+ attestation); if HTTP high-blast writes exist, same attestation. Auth required; frictionless local default for non-technical humans (work-session-tied 2h links, same-day browser session, read-oriented UI). Propose-first before expanding routes/writes.
