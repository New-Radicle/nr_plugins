---
name: impact-report
description: >
  Compile the AI adoption impact report the Sponsor presents: results against
  targets, hours saved and FTE-equivalent with assumptions labelled, the five
  best use cases, persona migration counts, what did not work and why, the
  three defining moments as they happened, and recommendations for the next
  plan. Week 11 in Team mode, Week 5-6 in Founder mode, monthly after. Use
  when someone says "write the impact report", "what did the program deliver",
  "board note on AI adoption", "monthly impact refresh", or "results for the
  Sponsor to present".
disable-model-invocation: true
argument-hint: "[monthly]"
---

# Impact report

The document the Sponsor stands behind. Every number traces to a table in the state folder or to a labelled assumption in `company-context.md`. Nothing is rounded up.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: the state folder's `company-context.md` (mode, headcount, economics assumptions, people); `companies.csv`; `scorecard.csv`; `use_cases.csv`; `checkins.csv`; `primitives.csv`; `wins.md`; `errors.md` if present; earlier `<state>/outputs/impact-report-*.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/scorecard-model.md`, `defining-moments.md`, `failure-modes.md`, `personas.md`.

Do not read `personas.private.md` for this skill; migration counts come from `scorecard` and `checkins` rows.

## When it runs

| Trigger | Scope |
|---|---|
| Team or Department mode, Week 11 | Full program report, Weeks 0-11 |
| Founder mode, Week 5-6 | One-page note for investors or a board |
| `$1` = `monthly`, post-program | Refresh: same structure, period = since the last report, targets replaced by stretch |
| Run early (before Week 9 in Team mode) | Warn that the data is partial, label the report "interim", continue if asked |

If `checkins.csv` has fewer than four rows in Team mode, say the report will be thin and name the gap before writing.

## Structure

| # | Section | Source | Rule |
|---|---|---|---|
| 1 | Headline | Scorecard Adoption metrics | One sentence in operating terms: what the company does differently now. Then a three-line table: active use %, hours saved per person per week, documented use cases, each as baseline to actual against target. |
| 2 | Results against targets | `scorecard` latest week, all categories | Table: metric, baseline, target, actual, status. Status as computed by `scorecard`; never restated. Unset or two-weeks-silent metrics shown as such. |
| 3 | Hours and FTE-equivalent | `checkins.hours_saved_avg`, headcount, economics assumptions | See the computation below. Every input labelled "measured" or "assumption". |
| 4 | Five best use cases | `use_cases` with `visibility` = `team` | Rank by `hours_saved`, then by whether the task was previously not done at all. Author name only if the row is `team`. Never a `leads` or `private` row, even anonymized. Fewer than five: show what exists and say so. |
| 5 | Persona migration | `scorecard` persona metric and `checkins.persona_shifts` | Counts only, for example "2 people moved one step". No names, no labels attached to individuals. |
| 6 | What did not work | `checkins.blockers`, flags raised in check-ins, `errors.md` | Group by failure mode. One line each: what happened, what was tried, current state. Honest; this section is what makes the rest credible. |
| 7 | The three defining moments | `checkins`, `errors.md`, `wins.md` | For each: did it happen, in which week, how leadership responded, what changed after. If one has not happened, say so; do not invent it. |
| 8 | Recommendations | Sections 2-7 | Three to five, each tied to a metric or a blocker, phrased as options for `plan-next` (continue, expand to a department, add a connector, retire a tool). No budget figures unless in `company-context.md`. |
| 9 | Attribution | `frameworks/SKILL.md` | The attribution line, exactly as written there, with the reviewed-by blank left empty |

## Hours and FTE computation

| Line | Formula | Label |
|---|---|---|
| Weekly hours saved, team | mean of `hours_saved_avg` over the period x headcount (or active count if the user prefers) | measured x measured |
| Program hours saved | weekly hours x weeks with a check-in row | measured |
| FTE-equivalent | weekly hours / hours per FTE-week (default 40; `company-context.md` may override) | assumption |
| Annualized value | FTE-equivalent x loaded cost per FTE from `company-context.md` | assumption; "verify" if the cost is a regional default |
| Tooling cost | Seats x price for the period, from `company-context.md` or the user | user input; "verify current pricing" |
| Net | Annualized value minus annualized tooling cost | derived from assumptions |

Print the assumptions block under the table. If the loaded cost is blank, print the FTE-equivalent and stop there; do not guess a salary. Speedup bands from `ai-modes.md` are not used here; the report uses measured hours only.

## Founder mode

One page, for investors or a board:

1. What changed in six weeks, one paragraph in the company's terms.
2. Three metrics: hours saved per week, workflows moved to AI-first, wins logged, each as baseline to actual against target.
3. The three best wins from `wins.md`.
4. One thing that failed and what was learned.
5. What happens next quarter, three lines.
6. `AI-assisted draft. Reviewed by: ________ (name, date)`, blank left empty.

No FTE-equivalent unless the founder asks; hours are enough at this size.

## Monthly refresh

Read the previous report. Period is from its date to today. Show every metric as previous report to now, against stretch targets. Section 4 lists use cases added since the last report. Section 7 becomes "moments since last report" and may be empty. Keep it to half the length of the program report.

## Output

Write `<state>/outputs/impact-report-YYYY-MM-DD.md` (add `-monthly` before the date suffix on a refresh). Print it in full. Append a dated line to "Notes and decisions" in `company-context.md`.

Then offer delivery:

| Adapter | Offer |
|---|---|
| Inbox adapter in use | `send_report(recipients, doc)` to the Sponsor, and to others the Rollout Lead names. Show recipients and the covering text; stop for a yes. |
| Channel adapter in use | A two-line post with a link or the headline numbers only; approval gate. |
| None | The file path and a one-line covering note to paste. |

The Sponsor presents the report; the skill never sends it to the whole team on its own.

Suggest next: `/ai-adoption:plan-next` in Week 12 (Team mode); `weekly-checkin` monthly and `impact-report monthly` (Founder mode and post-program).

## Rules

- Every figure comes from a state table or a labelled assumption. If a number cannot be traced, leave it out and say what is missing.
- `team`-visibility use cases only. Only the author changes visibility; the report never asks them to.
- Persona data as counts only; nothing from `personas.private.md`.
- Section 6 is never skipped or softened. A report with no failures is not credible.
- Never fill the reviewed-by blank. Never send without the approval gate; one approval covers one send.
