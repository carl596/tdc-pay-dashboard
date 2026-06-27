---
name: metric-trend-triage
description: >-
  Triage whether a movement in a business metric is a real trend or just
  normal month-to-month variation, before reacting to it. Use whenever
  someone points at a number that went up or down — takings, income, pay,
  gross-per-day, ad spend, a per-clinician figure — and asks "is this
  good/bad?", "what changed?", "should we worry?", "why did X drop?", or
  wants a month-over-month / year-over-year comparison explained. Codifies
  four traps that produce false alarms: confusing takings with income,
  not normalising by clinicians working, including partial months, and
  the ad-spend level trap.
---

# Metric trend triage: real trend or normal variation?

A number moved. Before anyone acts on it, run it through this triage. Most
month-to-month swings in a small dental practice are noise, composition
changes, or measurement artefacts — not signal. The job here is to strip
those out so a *real* trend is the only thing left to explain.

Work the four checks **in order** and stop reacting until all four pass.
Each one has killed more false alarms than it has hidden real problems.

## The data you're reasoning about

In this practice's dashboard (`index.html`), per clinician per month:

- **`gross`** — takings / revenue billed (stored in £k).
- **`pay`** — what the clinician was actually paid (income to them).
- **`days`** — days worked that month.
- **`grossPerDay`** = `gross / days` — the normalised productivity figure.

`gross` and `pay` are *different metrics that move for different reasons*.
Keep them straight (Check 1). Totals across clinicians move when headcount
or days move (Check 2). The latest month is usually incomplete (Check 3).

## Check 1 — Takings vs income: which metric actually moved?

"Takings are down" and "income is down" are not the same statement.

- **Takings (`gross`)** is top-line revenue billed. It moves with patient
  volume, treatment mix, and price.
- **Income (`pay`)** is what a person/practice keeps. It moves with the pay
  *formula* (percentage splits, lab deductions, targets), not just takings.

Failure mode: someone sees `pay` drop and concludes the practice is busier
or quieter, when only the split changed; or sees `gross` rise and assumes
everyone earned more, when a deduction ate it. **Always name which series
you mean, and check whether the *other* one moved with it.** If takings and
income diverge, the cause is in the formula or deductions, not in activity —
a completely different investigation.

## Check 2 — Normalise by clinicians working

A practice total (or a "site gross") is a sum over whoever happened to be in
that month. It drops when someone is on leave, started late, or left — with
zero change in underlying performance.

- Never compare raw **totals** across months without checking the headcount
  and **days worked** behind each.
- Prefer **per-clinician** and **per-day** figures (`grossPerDay`). A total
  that fell 20% while `grossPerDay` held flat means *fewer clinician-days*,
  not a performance problem — recruit/roster, don't panic.
- When comparing two clinicians, compare `grossPerDay`, not `gross`. Someone
  working three days a week is not underperforming a full-timer on totals.

Rule of thumb: if a total moved, decompose it into `days × grossPerDay`
before saying anything about productivity.

## Check 3 — Exclude partial / incomplete months

The current month-in-progress, and any month where a clinician worked far
fewer days than usual, will look like a cliff. It isn't — the month just
isn't finished or wasn't fully staffed.

- **Drop the latest month** from trend reads if it's still in progress (the
  dashboard caches monthly data; the newest point is often partial).
- Flag any month whose `days` is well below that clinician's normal as
  **partial** and exclude it from like-for-like comparisons — or compare on
  `grossPerDay`, which is days-normalised and therefore safe to include.
- A "sharp decline" that is entirely the most recent, unfinished month is
  the single most common false alarm. Check the calendar before the chart.

## Check 4 — The ad-spend level trap

Ad spend is a **level (a stock you set), not a per-unit driver**. Two related
errors:

1. **Attributing revenue swings to constant spend.** If ad spend has been
   roughly flat, it cannot explain a month's revenue jump or dip — you're
   reading a level as if it were the cause of a change. Look elsewhere
   (mix, capacity, seasonality).
2. **Reading totals when spend changed.** If spend *did* change, judge it on
   **return per £ spent** (new patients or gross per £ of ad spend), not on
   absolute new-patient counts. More spend usually buys more patients while
   the *efficiency* falls — the metric that matters.

Don't credit or blame advertising for a movement until you've shown spend
actually changed *and* the per-£ return changed with it.

## Output of the triage

After the four checks, classify the movement as one of:

- **Artefact** — partial month, or a totals/headcount composition change.
  Action: none; fix the comparison (normalise / drop the partial month).
- **Normal variation** — within the clinician's usual month-to-month spread
  on the *normalised* figure. Action: note it, don't react.
- **Real trend** — survives all four checks: persists across ≥2–3 complete
  months, shows up in the normalised per-day figure, and isn't explained by
  headcount, the pay formula, or ad spend. Action: investigate the cause and
  flag it.

State plainly which bucket the number falls in and *why*, citing which check
caught it. If the data needed to run a check is missing (e.g. `days` for a
month), say so rather than guessing — an unverifiable trend is not a trend.
