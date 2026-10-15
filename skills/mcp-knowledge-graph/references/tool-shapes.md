# Suggested tool vocabulary

One vocabulary + **scope** params (e.g. `project_id`). Do not invent parallel families per scope.

| Tool | Role |
|------|------|
| `list_entity` | Filtered/light list (type, query, project_id, limit) |
| `get_entity` | One entity by id (optional light vs include_edges) |
| `upsert_entity` | Create/update entity; gate new `type` if not in ontology |
| `link_entities` | Create edge (`from_id`, `to_id`, `rel_type`); gate new `rel_type` |
| `unlink_entities` | Remove edge by edge_id or endpoints+rel_type |
| `query_subgraph` | Explicit neighborhood / path query (heavier; not default list) |
| `explain_ontology` | Playbook: types, rel_types, constraints, examples |
| `bootstrap_agent` | Conventions + changelog + **start/resume work session** (`work_session_id`) |

## Scope params

Prefer: `project_id` (or tenant), `type`, `id`, `rel_type`, `confirm_*` for gates, `discovery` for attestation on high-blast mutates.

## HTTP parity

Map the same ops to routes, e.g.:

- `GET /entities` ↔ `list_entity`
- `GET /entities/:id` ↔ `get_entity`
- `PUT /entities/:id` ↔ `upsert_entity` (if writes enabled)
- `POST /links` ↔ `link_entities`
- `GET /subgraph` ↔ `query_subgraph`
- `GET /ontology` ↔ `explain_ontology`

Default: HTTP reads + reports; high-blast writes via MCP unless human confirmed API writes.
