---
name: plan-next
description: >
  Write the Week 12 options memo: three options (sustain, expand to the next
  team or department, deepen automation), each with cost, owner, hours, and
  what the scorecard must show by month 6; a recommendation; and the
  post-program cadence. Reads every table plus budget actuals. Use when
  someone says "what's next after the program", "plan next phase", "options
  memo", "should we expand to another department", "post-program cadence", or
  at Week 12.
disable-model-invocation: true
---

# Plan next

The program ends; the practice continues. This memo gives the Sponsor a decision to make in one sitting, with the evidence on one page and the three honest options behind it.

Read first: `<state>/company-context.md`; `<state>/companies.csv`; `<state>/scorecard.csv` (all weeks); `<state>/primitives.csv`; `<state>/use_cases.csv`; `<state>/checkins.csv`; `<state>/errors.md`; the most recent `<state>/outputs/impact-report-*.md` if present; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/scorecard-model.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/program.md`; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/<archetype>.md`.

| Mode | What changes |
|---|---|
| Founder | Skipped. The Week 6 impact note ends with one paragraph on what continues; that is the plan. |
| Team | Full run at Week 12. |
| Department | Full run, plus the next department is queued and its company-context scope is drafted (section 5). |

## 1. Evidence

Build the table from the latest scorecard week, status computed by the rules in `scorecard-model.md`:

| Category | Metric | Baseline | Target | Actual (Week 12) | Status |
|---|---|---|---|---|---|

Add four lines under it: documented use cases by team, primitives whose status improved since Week 0, errors logged and how many led to a workflow change, and forward persona migrations as a count only.

## 2. Budget actuals

Ask the Rollout Lead, one message: seats and licenses paid over 12 weeks; API or usage spend; Rollout Lead and Sponsor hours per week, actual not planned; any external cost. Compare to the sponsor brief's estimate if `<state>/outputs/sponsor-brief-*.md` exists and say where it was wrong.

## 3. Three options

One table, one column per option. Never omit an option because it seems obvious; the Sponsor needs to see what was not chosen.

| Row | Sustain | Expand | Deepen |
|---|---|---|---|
| What it is | Keep current workflows; monthly cadence; no new scope | Run the 12-week program in the next team or department | Move the top Augmentation primitives toward Automation; add connectors and first automations for the top three workflows |
| Cost | Seats at current count; API at current run rate | Seats for the new group; Rollout Lead time doubles for 12 weeks | Connector or tooling spend; engineering or vendor hours; seats unchanged |
| Owner | Rollout Lead | Sponsor for the new group, plus a new Rollout Lead inside it | Rollout Lead plus one technical owner |
| Hours per week | 2-3 for the Rollout Lead, 30 minutes for the Sponsor | 3-5 for each Rollout Lead, 1-2 for the Sponsor in the new group's Weeks 0-2 | 4-6 for the technical owner in month 1, then 2 |
| Scorecard by month 6 | Every Adoption metric holds at target; Impact at stretch | The new group at Week 12 targets; the original group holds | Hours saved per person at stretch; two or more workflows moved to Automation with success rate above the default |
| Risks | Adoption decays without visible wins; the underground pattern returns | Rollout Lead stretched; the first group loses attention | Integration Wall; over-trust as review steps are removed |
| Choose when | Any Adoption metric is At Risk or Behind | Program health On Track or better and another group has Weak or Absent primitives | Adoption Exceeded and Automation-mode primitives are still run by hand |

Fill every cell with this company's numbers: seat count, current spend, named teams from `primitives`, the actual scorecard status. Costs are estimates; label them and say "verify current pricing".

## 4. Recommendation

Pick one option using the "Choose when" row and state the rule that fired. Give the first three actions, each with an owner and a date in the next 30 days. If two options tie, recommend Sustain plus the cheapest first step of the other, and say why.

## 5. Post-program cadence

| Cadence | What runs | Owner |
|---|---|---|
| Monthly | `weekly-checkin` in monthly mode: use cases, hours, blockers, one digest | Rollout Lead |
| Monthly | `office-hours` | Rollout Lead |
| Quarterly | `impact-report` refresh; scorecard stretch targets reviewed | Rollout Lead, presented by the Sponsor |
| Month 3 | `map-personas` re-run, private | Rollout Lead |
| Month 6 | `handbook` re-run; the diff is the change log | Rollout Lead, compliance owner |

Department mode adds: queue the next department. Draft its scope as a "Department scope" block matching the `company-context.md` template (department, Sponsor as department head, candidate Rollout Lead, teams, dependencies already logged in `checkins`), and write it to `<state>/outputs/next-department-scope-YYYY-MM-DD.md`. Do not run `setup`; the new department's leads run it with this file in hand.

## 6. Output

Write `<state>/outputs/plan-next-YYYY-MM-DD.md`: evidence, budget actuals versus estimate, options table, recommendation, cadence. End with `AI-assisted draft. Reviewed by: ________ (name, date)` (the attribution line, blank left empty). Print it.

Offer, under the approval gate, `send_report` to the Sponsor. When the Sponsor decides in chat, `update_row('companies', {id}, {status: "post-program: <option>"})` and append a dated line to "Notes and decisions" with the option chosen, the owner, and the first review date.

## Rules

- Never invent budget actuals; if the Rollout Lead does not have them, the memo says "not collected" and the cost rows carry estimates only.
- Persona data appears as one count; no names, no breakdown by team smaller than five people.
- The recommendation follows the rules in section 3. If the Sponsor prefers another option, record the decision and the reason; do not rewrite the evidence to fit.
- Approval gate before `send_report`.
