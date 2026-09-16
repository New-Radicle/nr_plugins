---
name: scorecard
description: >
  View or update the AI adoption scorecard: baseline, target, stretch, actual,
  computed status, and trend since last week for every metric, grouped by
  category, with program health. Also sets the Week 0 baseline after the
  baseline survey. Use when someone says "show the scorecard", "update the
  scorecard", "how are we doing against targets", "set the baseline", "record
  this week's hours saved", or "what's our adoption rate".
disable-model-invocation: true
argument-hint: "view | update | baseline"
---

# Scorecard

The program's evidence. If it is not in the scorecard, it did not happen. Status is computed from the numbers, never chosen.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: the state folder's `company-context.md` (mode, headcount, start date); `scorecard.csv`; `companies.csv`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/scorecard-model.md`; `${CLAUDE_PLUGIN_ROOT}/skills/adapters/references/files.md` (keys and week rule).

Action is `$1`. Default `view`.

## Metric set by mode

| Mode | Metrics | Source |
|---|---|---|
| Founder | Hours saved per week; workflows moved to AI-first; documented wins in `wins.md` | `scorecard-model.md`, Founder table |
| Team, Department | Four categories, fourteen metrics, targets scaled at setup | `scorecard-model.md`, Team table |

Rows are keyed by `company_id` + `week` + `metric`. Seeding happened in setup; if `scorecard.csv` is header-only, say so and offer to seed it now with the mode's metric list and blank baselines.

## Status computation

Applied to every metric with a baseline and target, for the week being viewed or written.

| Step | Rule |
|---|---|
| Expected value this week | baseline + (target - baseline) x (week / program_weeks). Week 0 expected = baseline. |
| Direction | Most metrics are higher-is-better. "Time from idea to published document" and the archetype workflow times are lower-is-better; invert the comparison. |
| Exceeded | actual at or beyond target |
| On Track | actual at or above 85% of expected (for lower-is-better: at or below expected / 0.85) |
| At Risk | 60-85% of expected |
| Behind | below 60% of expected, or no actual recorded for two consecutive weeks |
| No status | baseline or target blank; print "unset" and flag it |
| Program health | Worst status among Adoption metrics (Founder mode: worst of the three) |

Rubric metrics (Team category) are scored by `assess-team`; treat a repeat assessment as the actual and use the same rules on the delta.

## view

Read every `scorecard` row. Show the latest week with an actual per metric, grouped by category.

| Column | Content |
|---|---|
| Metric | Name |
| Baseline | Week 0 value |
| Target / Stretch | From the row |
| Actual | Latest recorded value and the week it was recorded |
| Status | Computed above |
| Trend | Arrow and delta versus the previous week that has an actual: up, down, flat, or "first value" |
| Personas | Counts per persona from `persona_breakdown` when present, for example "Conformist 6, Skeptic 2". Counts only. |

After the table: one line for program health; one line per metric that is At Risk or Behind with the gap to expected; one line saying when the scorecard was last updated and by which skill. If the current week has no actuals and the previous week does not either, say "two weeks without data" and point to `weekly-checkin`.

Founder mode prints three rows and no category grouping.

## update

Ask which metrics changed this week and their values. Batch of two or three questions at most. Then:

| Check | Rule |
|---|---|
| Week | Default the current week; accept another week; never a future week |
| Number | Must parse as a number; percentages 0-100; satisfaction 1-5; counts non-negative integers; hours non-negative |
| Direction sanity | If a value moves more than 50% in one week, repeat it back and ask for a yes before writing |
| Persona breakdown | Optional; accept counts per persona only. If the user types a name, drop it, say why, and keep the count. |
| Unknown metric | Offer the list; do not add metrics here (targets change in setup or with the Sponsor's yes) |

Write with `update_row` when a row for that week and metric exists, else `write_row`, with `actual`, `status` (recomputed), `persona_breakdown`, and `updated` = today. Then print the `view` output for that week and any metric whose status changed.

Changing `target` or `stretch` needs the Sponsor's yes in chat; record it in "Notes and decisions" in `company-context.md`.

## baseline

Week 0 only, after the baseline survey (`assess-team` for rubric scores). Ask for each measured baseline in the mode's list, in batches by category. Rules:

| Metric type | Baseline |
|---|---|
| Measured (weekly use %, sessions, satisfaction, workflow times) | From the survey; write the value |
| Count-from-zero (use cases, workflows, channel posts, migrations, wins) | 0 unless the user says otherwise |
| Rubric scores | From `assess-team`; do not ask again |

Write `baseline` into every Week 0 row and set `updated`. If a baseline already exists, show it and ask before overwriting. Append "Baseline set on <date>" to "Notes and decisions".

## Founder mode

`view` shows three metrics with baseline, target, actual, status, trend. `update` reads "documented wins" by counting lines in `wins.md` rather than asking. `baseline` sets all three to 0 and confirms.

## Rules

- Persona data enters as counts only. Never write a name into `persona_breakdown`, and never read `personas.private.md` to fill it; the user supplies counts.
- Status is never typed by hand. If the user asks to mark something On Track, explain the rule and show the number it would need.
- One row per company, week, and metric. Re-runs update the row.
- Writes nothing outside `scorecard` and the notes line in `company-context.md`. Nothing is posted or sent.
