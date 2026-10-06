---
name: ratezip-rates
description: Compare live US savings, CD, mortgage and HELOC rates with source and timestamp on every figure. Use when the user asks for the best savings or money-market APY, CD rates for a term, today's mortgage or HELOC rates, how much more a balance would earn at a better rate, or whether a loan amount is conforming or jumbo.
---

# RateZip rates

The RateZip connector returns rates it observed on each institution's own
published rate page, plus FDIC national averages. Use it for factual
comparisons only.

## Which tool to call

| The user wants | Call |
|---|---|
| The best savings or money-market rate | `get_savings_rates`. Pass `balance` when they give one. |
| CD rates | `get_cd_rates` with `term_months` when a term is named. Omit it for every term. |
| Mortgage or HELOC rates | `get_mortgage_rates` with `loan_type` (`30-year fixed` or `HELOC`). Pass `loan_amount` for conforming-versus-jumbo context. |
| How much more a balance would earn | `calculate_deposit_earnings_difference` with `balance`. Let the two APYs default unless the user names them. |

## How to answer

1. Lead with the top figure and its institution, then the national average
   from `reference` for context. When the user gave a balance, quote
   `applicable_apy` rather than the headline APY and state every entry in
   `balance_tier_adjustments`.
2. Give each rate's `observed_at` date and link its `source_url`. Mention the
   `conditions` that change what the user would earn: minimums, promotional
   windows, waitlists, balance tiers.
3. Compare mortgage rates only within one `loan_type`, and show APR beside the
   note rate. A `loan_amount` answer is the conforming-versus-jumbo context the
   tool returns, nothing more.
4. If `observed` is empty, say so and offer the `available_terms` or
   `available_loan_types` the result names instead of guessing.
5. Show the top three to five rows unless the user asks for the full list. For
   a long comparison, call with `max_results` and `include_provenance: false`.
   Do not paste the raw JSON.
6. Do not recommend an institution, estimate a personal rate, imply approval,
   or offer to open an account. When the user asks what they should do with
   their money, say that this is rate data, not financial advice.
