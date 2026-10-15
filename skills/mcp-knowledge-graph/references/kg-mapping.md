# Standard → knowledge graph

| Standard | Graph application |
|----------|-------------------|
| 1 Envelope | Create/link/delete return ok/fail/actionRequired with `warnings` + `next_actions` |
| 2 Dense education | Return entity/edge ids, rel_type, scope, non-effects (e.g. dependents not deleted) |
| 3 No silent fan-out | No silent neighbor rewrites or cascade deletes without contract + visible side_effects |
| 4 Gates | Soft-gate new entity types, new rel_types, delete-with-dependents, ambiguous entity match |
| 5 Discover + attest | Playbook/ontology explain before structural writes; `discovery_path` before decide; `discovery` param on high-blast or after menu |
| 6 Exact paths | Failures cite `entity_id`, `edge_id`, `rel_type`, property paths |
| 7 Read hygiene | Filtered list; light get vs heavy neighborhood/subgraph |
| 8 Shared API | Same list/get/upsert/link/query shapes on HTTP; read-first UI; attestation if HTTP high-blast writes |

Storage (files, SQLite, hosted graph, …) is a project choice from the builder interview — not prescribed here.
