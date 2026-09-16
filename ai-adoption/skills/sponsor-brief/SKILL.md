---
name: sponsor-brief
description: >
  Write the one-page Sponsor Brief a Rollout Lead forwards to the CEO or
  executive who can approve the AI adoption program: the team score if known,
  the top five primitives, the cost comparison, and a two-line ask. Use after
  setup on the Rollout-Lead-first path, or when someone says "write the brief
  for my CEO", "make the case to leadership", "sponsor brief", or "one-pager
  for the founders".
disable-model-invocation: true
---

# Sponsor Brief

One page. Written in the company's own terms, for someone who has five minutes and budget authority. Designed to be forwarded unchanged.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: the state folder's `company-context.md`; `primitives.csv` and `scorecard.csv` if populated; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/<archetype>.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/ai-modes.md`.

## Inputs

If `assess-team`, `map-primitives`, and `economics` have run, use their outputs. If not, do not run them. Build the brief from the archetype's starter library and the company basics, and label estimates as "archetype defaults; the assessment will replace these". Ask at most three questions: the top two bottlenecks the Rollout Lead sees, the one hire being deferred or considered, and the loaded annual cost of that hire if known.

Use public information about the company only to phrase the brief in its language (product, market, stage). Do not add claims the Rollout Lead did not confirm.

## Structure (fit on one page)

1. **Headline.** One sentence: what changes for this company in 12 weeks (6 in Founder mode), in operating terms, not AI terms.
2. **Where we stand.** Team score and band if available; otherwise two sentences on the archetype's typical gaps and which ones the Rollout Lead confirmed.
3. **Top five primitives.** Table: primitive, team, AI mode, why it is first. Draw from the archetype emphasis and the confirmed bottlenecks.
4. **The economics.** Three lines: tooling cost for the program (seats and modest API use; ask for the seat count if unknown and say "verify current pricing"), the deferred hire or hours at stake, and the net. Label plugin speedup bands as assumptions.
5. **What it takes from you.** Sponsor time from `program.md`: visible use in Weeks 0-2, the kickoff message, the impact report. Two to three lines.
6. **Risks we are handling.** The top two from the archetype defaults plus the data profile, each with its mitigation in one line.
7. **The ask.** Two lines: approve the program and the seats; confirm the Rollout Lead as owner. Give a reply-by date.

End with `AI-assisted draft. Reviewed by: ________ (name, date)` (the attribution line, blank left empty).

## Output

Write to `<state>/outputs/sponsor-brief-YYYY-MM-DD.md`. Print it in full. If an inbox adapter is in use, offer to draft the forwarding email; apply the approval gate. Append a dated line to "Notes and decisions" in `company-context.md`.

## When the Sponsor replies

Record the outcome in `company-context.md` (Sponsor confirmed: yes/no/with changes) and update the People table. Only then do budget-dependent steps unlock.
