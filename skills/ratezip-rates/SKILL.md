---
name: ratezip-rates
description: Compare published US savings, CD, mortgage and HELOC rates with sources and timestamps, run weekly deposit rate watch comparisons, illustrate home equity payments from explicit assumptions, and research official state auto insurance benchmarks, insurer financial-strength standing, and caller-supplied auto quote comparisons.
---

# RateZip rates

The RateZip connector returns rates observed on institutions' own published rate
pages, FDIC national averages, audited state insurance department (`.gov`)
benchmark tables, verified carrier financial-strength records, and deterministic
payment/quote calculations. Use it for factual comparisons only.

This package connects to `https://mcp.ratezip.com/mcp` and covers all ten
read-only tools below. It does not provide lender or insurer inquiries, binding
quotes, or application submission.

## Which tool to call

| The user wants | Call |
|---|---|
| The best savings or money-market rate | `get_savings_rates`. Pass `balance` when they give one. |
| CD rates | `get_cd_rates` with `term_months` when a term is named. Omit it for every term. |
| A weekly deposit rate watch or snapshot comparison | `get_rate_watch_report` with optional `products`, `cd_terms_months`, `institutions`, `top_n`, `compare_to`, `state`, or `balance`. |
| Mortgage or HELOC rates | `get_mortgage_rates` with `loan_type` (`30-year fixed` or `HELOC`). Pass `loan_amount` for conforming-versus-jumbo context. |
| Home equity loan or HELOC payment scenarios | `compare_home_equity_payments` with one to eight explicitly labeled scenarios: amount borrowed, annual note rate and repayment months. Set HELOC interest-only months and optional repayment rate when supplied. Fees are optional; omitted fees remain unknown. |
| How much more a balance would earn | `calculate_deposit_earnings_difference` with `balance`. Let the two APYs default unless the user names them. |
| A deposit account that meets several requirements at once (balance, holding period, state, no monthly fee, no direct deposit) | `compare_deposit_options` with `balance`, `product` (`savings`, `cd` or `all`), `horizon_months`, and the `require_*` flags the user actually stated. |
| Official state auto insurance price benchmarks or carrier win counts | `get_state_auto_benchmarks` with `state` for one jurisdiction (or omit `state` for the 10-jurisdiction overview). Optional: `coverage_type`, `include_carriers`, `max_results`. |
| Financial strength rating or complaint lookup guidance for an auto insurer | `check_auto_carrier_quality` with `carrier_name` (and optional `state`). |
| Apples-to-apples comparison of auto insurance quotes the user received | `compare_auto_quotes` with `quotes` (1–10 items with `label` and `premium_usd`, plus optional `term_months`, `fees_usd`, `carrier`, liability limits, deductibles, and `um_uim_included`) and optional `current_policy`. |

## How to answer published-rate lookups

1. Lead with the top figure and its institution, then the national average
   from `reference` for context. When the user gave a balance, quote
   `applicable_apy` rather than the headline APY and state every entry in
   `balance_tier_adjustments`.
2. Give each rate's `observed_at` date and link its `source_url`. Mention the
   `conditions` that change what the user would earn: minimums, promotional
   windows, waitlists, balance tiers. In `get_rate_watch_report`, keep money
   market fund 7-day SEC yields separately labeled from bank/credit-union APYs.
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
   unsupported arguments to the other tools. Do not paste raw JSON.
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

## How to answer auto insurance research and quote comparisons

1. **State benchmarks (`get_state_auto_benchmarks`):** Preserve the exact
   jurisdiction, publication year, sample-profile scope, and official `.gov`
   source URL. If a state is outside the 10 covered jurisdictions (CA, TX, MD,
   KY, NV, AL, OK, ND, WV, DC), report the tool's explicit unavailable notice
   and share the NAIC state regulator directory rather than estimating prices.
   Benchmarks are published sample comparisons, not personalized quotes.
2. **Carrier standing (`check_auto_carrier_quality`):** State the verified
   rating agency (AM Best or Demotech) and status (`active`, `withdrawn`, or
   `not rated`), underwriting subsidiary or eligibility notes, and NAIC
   Consumer Insurance Search complaint-index guidance (1.00 national baseline).
   If a carrier is outside the verified registry, report unknown rather than
   estimating a rating.
3. **Quote normalization (`compare_auto_quotes`):** Show annualized and
   monthly-equivalent totals for each supplied quote, call out every coverage
   downgrade flagged against `current_policy` (lower liability limits, higher
   deductibles, or omitted UM/UIM), and treat omitted fees as unknown rather
   than guaranteed zero. These are mathematical comparisons of user-supplied
   inputs, not binding offers or insurer endorsements.
