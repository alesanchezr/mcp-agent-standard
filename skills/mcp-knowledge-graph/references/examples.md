# Sample envelopes (generic nouns)

## Create entity (ok)

```json
{
  "success": true,
  "entity": { "id": "ent_42", "type": "Client", "project_id": "proj_01" },
  "warnings": [
    { "code": "no_edges_created", "message": "Entity created with no relations." }
  ],
  "side_effects": [{ "code": "entity_upserted", "paths": ["entities/ent_42"] }],
  "next_actions": [
    {
      "tool": "link_entities",
      "priority": "optional",
      "args_hint": { "from_id": "ent_42", "rel_type": "has_interaction" }
    }
  ]
}
```

## Add edge with prior discovery_path (then attest on link)

Prior read/propose response may include:

```json
{
  "discovery_path": {
    "goal": "Confirm both endpoints exist before linking",
    "items": [
      {
        "kind": "think",
        "id": "think:endpoints",
        "title": "Endpoint check",
        "why": "Broken links pollute the graph",
        "look_for": ["from_id exists", "to_id exists", "rel_type in ontology"]
      },
      {
        "kind": "tool",
        "id": "tool:get_entity",
        "tool": "get_entity",
        "why": "Load both entities",
        "look_for": ["id", "type"],
        "available": true
      }
    ],
    "non_effects": ["skip_does_not_create_edge"]
  },
  "next_actions": [
    {
      "tool": "link_entities",
      "priority": "recommended",
      "args_hint": {
        "from_id": "ent_42",
        "to_id": "ent_99",
        "rel_type": "has_interaction",
        "discovery": {
          "status": "completed",
          "items_used": ["think:endpoints", "tool:get_entity"]
        }
      }
    }
  ],
  "warnings": [],
  "success": true
}
```

## Delete with dependents (actionRequired then ok with attestation)

```json
{
  "action_required": "confirm_delete_with_dependents",
  "message": "Entity ent_42 has 3 outbound edges. Confirm cascade or unlink first.",
  "warnings": [
    { "code": "dependents_exist", "message": "3 edges would be removed if confirm_cascade=true." }
  ],
  "next_actions": [
    {
      "tool": "delete_entity",
      "priority": "required",
      "args_hint": {
        "id": "ent_42",
        "confirm_cascade": true,
        "discovery": {
          "status": "skipped",
          "reason": "human confirmed cascade delete"
        }
      }
    }
  ]
}
```
