# RateZip Bank Deposit and Mortgage Rates — MCP Server

Live US consumer interest rates for AI assistants and agents, over the open
[Model Context Protocol](https://modelcontextprotocol.io): high-yield savings
and money-market APYs, CD rates by term, fixed mortgage rates, and
home-equity/HELOC rates — **every figure carries its source URL, observation
timestamp, and freshness status**. Data past its serve window is withheld,
never shown as current.

**Remote server:** `https://mcp.ratezip.com/mcp` (streamable HTTP, **no auth,
no API key**)
**Official MCP Registry:** [`com.ratezip/rates`](https://registry.modelcontextprotocol.io/v0/servers?search=com.ratezip)
**Operated by:** [Peklava LLC / RateZip](https://www.ratezip.com) · [Methodology](https://www.ratezip.com/methodology) · [Terms](https://www.ratezip.com/rates-terms) · [Privacy](https://www.ratezip.com/privacy-and-security)

## Tools

| Tool | What it does |
|---|---|
| `get_savings_rates` | Current savings/MMA APYs. Optional `balance` resolves each account's **published balance tier** (threshold and blended tiers both modeled) and re-ranks by what you'd actually earn. |
| `get_cd_rates` | CD APYs by term (`term_months`), with minimums and conditions. |
| `get_mortgage_rates` | Mortgage rates grouped by loan type incl. HELOC — note rate + APR, representative-scenario conditions. Optional `loan_amount` flags conforming vs jumbo. |
| `calculate_deposit_earnings_difference` | Illustrative annualized earnings difference between two APYs, with every omission stated. |

All tools are **read-only** (`readOnlyHint: true`, `destructiveHint: false`).
The server also ships an interactive rate-card widget resource
(`ui://ratezip/rates-cards.html`) for Apps-SDK surfaces.

## Add it to your assistant

- **Claude** — Settings → Connectors → Add custom connector → URL above, no auth
- **Grok** — grok.com/connectors → New Connector → Custom
- **Perplexity** (Pro/Max) — Connectors → Add custom connector → Advanced → Authentication: **None**
- **Gemini CLI** — `gemini mcp add --scope user -t http ratezip-rates https://mcp.ratezip.com/mcp` (set the server `"trust": true` for headless use)
- **Any MCP client** — point a streamable-HTTP client at the URL above

Try: *"What's the best high-yield savings account rate right now?"* ·
*"I have $50,000 in savings — which account earns me the most?"* ·
*"What are today's mortgage and HELOC rates?"*

## Data honesty

- Rates are observed on institutions' **own published rate pages** by an
  identified agent that honors robots.txt; national deposit averages come from
  the FDIC's public series. No third-party benchmark is redistributed.
- Conditions travel with every rate — waitlists, promo windows, balance
  tiers, "up to" qualifiers — instead of a bare headline number.
- Factual rate data, **not** financial advice; no offers, ads,
  lender-specific quotes, approvals, or account opening.

## About this repository

This is the public home for the hosted service (docs + registry manifest —
see [`server.json`](server.json)). The service implementation is operated
privately by Peklava LLC. Questions or data issues: press@ratezip.com.
