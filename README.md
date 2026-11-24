# RateZip Bank Deposit, Mortgage, and Auto Insurance Research

Open-source plugin package for RateZip's ten-tool rate-research, benchmark, and calculation connector, including the Grok Bot marketplace submission. Package version **1.2.0**.

**MCP endpoint:** `https://mcp.ratezip.com/mcp`  
**Transport:** Streamable HTTP  
**Authentication:** none; no RateZip account or API key required  
**Publisher:** Peklava LLC d/b/a RateZip  
**Package license:** [MIT](LICENSE)

The [November 22, 2026 coverage update](docs/rate-watch-2026-11-22.md) lists the rates, account terms, and date of the weekly deposit comparison served by `get_rate_watch_report`.

## Package and connection

This repository's root is an [Agent Plugin](https://agent-plugins.org/):

- [`plugin.json`](plugin.json) identifies the plugin and its license.
- [`mcp.json`](mcp.json) connects to the hosted MCP endpoint.
- [`skills/ratezip-rates/SKILL.md`](skills/ratezip-rates/SKILL.md) explains tool selection, rate provenance, auto-benchmark boundaries, and calculation assumptions.

Use the repository root in clients that support Agent Plugins. [Cursor documents support for this format](https://cursor.com/docs/reference/plugins). In a client that accepts a remote MCP connector, enter the endpoint above and select Streamable HTTP with no authentication. No local server, install script, executable hook, or credential is bundled.

This ten-tool package uses `/mcp`. The separate `/mcp/grok` route exposes additional lender/partner integrations and is not the endpoint configured by this package. Marketplace reviewers should use the endpoint in `mcp.json` (`https://mcp.ratezip.com/mcp`) for this ten-tool read-only submission.

## Ten read-only tools

| Tool | Purpose |
|---|---|
| `compare_deposit_options` | Compare published deposit options against a balance, product, holding period, state, and explicitly requested fee or direct-deposit requirements. Unknown terms remain unknown. |
| `get_savings_rates` | Retrieve published savings and money-market APYs. An optional balance resolves published balance tiers. |
| `get_cd_rates` | Retrieve published CD APYs by term, with minimums and conditions. |
| `get_rate_watch_report` | Rank observed deposit APYs and separately labeled money market fund 7-day SEC yields, compare against a historical snapshot and the latest FOMC decision, watch institutions, and flag threshold crossings. |
| `get_mortgage_rates` | Retrieve published mortgage or HELOC note rates and APRs by loan type, with source conditions and optional conforming-versus-jumbo context. |
| `compare_home_equity_payments` | Compare one to eight hypothetical home equity loan or HELOC payment scenarios using supplied amounts, note rates, terms, and fee assumptions. |
| `calculate_deposit_earnings_difference` | Illustrate the earnings difference between two APYs for an explicit balance and period. |
| `get_state_auto_benchmarks` | Retrieve audited 2024–2026 sample auto insurance price spreads and cheapest open-market carrier win counts from 10 official US state insurance department (`.gov`) publications (CA, TX, MD, KY, NV, AL, OK, ND, WV, and DC), or return an explicit unavailable notice with the NAIC regulator directory for other states. |
| `check_auto_carrier_quality` | Look up verified AM Best or Demotech financial strength rating standing (`active`, `withdrawn`, or `not rated`), underwriting subsidiary/eligibility notes, public regulatory context, and NAIC Consumer Insurance Search guidance for a US auto insurer. |
| `compare_auto_quotes` | Normalize one to ten caller-supplied auto insurance quotes (monthly, 6-month, or annual, plus optional fees) to annualized and monthly-equivalent cost, rank them, flag coverage downgrades against an optional current policy, and attach verified carrier standing context when recognized. |

All ten tools are read-only (`readOnlyHint: true`, `destructiveHint: false`). They do not open accounts, submit applications, initiate lender or insurer inquiries, or require personal identifiers. Rates and benchmarks carry source URLs, observation timestamps, freshness information, and published conditions. The skill requires the assistant to preserve those qualifications and distinguish published data or mathematical illustrations from personal quotes or endorsements.

## Try it

- "Compare published savings rates for a $50,000 balance and show each source and observation date."
- "Show published 12-month CD rates, including minimum deposits and conditions."
- "Run the weekly deposit rate watch report and flag major moves."
- "Illustrate a hypothetical $100,000 home equity loan at an 8% annual note rate over 120 months. Assume zero upfront and financed fees."
- "Show official state auto insurance price benchmarks for Texas, and check financial strength standing for Progressive."
- "Compare two 6-month auto quotes ($920 vs $1,080) against my current $1,150 6-month policy and flag any deductible or liability coverage downgrades."

For the home equity example, call `compare_home_equity_payments` with:

```json
{
  "scenarios": [
    {
      "label": "Hypothetical verification",
      "product_type": "home_equity_loan",
      "amount_borrowed": 100000,
      "annual_interest_rate_percent": 8,
      "repayment_months": 120,
      "fees": { "upfront_cash": 0, "financed": 0 }
    }
  ]
}
```

Expected monthly principal and interest: **$1,213.28**. The result is a payment illustration, not a lender offer or APR. Other costs are excluded. Omitted fees in other scenarios remain unknown, not zero; HELOC comparisons show both interest-only and repayment phases when supplied.

## Open-source scope

The plugin manifest, connection configuration, skill, and documentation in this repository are available under the MIT license. This is a client plugin for a remotely operated service; it does not contain the hosted MCP backend, rate-collection infrastructure, or provider integrations. Those service implementations remain privately operated by Peklava LLC. The MIT license covers this repository's package, not the separately operated service or third-party data.

The older Claude-oriented package remains in the historical `claude/plugin-bundle` branch. Use `main` for this current package. Existing [`server.json`](server.json) and [`glama.json`](glama.json) retain the service's registry metadata.

[RateZip](https://www.ratezip.com) · [Methodology](https://www.ratezip.com/methodology) · [Terms](https://www.ratezip.com/rates-terms) · [Privacy](https://www.ratezip.com/privacy-and-security) · [Official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.ratezip)

Questions or data issues: press@ratezip.com.
