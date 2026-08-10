# Recipe — grounding a trading/analysis agent in world state

**Who this is for:** quant/macro teams whose agent summarises overnight risk or
sizes exposure to geopolitically sensitive assets, and needs a world-state input
that is not a news scrape.

## Connect

Same as any MCP client:
```bash
claude mcp add nerai --transport http https://nerai-mcp.<your>.workers.dev/mcp \
  --header "Authorization: Bearer <key>"
```

## The pattern

Morning brief, per watchlist country:
1. `get_country_risk {country, all_topics: true}` — ranked topic percentiles.
2. `get_causal_drivers {country}` — which series historically lead this one
   (Granger; lead-lag evidence, not proof of causation — the tool says so).
3. `get_forecast_desk` — open calibrated questions with probability history,
   re-scored every 2 days; each carries its resolution rule.
4. `get_commodities` / `get_live_chokepoint_traffic` (team+) for the physical
   channel: chokepoint stress moves freight, freight moves goods inflation.

## What NOT to do

Do not turn a percentile into a position. The `_meta.limitations` block in every
response states this and your compliance team will appreciate that it is logged:
NERAI publishes out-of-sample MASE and directional accuracy — direction is
61–67%, which is an edge for *attention*, not an execution signal. Nothing from
this server is investment advice.
