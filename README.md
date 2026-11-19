# RateZip Bank Deposit and Mortgage Rates

Open-source plugin package for RateZip's six-tool rate-research and payment-calculation connector, including the Grok Bot marketplace submission. Package version **1.1.0**.

**MCP endpoint:** `https://mcp.ratezip.com/mcp`  
**Transport:** Streamable HTTP  
**Authentication:** none; no RateZip account or API key required  
**Publisher:** Peklava LLC d/b/a RateZip  
**Package license:** [MIT](LICENSE)

## Package and connection

This repository's root is an [Agent Plugin](https://agent-plugins.org/):

- [`plugin.json`](plugin.json) identifies the plugin and its license.
- [`mcp.json`](mcp.json) connects to the hosted six-tool MCP endpoint.
- [`skills/ratezip-rates/SKILL.md`](skills/ratezip-rates/SKILL.md) explains tool selection, rate provenance, and calculation assumptions.

Use the repository root in clients that support Agent Plugins. [Cursor documents support for this format](https://cursor.com/docs/reference/plugins). In a client that accepts a remote MCP connector, enter the endpoint above and select Streamable HTTP with no authentication. No local server, install script, executable hook, or credential is bundled.

The six-tool package uses `/mcp`. The separate `/mcp/grok` route now advertises additional Figure tools and is not the endpoint configured by this package. Marketplace reviewers should use the endpoint in `mcp.json` for this six-tool submission.

## Six tools

| Tool | Purpose |
|---|---|
| `compare_deposit_options` | Compare published deposit options against a balance, product, holding period, state, and explicitly requested fee or direct-deposit requirements. Unknown terms remain unknown. |
| `get_savings_rates` | Retrieve published savings and money-market APYs. An optional balance resolves published balance tiers. |
| `get_cd_rates` | Retrieve published CD APYs by term, with minimums and conditions. |
| `get_mortgage_rates` | Retrieve published mortgage or HELOC note rates and APRs by loan type, with source conditions and optional conforming-versus-jumbo context. |
| `calculate_deposit_earnings_difference` | Illustrate the earnings difference between two APYs for an explicit balance and period. |
| `compare_home_equity_payments` | Compare one to eight hypothetical home equity loan or HELOC payment scenarios using supplied amounts, note rates, terms, and fee assumptions. |

These six tools are read-only. They do not open accounts, submit applications, or initiate lender inquiries. Rates carry source URLs, observation timestamps, freshness information, and published conditions. The skill requires the assistant to preserve those qualifications and distinguish published data from a personal quote.

## Try it

- "Compare published savings rates for a $50,000 balance and show each source and observation date."
- "Show published 12-month CD rates, including minimum deposits and conditions."
- "Illustrate a hypothetical $100,000 home equity loan at an 8% annual note rate over 120 months. Assume zero upfront and financed fees."

For the last example, call `compare_home_equity_payments` with:

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
