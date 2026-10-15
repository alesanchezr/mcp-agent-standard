# Interview checklist

Ask until clear. Prefer outcomes over tech jargon. Stop auth questions when the human does not care — use the frictionless local default.

## Outcomes

- What should success look like in a week? (e.g. “never miss a client follow-up”)
- Who acts — human, agent, or both?
- Reports or UI needed now, later, or never? (stub HTTP is OK if later)

## Entities and relations

- What nouns matter? (people, accounts, tasks, documents…)
- How do they connect? (has, owns, followed_by…)
- Lifecycle: create → update → close/archive/delete?

## Scope and blast radius

- One project/tenant or many?
- What must never happen silently? (cascade delete, auto-link all neighbors…)
- High-blast actions: merge, delete-with-dependents, publish…

## Reads vs writes

- What does the agent need to list/search vs mutate?
- Should humans only view reports, or also edit in UI? (default: UI read-oriented)

## Access (keep light)

- Non-technical / “I don’t care” → **do not** design OAuth. Plan: work session + 2h session-tied links + same-day browser session + gitignored secret.
- Only if they want multi-user or public internet access → propose stronger login (propose-first).

## Storage

- Any preference (local files, SQLite, hosted graph)? If none, pick a simple local default and say so in the proposal.
