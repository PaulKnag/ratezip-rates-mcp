# Rate Watch coverage for November 22, 2026

**Four of the 19 RateZip banking offers reviewed appeared in the public Rate Watch report.** At 1:58 p.m. Eastern, the report returned three fresh issuer observations and one stale observation for a $10,000 balance. The comparison matched the institution, product, promotion and CD term across the top seven results and the watchlist. The 19 offers are the website offers selected for this comparison.

| Offer and issuer source | APY at $10,000 | Position | Published conditions |
| --- | ---: | --- | --- |
| [Forbright 12-Month Growth CD](https://openaccount.forbrightbank.com/products/12MonthGrowthCd) | 4.50% | 4th, fresh | $1,000 minimum to fund and maintain the account. |
| [Mitten Digital High-Yield Savings](https://www.mittensavings.com/) | 4.41% | 5th, fresh | Top tier starts at $10,000. Membership eligibility applies. New accounts have 180-day rate protection. |
| [CIT Platinum Savings BOOST60](https://www.cit.com/cit-bank/platinum-savings/boost60) | 4.35% | 6th, fresh | Six-month variable boost with code BOOST60 on qualifying accounts enrolled by December 15. Upper boosted tier starts at $5,000. Recent cash-bonus and other boosted accounts have exclusions. |
| [Capital One 360 Performance Savings](https://www.discover.com/online-banking/savings-account/) | 3.20% | Watchlist, **stale** | Variable rate from the former Discover savings page, which redirects to Capital One. The report marked this observation stale. |

## The 15 remaining gaps

RateZip traced all 15 to the Bankrate feed used on its banking pages. The public MCP serves rates from sources approved for public agent redistribution. RateZip has no verified permission to distribute these partner listings through that endpoint. The RateZip Rate Index also draws on the website observations, so it supplies no independent source for these offers. This review added zero offers.

| Website listing | Count | Offers missing from this report |
| --- | ---: | --- |
| [Sponsored CDs](https://banking.ratezip.com/cd-rates) | 4 | Happen 11-month, Marcus 12- and 18-month, Synchrony 15-month |
| [Sponsored savings](https://banking.ratezip.com/savings-accounts) | 6 | Vio three-month promotion, Openbank High Yield Savings boost, Happen LevelUp, Chime Prime, Synchrony High Yield Savings, Marcus Online Savings |
| [Additional editorial CDs](https://banking.ratezip.com/cd-rates) | 5 | Wells Fargo 48- and 60-month, Bank of America 48- and 60-month, Chase 60-month |

## Run the same comparison

Send this `tools/call` to the [public MCP endpoint](https://mcp.ratezip.com/mcp). It uses the server’s default CD terms and prior-snapshot comparison. A new call returns the observations available at the time it runs.

```json
{
  "name": "get_rate_watch_report",
  "arguments": {
    "products": ["savings", "cd"],
    "balance": 10000,
    "top_n": 7,
    "institutions": [
      "Mitten Savings", "CIT", "Capital One", "Forbright", "Happen", "Marcus",
      "Vio", "Chime", "Synchrony", "Openbank", "Bank of America", "Chase",
      "Wells Fargo", "PiBank"
    ]
  }
}
```
