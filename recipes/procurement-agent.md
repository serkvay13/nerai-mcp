# Recipe — a procurement agent that checks the world before it re-orders

**Who this is for:** teams whose sourcing/procurement agent (SAP, Coupa, custom
LangGraph/CrewAI) decides re-orders, supplier switches or safety stock, and
currently does it blind to geopolitics.

## Connect

Claude Code / any MCP client:
```bash
claude mcp add nerai --transport http https://nerai-mcp.<your>.workers.dev/mcp \
  --header "Authorization: Bearer <key>"
```
Claude.ai custom connector (no header field): use `https://.../mcp/<key>`.
Free key: `POST /register {"email": "you@co.com"}`.

## The pattern

Before committing a PO to a supplier in country X, the agent calls:

1. `get_country_risk {country: "X"}` — is the supplier's country historically
   elevated? Percentile 90+ means elevated **against its own history**, not doom.
2. `get_early_warning` — is X among the platform-wide movers this week?
3. `get_supply_chain_risk {country: "X"}` — next-month disruption forecast.
4. If the goods move by sea: `get_maritime_corridors` — is the corridor
   deteriorating? (Team tier adds `get_live_chokepoint_traffic`.)

## Decision rule worth copying

Do not let the agent block a PO on one signal. A sensible policy:
- all quiet -> proceed;
- one elevated signal -> proceed + flag for human review;
- country elevated AND corridor deteriorating -> hold and escalate with the
  `_meta.as_of` and source lines quoted, so the human sees why.

Every response carries `_meta` (as_of, cadence, limitations). Log it with the
decision — that is your audit trail for "why did the agent hold this order".
