# Response envelope

Conceptual shapes: **ok** / **fail** / **actionRequired**. Implement via one shared helper so every mutate stays consistent.

## Fields

| Shape | Typical signals | Always / often |
|-------|-----------------|----------------|
| ok | Write succeeded | `warnings` (array), `next_actions` (array); optional `side_effects`; optional `discovery_path` |
| fail | Retrying same call won’t help | Clear `message` / codes; do not invent a safe next tool |
| actionRequired | Judgment needed | May include `next_actions` for retry; `warnings` array |

Non-effects: structured `warnings[]` with stable `code` (and paths when known) — not only prose `message`.

## Example (generic project / entity)

Success after creating an entity (illustrative JSON; field names for success may use `success: true` in implementations):

```json
{
  "success": true,
  "entity": {
    "id": "ent_01",
    "type": "Client",
    "project_id": "proj_01"
  },
  "warnings": [
    {
      "code": "neighbors_unchanged",
      "message": "No related FollowUp entities were created."
    }
  ],
  "side_effects": [
    {
      "code": "entity_created",
      "paths": ["entities/ent_01"]
    }
  ],
  "next_actions": [
    {
      "tool": "link_entities",
      "priority": "recommended",
      "args_hint": {
        "from_id": "ent_01",
        "rel_type": "has_follow_up",
        "to_type": "FollowUp"
      }
    }
  ],
  "discovery_path": {
    "goal": "Confirm naming matches peer Clients before linking",
    "items": [
      {
        "kind": "think",
        "id": "think:peer-names",
        "title": "Peer naming",
        "why": "Avoid duplicate Client labels",
        "look_for": ["similar display names", "same external_id"]
      },
      {
        "kind": "tool",
        "id": "tool:list_entity",
        "tool": "list_entity",
        "why": "Sample peer Clients in this project",
        "look_for": ["type=Client", "project_id"],
        "available": true
      }
    ],
    "non_effects": ["skip_does_not_create_links"]
  }
}
```

Soft gate when attestation is missing on a high-blast call:

```json
{
  "action_required": "discovery_attestation_required",
  "message": "Pass discovery: { status: \"completed\", items_used } or { status: \"skipped\", reason }.",
  "warnings": [],
  "next_actions": [
    {
      "tool": "delete_entity",
      "priority": "required",
      "args_hint": {
        "id": "ent_01",
        "discovery": { "status": "skipped", "reason": "confirmed delete with human" }
      }
    }
  ]
}
```
