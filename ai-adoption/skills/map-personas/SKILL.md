---
name: map-personas
description: >
  Assign each team member an adoption persona (Trailblazer, Self-Starter,
  Conformist, Skeptic, Traditionalist) from the Rollout Lead's 1:1 notes, with
  confidence, signals, and one suggested first task tied to their team's
  primitives. Writes only to the private personas file and emits counts for
  the scorecard. Use in Week 0 after the 1:1s, or when someone says "map the
  personas", "who is a skeptic", "which persona is this person", "update the
  persona map", "record a persona migration", or "how many traditionalists do
  we have".
disable-model-invocation: true
argument-hint: "[path to 1:1 notes]"
---

# Map personas

Private work for the leads. The output is a table nobody outside the Sponsor and Rollout Lead sees. It shapes the workshop, the playbooks, and the coaching, and it feeds the scorecard as counts only.

Read first: `<state>/company-context.md`; `<state>/primitives.csv`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/personas.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/barriers.md`; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/<archetype>.md` (persona prior); `<state>/personas.private.md` if it already has rows.

| Mode | What changes |
|---|---|
| Founder | Skipped. One person does not need a persona map; the founder's own three workflows come from `prioritize`. |
| Team | Full run for everyone in the program. |
| Department | Only people inside the department scope. Other departments are mapped when their own program starts. |

## 1. Inputs

Ask the Rollout Lead for the 1:1 notes (`$1` or pasted). Where notes are thin or missing for a person, ask the Rollout Lead these three questions about that person, in one message, up to five people at a time:

| # | Question | What it reveals |
|---|---|---|
| 1 | How have they reacted to AI so far, from what you have seen them say and do? | Motivation and current usage, including the underground pattern |
| 2 | How do they usually learn a new tool? | Whether they need docs, a group, a person beside them, or nothing |
| 3 | What are they proud of in their work? | The task never to start with, and the craft to respect |

Never ask the team members themselves, and never send a survey for this. The map is the leads' judgment, not a self-report; a self-report would make the persona visible to the person and defeat the privacy rule.

## 2. Assignment

Match the answers to the signals below. Use the archetype's persona prior only to break ties.

| Persona | Reaction so far | Learns by | Proud of |
|---|---|---|---|
| Trailblazer | Already using it, unprompted, for several things | Trial and error; reads release notes | Being first; finding a better way |
| Self-Starter | Tried it on their own terms; bristles at being told to | Docs, alone, at their own pace | Mastery; doing it well without help |
| Conformist | Waiting to see what leadership and peers do | Group session; following the team's lead | Reliability; delivering what was asked |
| Skeptic | Asked about accuracy, security, or quality before trying | Evidence and a side-by-side first | Quality; catching errors; protecting the standard |
| Traditionalist | Avoids it; "the current way works" | Step by step with someone next to them | Consistency; the same good output for years |

Confidence: High when all three answers point the same way; Medium when two do; Low when one does or the persona is inferred from role alone. Flag the underground pattern (polished output faster than before, deflects when asked how) as a note on the row, not as a persona.

## 3. First task per person

Read `primitives` and filter to the person's team. Pick one primitive that is not Excluded and prefer status Weak, Absent, or Ad hoc. Then apply the persona rule:

| Persona | First task rule |
|---|---|
| Trailblazer | The hardest primitive on the list, plus "document what worked" for the channel |
| Self-Starter | Any primitive they choose; ask them to write the one-page playbook for it |
| Conformist | The same task the Sponsor demonstrated at kickoff, in their team's version |
| Skeptic | The task they said they hate. Never the one they are proud of. They review the output. |
| Traditionalist | The simplest Automation primitive, one step, paired with a named Trailblazer |

Write the task as one line: primitive name, what to produce, what to check before trusting it.

## 4. Write the private file

Write one row per person to `<state>/personas.private.md`, columns as in the template: Person, Role, Persona, Confidence, Signals, First task suggestion, Last reviewed (today). Do not use `write_row`; this file is never a table in a shared state store, even when a connector is the state store for everything else.

Then say, every time: keep `personas.private.md` out of shared drives and out of any folder sync; the state README says the same.

## 5. Counts to the scorecard

Compute counts per persona, for example `{"Trailblazer": 3, "Self-Starter": 2, "Conformist": 7, "Skeptic": 3, "Traditionalist": 1}`. Then `update_row('scorecard', {week: 0, metric: "Persona migrations (count only)"}, {baseline: 0, persona_breakdown: <counts>})`. On the first run that row is the Culture baseline. Counts only; no names, no signals, no tasks reach any table.

Print the counts and what they imply, using `personas.md`: which group is largest, whether Conformists will need visible Sponsor use, how many pairings the workshop needs.

## 6. Re-runs and migrations

On a later run, for each person compare the new assignment to the existing row.

| Change | Action |
|---|---|
| Same persona | Update Signals and Last reviewed |
| One step forward (Skeptic to Self-Starter, Traditionalist to Conformist, Conformist to Self-Starter) | Update the row and add a line under a "Migration log" heading: date, previous persona, new persona, evidence |
| One step back | Same log line; tell the Rollout Lead it usually follows a badly handled first error (`defining-moments.md`) |

Then `update_row('scorecard', {week: <current>, metric: "Persona migrations (count only)"}, {actual: <forward migrations since Week 0>, persona_breakdown: <new counts>})`. When `weekly-checkin` asks, give it the forward-migration count for `persona_shifts` as a number, nothing more.

## Rules

- Never post, send, or write persona names anywhere except `personas.private.md`.
- Never ask a person their own persona, and never tell one.
- If the Rollout Lead wants to share the map with a team lead, state the privacy rule and offer counts for that team instead.
- If a name appears next to a persona in any other skill's shared output, that is a bug; remove it and say so.
- Assignments are working hypotheses. Say so, and re-run after the workshop and at Week 8.
