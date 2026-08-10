# Recipe — a trade-compliance agent that cites official sources

**Who this is for:** compliance teams whose agent triages orders or answers
"can we ship this?" questions, and must show its sources. Enterprise tier.

## Connect

```bash
claude mcp add nerai --transport http https://nerai-mcp.<your>.workers.dev/mcp \
  --header "Authorization: Bearer <key>"
```

## The pattern

For an order (product + destination + parties):
1. `get_tradecontrol_changes {jurisdiction}` — what changed in UK/US/EU lists
   this week, with the legal instrument and source link.
2. `check_commodity_controls {hs_code}` — deterministic: BIS CHPL by HS6, UK
   tariff measures. HS4 is never silently expanded to HS6 — an ambiguous code
   returns the candidate subheadings and says so.
3. `find_enforcement_precedent {product_family, destination, transit}` — only
   **analyst-reviewed** cases are returned; each names the shared features and
   links the authority's own document.
4. `get_sanctions_overview` for programme-level context.

## The rule that matters

The agent may draft; a human signs. Every response's `_meta` block says the data
summarises official sources and is not legal advice — surface that line in the
agent's answer verbatim. The precedent corpus tells you what authorities have
done; only counsel can tell you what your client should do.
