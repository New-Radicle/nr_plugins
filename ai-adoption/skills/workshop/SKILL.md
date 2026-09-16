---
name: workshop
description: >
  Produce the Week 1-2 hands-on workshop plan: a 60-minute agenda with the
  Sponsor's demo, one per-role exercise per team drawn from that team's top
  primitives with a starter prompt and a "check this before trusting it" line,
  a first-wins posting segment, and the data rules. Adds persona-aware
  facilitation notes using counts only. Use when someone says "plan the
  workshop", "workshop agenda", "hands-on session", "per-role exercises",
  "what do we do in the first training", or "schedule the workshop".
disable-model-invocation: true
argument-hint: "[workshop date]"
---

# Workshop

Hands-on, not a lecture. Every person leaves having produced one output with AI, checked it, and posted it. This is the direct fix for Skills Gap Abyss and the first place Conformists see peers doing it.

Read first: `<state>/company-context.md` (teams, data profile, never-in-prompts list, capabilities, Sponsor); `<state>/primitives.csv` (per team, by priority_rank); `<state>/personas.private.md` (counts only leave this file); `<state>/outputs/kickoff-*.md` for the Sponsor's real use cases; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/ai-modes.md`; `${CLAUDE_PLUGIN_ROOT}/skills/foundations/references/exercises.md` (per-layer exercises to draw from); `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/personas.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/barriers.md`.

| Mode | What changes |
|---|---|
| Founder | Skipped. No workshop; the founder's three workflows are set in `prioritize` and run daily. |
| Team | Full run, every team. |
| Department | Only the teams inside the department scope. Sixty minutes still; if the department has more than six teams, run two sessions. |

## 1. Inputs

Ask for the date (`$1` or next available in Week 1-2), the room or call link, and whether the Sponsor will attend the full hour (the answer changes the facilitation notes). If the kickoff file has no Sponsor use cases, ask for one now; do not invent the demo.

## 2. Agenda

| Time | Segment | Who | Content |
|---|---|---|---|
| 0-10 | Sponsor demo | Sponsor | One real task from the kickoff message, done live, including the check before trusting it |
| 10-45 | Per-role exercises | Everyone, seated by team | One exercise per team (section 3); the Rollout Lead and team AI leads circulate |
| 45-55 | Post first wins | Everyone | Each person posts one line to the channel: task, tool, time, what was checked |
| 55-60 | Data rules | Rollout Lead or compliance owner | The rules from the data profile (section 4), read aloud, no discussion needed |

## 3. Per-team exercises

For each team, take the highest-ranked primitive that is not Excluded and that a person can finish in 30 minutes. Build one row per team:

| Team | Primitive | AI mode | Exercise (one line) | Starter prompt | Check this before trusting it |
|---|---|---|---|---|---|

Shape the starter prompt and the check line by AI mode:

| Mode | Starter prompt shape | Check this before trusting it |
|---|---|---|
| Automation | "Here is [source material]. Produce [output] in [format]. List every fact you used and where it came from." | Every number, name, and citation against the source. Anything not in the source is suspect. |
| Augmentation | "Here are my notes. Draft [document] in my structure. Keep my claims; flag anything you added." | It kept your meaning. Read the flagged additions first. |
| Capture | "Here are raw notes. Structure them into [log format] with fields [list]. Mark anything you inferred." | Compare to the raw input; every inferred field is a guess until confirmed. |
| Analyze | "Here is [data]. Find [pattern]. Show the calculation and reproduce one result I already know." | Reproduce one known value by hand before trusting the rest. |
| Edge | "For [job, site, or equipment], produce a [checklist or report] from [manual or notes]." | It matches the actual equipment, site, and current procedure, not a generic one. |

Exercise material follows the data profile:

| Profile | Material |
|---|---|
| Open | The person's own recent work, minus anything they consider IP |
| Restricted | A sample input each team lead prepares beforehand; nothing on the never-in-prompts list. List the sample per team in the plan. |
| Controlled | Same as Restricted, and only in the approved tool under the confirmed terms. Name the tool in the plan. |
| Regulated | Anonymized sample input; the anonymization step is the first step of the exercise |

## 4. Data rules segment

Five lines, taken from `company-context.md`: what never goes in a prompt, which tools are approved, the anonymization step if Regulated, who to ask (compliance owner or Rollout Lead), and where the error log lives (`<state>/errors.md`, described as "the shared error log", not by path).

## 5. Facilitation notes

Two layers. The shared plan gets counts; only the leads get names.

In the plan, one line per persona present, using counts from `personas.private.md`:

| Count of | Note in the shared plan |
|---|---|
| Trailblazers | Seat one per table; ask them to post first in the wins segment so it is safe for everyone else |
| Self-Starters | Hand them the exercise sheet and leave them alone; no walkthrough |
| Conformists | The Sponsor stays the full hour and does the exercise at a table; this group is watching |
| Skeptics | Give them the reviewer role for a neighbour's output; the check line is theirs to run |
| Traditionalists | Pair each with a Trailblazer, one step at a time, simplest exercise in the room |
| Underground signals noted | Say aloud that usage is welcome and nothing is audited |

Names for pairings and seating go only in a "Workshop pairings YYYY-MM-DD" section appended to `<state>/personas.private.md`, and are read out to the Rollout Lead in chat. They never appear in the plan file.

## 6. Output

Write `<state>/outputs/workshop-YYYY-MM-DD.md`: agenda, exercise table, data rules, shared facilitation notes, and a materials checklist (licenses active for everyone, channel exists, sample inputs ready per team, error log created). End with `AI-assisted draft. Reviewed by: ________ (name, date)`; leave the blank empty. Print it.

Then offer, each under the approval gate, stopping for a yes on each:

| Offer | Operation |
|---|---|
| Calendar invite, 60 minutes, all attendees, the plan file linked in notes | `schedule_event` |
| Two-line channel reminder the day before, from the Rollout Lead | `post_message` |

Append a dated line to "Notes and decisions" in `company-context.md`.

## After the workshop

Tell the Rollout Lead: wins posted in the room are the first `use_cases` rows; `weekly-checkin` collects them. Re-run `map-personas` after the workshop; what people did in the room is better evidence than the 1:1s.

## Rules

- No persona names in the plan file, the invite, or the reminder. Counts only.
- Never invent the Sponsor demo. No demo means the Sponsor does the exercise with a team instead, and the plan says so.
- Nothing on the never-in-prompts list is used in any exercise, including as an example.
- Approval gate before every `schedule_event` and `post_message`; one yes covers one action.
