# RateZip Bank Deposit and Mortgage Rates

Live US consumer interest rates for Claude: high-yield savings and money-market
APYs, CD rates by term, 30-year fixed mortgage rates and HELOC rates. Every
figure carries the institution's own source URL, the time RateZip observed it,
and a freshness status. Rates past their serve window are withheld, never shown
as current.

## Use it

Ask Claude for the best savings rate, to compare CD terms, for today's mortgage
or HELOC rates, or how much more a balance would earn at a better rate. The
bundled skill tells Claude which RateZip tool to call and how to present the
result: the top figure first, the FDIC national average for context, the
observation date and source on every rate, the conditions that change what you
would earn, and no recommendations.

Example prompts:

- "What's the best high-yield savings account rate right now?"
- "Compare 6-month CD rates."
- "I have $50,000 in savings. Which account earns me the most, and how much?"
- "What are today's 30-year fixed mortgage rates?"
- "What's the going rate on a HELOC?"
- "For a $400,000 mortgage, is that conforming or jumbo, and what's the rate?"

## Connector

The plugin connects to RateZip's remote MCP server at
`https://mcp.ratezip.com/mcp` (streamable HTTP, no account or credentials).
All four tools are read-only: `get_savings_rates`, `get_cd_rates`,
`get_mortgage_rates` and `calculate_deposit_earnings_difference`.

## Data

A tool call sends only the arguments you provide (an optional balance, CD
term, loan type, loan amount, state or credit score) to mcp.ratezip.com, which
answers from its own cache of observed rates. The plugin reads nothing else
from the conversation and stores nothing on your device. Rate data is factual
information, not financial advice; the service returns no offers, ads,
lender-specific quotes or account opening.

Methodology: https://www.ratezip.com/methodology
Terms: https://www.ratezip.com/rates-terms
Privacy policy: https://www.ratezip.com/privacy-and-security

Operated by Peklava LLC (RateZip). Support: press@ratezip.com
