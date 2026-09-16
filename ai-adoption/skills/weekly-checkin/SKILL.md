---
name: weekly-checkin
description: >
  Run the weekly loop of the AI adoption program: collect use cases, hours,
  and blockers; update the scorecard; diagnose the active failure mode;
  recommend one intervention; draft the team digest for approval; write the
  week's check-in row. Idempotent: safe to run late or twice. Use on the
  scheduled check-in day, or when someone says "run the weekly check-in",
  "check-in for week 5", "log this week's wins", "what's blocking adoption",
  or "draft the digest".
disable-model-invocation: true
argument-hint: "[week number]"
---

# Weekly check-in

The loop from `program.md`: collect, update, diagnose, act, digest. One row per company and week. Takes 15 minutes in Team mode, 5 in Founder mode. Ask in batches of two or three questions.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: the state folder's `company-context.md`; `companies.csv`; `checkins.csv`; `scorecard.csv`; `use_cases.csv`; `wins.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/program.md`, `scorecard-model.md`, `failure-modes.md`, `personas.md`, `defining-moments.md`; `${CLAUDE_PLUGIN_ROOT}/skills/adapters/SKILL.md` (detection, approval gate).

## 1. Load context and the week

Week = `$1` if given, else the current week from `start_date` (rule in `adapters/references/files.md`). Post-program weeks run the same loop and label the row "post-program month N".

| Check | Action |
|---|---|
| A `checkins` row for this week exists | Say when it ran and what it recorded. Continue; the row will be updated and `status` set to `rerun`. |
| No row for the previous week | Note the gap; offer to run that week first. |
| Headcount in `company-context.md` is 10 or more and `size_mode` is Founder | Print the upgrade prompt from section 10 before anything else. |
| Sponsor absent (no Sponsor action recorded) for two consecutive rows | Carry the "sponsor bottleneck" flag into diagnosis. |

Name the phase and this week's expectations from `program.md` in one line.

## 2. Collect

| Situation | How |
|---|---|
| Inbox adapter in use and a survey was sent this week | `collect_replies(since=Monday)`; parse hours, active use, use cases, blockers per reply |
| Channel adapter in use | `read_recent(7)` for wins and questions; count posts |
| Neither, or replies incomplete | Ask the questions for the phase below, two at a time. Pasted replies are parsed the same way. |

| Phase (Team weeks) | Questions |
|---|---|
| First Win (1-2) | Has everyone had one positive interaction? How active was the channel (posts this week)? Who is racing ahead and who is holding back (counts, not names, for the record)? What concerns came up in 1:1s? Hours saved, roughly, per person? |
| Workflow Layer (3-4) | Which AI-first processes are defined and for which teams? Which playbooks are written and distributed? Any connectors switched on? Any pushback about quality or craft? New use cases? |
| Depth (5-8) | How many use cases this week and hours saved? Has the first error happened and how was it handled? Any sign of hidden usage (quiet channel, faster polished outputs)? Week 8: mid-point survey results? |
| Institutional (9-12) | Handbook status? Use-case library status? Impact data gaps? Recommendations forming for next quarter? |

Always end with: "Any blocker the Sponsor needs to know about?"

## 3. Record use cases and wins

For each new use case: ask for author, role, task, tool, prompt summary, hours saved, and visibility (`team`, `leads`, `private`; default `team`, the author decides). `write_row("use_cases", ...)` with id `uc-<n>`, `week`, `primitive_id` if it maps, `added` = today. Re-runs skip use cases already recorded this week for the same author and task.

Wins that are not use cases (a shared post, a first-time try) go to `wins.md` as one line each.

## 4. Update the scorecard

Write actuals for this week with `update_row` or `write_row` and recompute status with the rules in `scorecard-model.md` (same computation as `/ai-adoption:scorecard`).

| Metric | From |
|---|---|
| % using AI weekly, sessions per person | Replies or license data the user pastes |
| Documented use cases | Count of `use_cases` rows to date |
| AI-first workflows | Count reported in Workflow Layer questions |
| Hours saved per person per week | Mean of reported hours |
| Channel posts per week | `read_recent` count or the user's count |
| Persona migrations | Count only, from the leads |

