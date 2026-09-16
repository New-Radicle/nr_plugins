---
name: week
description: >
  Say where the AI adoption program is right now: the current week, its phase
  and theme, what is due this week and who owns it, the last check-in status,
  any scorecard metric At Risk or Behind, and the one skill to run next.
  Read-only; writes nothing. Use when someone says "what week are we on",
  "what's due this week", "where are we in the rollout", "what should I do
  next", "program status", or "preview week 7".
argument-hint: "[week number to preview]"
---

# Week

A one-screen answer to "what week is it, what is due, who owns it". Runs in under a minute. Reads state, never writes it.

Read first: the state folder's `company-context.md` (mode, roles, start date), `companies.csv`, `checkins.csv`, `scorecard.csv`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/program.md`; `${CLAUDE_PLUGIN_ROOT}/skills/adapters/references/files.md` (week rule).

If no state folder exists, say so and point to `/ai-adoption:setup`. Do not run it.

## 0. Compute the week

| Input | Rule |
|---|---|
| `start_date` | Monday of Week 0, from `companies.csv` |
| `program_weeks` | 6 (Founder) or 12 (Team, Department), from `companies.csv` |
| Current week | floor((today - start_date) / 7) |
| Today before start_date | "Pre-program. Week 0 starts on <date>." Show the Week 0 list anyway. |
| Week > program_weeks | "Post-program" (see section 5) |
| `$1` given | Preview that week; label the output "Preview of Week N", still show today's real week in one line |

State the week, the dates it covers (Monday to Sunday), and the mode in one line before anything else.

## 1. Phase and theme

Name the phase from `program.md`. Department mode follows the Team column with the department scope from `company-context.md` in the heading.

| Week | Founder | Team and Department |
|---|---|---|
| 0 | Foundation: setup, assess, map, prioritize in one session | Foundation: align Sponsor, baseline survey, licenses, channel, 1:1s, scorecard baseline |
| 1-2 | First Win: the founder's own three workflows, daily use | First Win: Sponsor kickoff, hands-on workshop, everyone posts one win by day 10 |
| 3-4 | Workflow Layer: automate the top grant or commercial primitive | Workflow Layer: AI-first process per team, role playbooks, first connectors |
| 5-6 | Depth and close: self check-in, playbook revised, impact note | Depth: advanced tools for technical roles, first automations |
| 7-8 | post-program | Depth: office hours (7), mid-point survey (8) |
| 9-12 | post-program | Institutional Layer: handbook (9), use-case library (10), impact report (11), plan next (12) |

## 2. What is due this week and who owns it

Show only the current week's row, then the next week's row under "Coming up". Owners come from the People table in `company-context.md`; print the role and the name recorded there.

**Team and Department mode**

| Week | Due | Owner |
|---|---|---|
| 0 | Sponsor alignment; baseline survey sent and collected; licenses; channel created; 1:1s; `scorecard baseline` | Sponsor (alignment, licenses); Rollout Lead (everything else) |
| 1 | Kickoff message (`kickoff`); workshop scheduled (`workshop`); Sponsor visibly using the tools | Sponsor; Rollout Lead |
| 2 | Workshop held; one win per person by day 10; first `weekly-checkin` | Rollout Lead; team AI leads collect wins |
| 3 | One AI-first process per team named; role playbooks (`playbook`); first-error response scripted with the Sponsor | Rollout Lead; team AI leads |
| 4 | Playbooks distributed; first connectors; at least one `use_cases` row per team | Rollout Lead |
| 5 | Advanced tools for technical roles; first automation chosen with a 4-week scale-or-kill date | Rollout Lead; team AI leads |
| 6 | Automation running; persona-group coverage check (counts only); hidden usage surfaced gently | Rollout Lead |
| 7 | Office hours (`office-hours`) | Rollout Lead; Trailblazers present |
| 8 | Mid-point survey sent and collected; scorecard updated | Rollout Lead |
| 9 | AI handbook draft (`handbook`) | Rollout Lead; compliance owner when the profile is Restricted, Controlled, or Regulated |
| 10 | Use-case library compiled from `team`-visibility rows | Rollout Lead; team AI leads |
| 11 | Impact report (`impact-report`) compiled and presented | Rollout Lead compiles; Sponsor presents |
| 12 | Plan next (`plan-next`); budget and seat decision | Sponsor; Rollout Lead |

Department mode adds: Weeks 3-4 log cross-department dependencies; Weeks 7-8 the department head reports to the company Sponsor; Week 12 the next department is queued.

**Founder mode** (owner is the founder throughout)

| Week | Due |
|---|---|
| 0 | Setup, assess, map, prioritize in one session |
| 1 | Three workflows chosen; `playbook` generated; daily use begins |
| 2 | Daily use; first lines in `wins.md`; first self check-in |
| 3 | Top grant or commercial primitive chosen for automation |
| 4 | Automation running; wins logged |
| 5 | Self check-in; `playbook` revised |
| 6 | Impact note for investors or board (`impact-report`) |

## 3. Status

| Item | Source | Show |
|---|---|---|
| Last check-in | Latest `checkins` row | Week, `ran_on`, `status`, blockers in one line |
| Missed check-ins | Gap between last `checkins.week` and the current week | If two or more weeks have no row, say "no check-in for N weeks" and make the next skill `weekly-checkin` |
| Metrics in trouble | `scorecard` rows for the latest week with status At Risk or Behind | Metric, actual versus expected, status. Nothing else from the scorecard. |
| Program health | Worst Adoption status (`scorecard-model.md`) | One word |
| Sponsor absence | Two consecutive `checkins` rows with a Sponsor-absent blocker | "Sponsor bottleneck; escalate once, then proceed on what does not need the Sponsor" |

Persona information appears only as counts, if at all. Never print a line from `personas.private.md`.

## 4. The one skill to run next

Pick exactly one, in this order of precedence:

| Condition | Next |
|---|---|
| No `companies` row or context placeholders unfilled | `setup` |
| Sponsor not yet confirmed (People table) | `sponsor-brief` |
| Week 0 and no scorecard baseline | `assess-team`, then `scorecard baseline` |
| A check-in is missing for the current or previous week | `weekly-checkin` |
| A due item in section 2 maps to a skill and has no output in `<state>/outputs/` | That skill |
| Otherwise | `weekly-checkin` on the usual day |

## 5. Post-program

When the week exceeds `program_weeks`, say "Post-program, month N" (month = floor((week - program_weeks) / 4) + 1) and list the monthly items:

| Item | Owner | Skill |
|---|---|---|
| Office hours | Rollout Lead | `office-hours` |
| Impact report refresh | Rollout Lead compiles, Sponsor presents | `impact-report monthly` |
| Scorecard update against stretch targets | Rollout Lead | `scorecard update` |
| New use cases folded into playbooks | Team AI leads | `playbook` |

Founder mode post-program: monthly `weekly-checkin` (self) and `impact-report monthly` only.

## Output format

Six short blocks, in this order: week line; phase and theme; due this week (table); coming up (one line); status (table); next skill (one line with the command). Fit on one screen. No preamble.

## Rules

- Read-only. If the user asks to change something, name the skill that writes it.
- Never post, send, or schedule.
- Company facts come only from the state folder; the plugin holds none.
