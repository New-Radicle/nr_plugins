---
name: office-hours
description: >
  Build the Week 7 office-hours agenda, and the monthly one after that, from
  the blockers in recent check-ins grouped by theme and failure mode, with one
  live demo per top blocker, a reserved segment for the questions of people
  who want proof first, and a walk-through of the shared error log. Use when
  someone says "plan office hours", "what are people stuck on", "office hours
  agenda", "monthly AI clinic", or "schedule the week 7 session".
disable-model-invocation: true
argument-hint: "[session date]"
---

# Office hours

The mid-point release valve. Blockers that have been sitting in check-ins for six weeks get a live demo, and the people who have been asking hard questions get the floor, framed as what they are: the quality gate.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: `<state>/company-context.md`; `<state>/checkins.csv`; `<state>/use_cases.csv`; `<state>/errors.md`; `<state>/scorecard.csv` (current week); `<state>/personas.private.md` (counts only); `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/failure-modes.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/barriers.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/defining-moments.md`.

| Mode | What changes |
|---|---|
| Founder | Skipped. The Week 5-6 self check-in covers what the founder is stuck on. |
| Team | Full run at Week 7, then monthly after the program. |
| Department | Blockers from the department only. If a cross-department dependency is logged in check-ins, invite that department's lead for the demo that touches it. |

## 1. Gather blockers

`query('checkins', week between current-6 and current)` and read the `blockers` and `wins` columns. If a channel adapter is in use, `read_recent(42)` for anything the check-ins missed. If blockers are mostly empty, say that the check-in has not been collecting them and ask the Rollout Lead to paste what they have heard; an empty blockers column is itself a Measurement Void signal.

Also look for silent blockers: a team with zero `use_cases` rows since Week 3 is stuck even if nobody said so.

## 2. Group and diagnose

| Theme | Mentions | Weeks seen | Failure mode | Barrier | Demo candidate |
|---|---|---|---|---|---|

Map themes with these shortcuts, then confirm against `failure-modes.md`:

| What people say | Failure mode | Barrier |
|---|---|---|
| "Don't know what to use it for" | Skills Gap Abyss | none |
| "It doesn't connect to [system]" | Integration Wall | none |
| "The output was wrong" or "I can't trust it" | Trust Deficit | Opacity, nuance |
| "No time" or "not my job" | Culture Collision, or Conformists waiting for a signal | Preference for people |
| "Too many tools" or "which one" | Crowdsource Trap | none |
| "It's generic" | Skills Gap Abyss | Inflexibility |
| "What happens to my role" | escalate to the Sponsor | Autonomy threat |

Rank themes by mentions times weeks seen. The top three get demos.

## 3. Demos

One per top theme, ten minutes, live, on real material allowed by the data profile. Prefer a demo by the person who solved it: search `use_cases` for a row matching the theme's primitive with `visibility` of team or shared, and ask the Rollout Lead to invite that author. Fall back to the Rollout Lead using the starter prompt from the workshop plan. Each demo ends with its "check this before trusting it" line said aloud.

## 4. Agenda

| Time | Segment | Who | Content |
|---|---|---|---|
| 0-5 | Where we are | Rollout Lead | Three scorecard numbers, count of wins since Week 1, count of errors logged |
| 5-35 | Three demos | Authors or Rollout Lead | One per top theme, in rank order |
| 35-50 | Proof first | Anyone; Sponsor answers role questions | Questions from people who want evidence before trusting an output. Framed as the quality gate: these questions are how the team keeps its standard. |
| 50-58 | The error log | Rollout Lead | Two or three entries: what was wrong, how it was caught, what changed |
| 58-60 | One change | Rollout Lead | The single thing that changes next week, recorded for the next check-in |

The "Proof first" segment is the Skeptics' segment, but the agenda never calls it that. Collect questions in advance through the weekly check-in or the channel, anonymous by default. If the private file shows Skeptics above a fifth of the team, extend the segment to twenty minutes and shorten the demos.

If `errors.md` is empty at Week 7, say so plainly: it means errors are not being reported, not that there were none. Replace the walk-through with "how to log one", and ask the Sponsor to log the first entry from their own work.

## 5. Output

Write `<state>/outputs/office-hours-YYYY-MM-DD.md`: the theme table (without the barrier column, which is for the leads), the agenda, the demo list with starter prompts, and the pre-collected questions. End with `AI-assisted draft. Reviewed by: ________ (name, date)` (the attribution line, blank left empty). Print it.

Offer, each under the approval gate:

| Offer | Operation |
|---|---|
| Calendar invite, 60 minutes, all attendees | `schedule_event` |
| Channel post announcing the session and asking for questions in advance | `post_message` |

Append a dated line to "Notes and decisions" in `company-context.md`.

## Monthly re-runs

After Week 12, run again each month. The window is everything since the previous office-hours file in `<state>/outputs/`. Carry unresolved themes forward and mark how many sessions they have appeared in; a theme at three sessions is escalated to the Sponsor as a tooling or budget decision, not another demo.

## Rules

- Counts only from the private file; no persona is named in the agenda, the invite, or the post.
- No question is treated as resistance. If the Rollout Lead wants to skip the Proof first segment, say why it exists and keep it.
- Approval gate before every `schedule_event` and `post_message`.
- Demos use only material the data profile allows; never anything on the never-in-prompts list.