Metrics not mentioned keep their last actual; two silent weeks make them Behind by rule.

## 5. Diagnose

Apply the shortcuts from `failure-modes.md` and `personas.md`. Flag every match explicitly, by name.

| Signal this week | Flag |
|---|---|
| Channel quiet (posts below target) and usage high | Underground pattern |
| Pushback framed as quality, craft, or "mine are fine" | Culture Collision; prepare the craft-challenge response |
| Week 4 or later and zero `use_cases` rows | Measurement Void (plus Skills Gap Abyss if usage is flat) |
| Week 6 or later and a persona group at 0% weekly use (counts from the leads) | Uneven adoption; name the group, never a person |
| Sponsor absent two consecutive weeks | Sponsor bottleneck; escalate once, then proceed |
| Over-checking or over-trusting reported | Trust Deficit |
| A pilot past its 4-week date with no scale-or-kill decision | Pilot Purgatory |
| The first error reported | Defining moment 1; check it was answered publicly and logged in `errors.md` |
| Something previously impossible now done | Defining moment 3; make it visible this week |

## 6. Recommend one intervention

One, tied to the week and the flag. Use the fix column in `failure-modes.md` or the persona guidance. State who does it and by when. If nothing is flagged, the intervention is the next due item from `/ai-adoption:week`.

## 7. Digest (Team and Department mode)

Draft for the team channel, under 150 words:

| Part | Content |
|---|---|
| Wins | Two or three, from `team`-visibility use cases only, with the author's name only if the row's visibility is `team` |
| One tip | A prompt pattern or check drawn from this week's use cases or the playbook |
| Next week's ask | The one thing everyone does, from the program calendar |

No persona names or labels. No blockers, no scorecard numbers unless the Rollout Lead asks for them. Show the exact text and the channel, then stop. On a yes from the Sponsor or Rollout Lead in this chat, `post_digest`; record `posted`. On no, edits, or no channel adapter, print it under "Paste this into your team channel" and record `drafted`. One approval covers this one post.

## 8. Write the check-in row

`write_row("checkins", ...)` or `update_row` on the existing week with: `company_id`, `week`, `ran_on` = today, `hours_saved_avg`, `active_pct`, `blockers` (short, no names from `personas.private.md`), `wins` (count and one line), `persona_shifts` (counts only, for example "Skeptic to Self-Starter: 1"), `digest_text` with a leading `[posted]` or `[drafted]` tag, `status` = `done` or `rerun`.

Append one dated line to "Notes and decisions" in `company-context.md` only when a flag was raised or a defining moment happened.

## 9. Preview next week

Print next week's phase, due items, and owner from `program.md` in three lines. Say when the next check-in is expected.

## Founder mode

Three questions, one message: what saved you time this week and roughly how many hours; which of your three workflows did you actually use; what did you avoid or where did it fail. Then: append wins to `wins.md`; update the three metrics; run the diagnosis (Culture Collision and Trust Deficit still apply to one person); recommend one change to a workflow or prompt; write the `checkins` row with `digest_text` empty. No digest, no channel, no survey. Weeks 5-6: point to `playbook` for the revision and `impact-report` for the note.

## 10. Founder-to-Team upgrade prompt

When headcount in `company-context.md` is 10 or more and the mode is Founder, print before the check-in:

"Headcount is now N. This program was set up in Founder mode (6 weeks, three metrics, no channel). Team mode adds a Rollout Lead separate from the Sponsor, a 12-week calendar, a team channel, and the full scorecard. Upgrade? If yes, run `/ai-adoption:setup` and choose 'continue this program'; existing rows are kept and the scorecard is re-seeded. If no, I will ask again next check-in."

Do not upgrade inside this skill.

## Rules

- One row per company and week. Re-runs update and mark `rerun`. Never a second row.
- Nothing is posted or sent without the approval gate; the request in the same message does not count as approval.
- Persona names never leave `personas.private.md`; shared outputs use counts.
- Only the author changes a use case's visibility. The digest uses `team` rows only.
- Use the adapter chosen in `company-context.md`; if its tools are missing this session, say so and use files for this run.
