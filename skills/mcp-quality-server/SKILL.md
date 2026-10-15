---
name: mcp-quality-server
description: >-
  Designs or reviews MCP servers and tools for quality: response envelopes
  (ok / fail / actionRequired), soft gates, playbooks, agent education,
  warnings / side_effects / next_actions, discovery_path research menus,
  discovery attestation on consequential calls, and shared domain APIs for
  reports and human UIs. Use when the user mentions MCP quality, MCP standards,
  tool envelope, actionRequired, discovery_path, discovery attestation,
  shared domain API, or when reviewing MCP mutate responses.
---

# MCP quality server

Portable quality bar for building or reviewing MCP servers. Domain-agnostic. For the long-form essay see `docs/mcp-standards.md` (humans); follow this skill when acting as an agent.

## Optimize for

1. Correct writes
2. Honest blast radius (what happened *and* what did not)
3. Obvious next call (real registered tool names)
4. Token-efficient facts (dense structure, not omitted truths)

Do **not** optimize for tool count, README-length descriptions, or silent fan-out.

## Agent loop

discover (contract / playbook / bootstrap / work session) → optional `discovery_path` → **attest on consequential call** → gate if needed → mutate → educate (`warnings` / `side_effects`) → `next_actions`

## Ship checklist (mutate tools)

- [ ] Shared envelope: success / hard error / soft gate
- [ ] Always `warnings` and `next_actions` arrays
- [ ] Side effects and non-effects with stable codes (+ paths when known)
- [ ] `next_actions` only name real registered tools; `args_hint` when possible
- [ ] No silent multi-resource fan-out
- [ ] Soft gates for destructive/live, ambiguous target, missing tenant, new taxonomy
- [ ] Playbook / bootstrap when shape is dangerous
- [ ] Exact property/resource paths on validation failures when known
- [ ] Default reads stay small
- [ ] Dense education — same facts as human docs, not a shorter lie
- [ ] Changelog / conventions bumped when agent-facing contract changes
- [ ] `discovery_path` when consequential judgment helps (menu optional; not folded into `next_actions`)
- [ ] `discovery` attestation param when prior menu **or** high-blast write; `skipped` needs a reason; no separate ack tool
- [ ] If reports/UI: shared domain core + MCP + HTTP (stub OK); auth not public; work session before UI links

## Refusals

| Refuse | Why |
|--------|-----|
| Parallel tool families for the same write | One vocabulary + scope param |
| Silent fan-out | Guide with `next_actions` |
| Invent taxonomy without confirmation | Product decision |
| Missing `warnings` / `next_actions` keys | Empty arrays are honest |
| Non-effects only in prose | Skimmed away |
| Tool-count vanity | Quality ≠ more tools |
| Menu items as required next steps | `discovery_path` is optional |
| Silent omit of required attestation | Explicit completed or skipped+reason |
| Empty `skipped` | Needs a one-line reason |
| Dual UI/MCP schemas | Shared domain core |
| Public anonymous reads by default | Auth required |
| Blocking non-technical humans on OAuth | Frictionless local default |
| High-blast API writes without attestation | Same rule as MCP |
| UI links without work session | Session-tied tokens |

## Deepen when needed

Read references in this skill folder:

- `references/standards.md` — Standards 1–8 distilled
- `references/envelope.md` — ok / fail / actionRequired + example JSON
- `references/discovery-path.md` — menu vs next_actions vs attestation
- `references/shared-domain-api.md` — core + MCP + HTTP + frictionless auth
- `references/playbook-bootstrap.md` — playbooks, conventions, work session, changelog
