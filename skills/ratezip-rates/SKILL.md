---
name: ratezip-rates
description: Compare published US savings, CD, mortgage and HELOC rates with sources and timestamps, or illustrate home equity payments using explicit amounts, note rates, terms and fee assumptions. Use for rate research, deposit earnings, conforming-versus-jumbo context, and home equity loan or HELOC payment scenarios.
---

# RateZip rates

The RateZip connector returns rates it observed on each institution's own
published rate page, plus FDIC national averages. Use it for factual
comparisons only.

This package connects to `https://mcp.ratezip.com/mcp` and covers the six
tools below. It does not provide lender inquiries or application submission.

## Which tool to call

| The user wants | Call |
|---|---|
| The best savings or money-market rate | `get_savings_rates`. Pass `balance` when they give one. |
| CD rates | `get_cd_rates` with `term_months` when a term is named. Omit it for every term. |
| Mortgage or HELOC rates | `get_mortgage_rates` with `loan_type` (`30-year fixed` or `HELOC`). Pass `loan_amount` for conforming-versus-jumbo context. |
| Home equity loan or HELOC payment scenarios | `compare_home_equity_payments` with one to eight explicitly labeled scenarios: amount borrowed, annual note rate and repayment months. Set HELOC interest-only months and optional repayment rate when supplied. Fees are optional; omitted fees remain unknown. |
| How much more a balance would earn | `calculate_deposit_earnings_difference` with `balance`. Let the two APYs default unless the user names them. |
| A deposit account that meets several requirements at once (balance, holding period, state, no monthly fee, no direct deposit) | `compare_deposit_options` with `balance`, `product` (`savings`, `cd` or `all`), `horizon_months`, and the `require_*` flags the user actually stated. |

## How to answer published-rate lookups

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
   For `compare_deposit_options`, report each requirement's status as the
   tool gives it: a published match, a conflict, or unknown. Unknown is not a
   match; tell the user which terms still need verification with the bank.
5. Show the top three to five rows unless the user asks for the full list.
   For savings, CD and mortgage lookups, use `max_results` and
   `include_provenance: false` when a compact result is enough. Do not pass
   those arguments to the other tools. Do not paste the raw JSON.
6. Do not recommend an institution, estimate a personal rate, imply approval,
   or offer to open an account. When the user asks what they should do with
   their money, say that this is rate data, not financial advice.
7. When the result includes `compare_link`, end with a link labeled
   "Compare on RateZip" and preserve the returned URL exactly, including
   its query string. Describe it as a comparison of observed rates, not an
   offer, recommendation or application.

## How to answer payment scenarios

Ask for missing amounts, note rates and repayment terms, or explicitly agree
on hypothetical assumptions. Do not silently use a published rate as a
personal quote. State the supplied rate and fee assumptions; omitted fees
are unknown, not zero. Financed fees increase the modeled balance. Show both
interest-only and repayment payments for a HELOC, the change between them,
and the modeled term and interest. Preserve the tool's limitations, including
constant rates within each phase and no additional draws. These are payment
illustrations, not lender offers, borrower eligibility decisions or APRs.
Do not rank a lender as best from these calculations or infer FICO/LTV adjustments.
