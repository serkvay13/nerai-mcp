# NERAI Risk Intelligence — MCP Server

**The geopolitical grounding layer for AI agents.** Your procurement, trading or
compliance agent knows your systems; it does not know the world. NERAI is the
world, as a tool call — 60 countries, calibrated forecasts, maritime chokepoints,
sanctions and trade-control data, every answer source-linked and dated.

**Remote MCP server** (Streamable HTTP) — no install, no local process:

```
https://nerai-mcp.neraicorp.workers.dev/mcp
```

## Quick start

Get a free key instantly (no card, no call):

```bash
curl -X POST https://nerai-mcp.neraicorp.workers.dev/register \
  -H 'content-type: application/json' -d '{"email":"you@company.com"}'
```

Connect from Claude Code (any MCP client works the same way):

```bash
claude mcp add nerai --transport http https://nerai-mcp.neraicorp.workers.dev/mcp \
  --header "Authorization: Bearer <your-key>"
```

Clients without a header field (e.g. Claude.ai custom connectors):
`https://nerai-mcp.neraicorp.workers.dev/mcp/<your-key>`

## What your agent gets

16 tools across five layers:

| Layer | Tools |
|---|---|
| Country risk | `list_countries` · `get_country_risk` · `compare_countries` · `get_causal_drivers` |
| Foresight | `get_forecast_desk` (re-scored every 2 days, Brier-tracked) · `get_early_warning` |
| Supply chain | `get_supply_chain_risk` · `get_commodities` |
| Maritime | `get_maritime_corridors` · `get_live_chokepoint_traffic` (live AIS) · `get_circumvention_corridors` |
| Trade controls | `get_sanctions_overview` · `get_tradecontrol_changes` · `check_commodity_controls` · `find_enforcement_precedent` · `get_transaction_intelligence_status` |

## Why this one

- **Validation is published, not promised.** Out-of-sample MASE 0.85–0.95 and
  61–67% directional accuracy across 4–52 week horizons. Ask your data vendor
  for their Brier score — we publish ours.
- **Agent-safe by contract.** Every response carries `_meta`: `as_of`, source,
  refresh cadence and limitations. An agent cannot ask a follow-up question, so
  the answer states its own age and limits. That block is also your audit trail.
- **Enforcement precedents are human-reviewed.** Client-facing tools return only
  analyst-signed cases, each linking the authority's own document.

## Endpoints

- `GET /pricing` — machine-readable tier table (free / pro / enterprise)
- `GET /llms.txt` — agent-readable capability sheet
- `GET /openapi.json` — transport spec with the full tool catalogue
- `POST /register` — instant free key

## Recipes

Copy-paste agent patterns in [`recipes/`](recipes/): a procurement agent that
checks the world before re-ordering, a trading agent grounded in world state,
and a compliance agent that cites official sources and leaves signing to a human.

## Pricing

Free (5 countries, 100 calls/mo) · Pro €490/mo or €4,900/yr (all 60 countries,
100k calls) · Enterprise from €15,000/yr (HS-level commodity checks, reviewed
precedent corpus, SLA). Details: [neraicorp.com/ai-agents.html](https://neraicorp.com/ai-agents.html)

---

Not legal or investment advice. Built and operated by [NERAI BV](https://neraicorp.com),
Brussels. Contact: kagan@neraicorp.com
