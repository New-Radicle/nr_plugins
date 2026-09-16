---
name: prioritize
description: >
  Rank the company's primitives with the three-factor score (gap severity, AI
  addressability, economic weight), break ties toward the archetype emphasis,
  write priority_rank and quadrant on every primitives row, and produce the
  top-10 document with one paragraph per primitive. Use when someone says
  "prioritize the primitives", "what should we do first", "rank the
  primitives", "top ten", "top five", "which workflows first", or after
  economics.
disable-model-invocation: true
argument-hint: "[top N, default 10]"
---

# Prioritize

Pass 2. Turns the primitives table into an order of work. The ranking is arithmetic so it can be argued with; the calibration step is where the user's judgment enters, and it is recorded.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: `company-context.md` (Program emphasis, Size mode, Economics assumptions, Notes); `primitives` via `read_table`; the latest `<state>/outputs/economics-*.md` if present; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/ai-modes.md`; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/<code>.md` (emphasis); `${CLAUDE_PLUGIN_ROOT}/skills/adapters/SKILL.md`.

## 0. Preconditions

| Situation | Action |
|---|---|
| `primitives` empty, or rows without `status` and `ai_mode` | Stop; say to run `/ai-adoption:map-primitives`. |
| `hours_wk` blank on every row (economics has not run) | Ask: run `/ai-adoption:economics` now, or rank with estimated hours from the status table in that skill. If estimated, label every economic-weight score "estimated" in the output and the note line. |
| `priority_rank` already filled | Show the current top of the list. Ask: re-rank (ranks are overwritten) or stop. |
| `$1` given | Top N instead of 10. Founder mode default is 5. |

## 1. Score three factors

Score every primitive, Excluded ones included.

| Factor | 5 | 4 | 3 | 2 | 1 | 0 |
|---|---|---|---|---|---|---|
| Gap severity (from `status`) | Absent, Weak | Ad hoc | Early, Growing, Unknown | Moderate, Active | Strong | |
| AI addressability (from `ai_mode`) | Automation | Analyze | Augmentation, Edge | Capture | | Excluded |
| Economic weight | top fifth | second | third | fourth | bottom fifth | no hours |

Economic weight raw = hours per week at stake x loaded hourly cost of the people doing it (loaded cost per FTE from `company-context.md` divided by annual hours, standard week x 48). Rank the raw values among this company's primitives and split into fifths. Sum the three factors for a 0-15 score.

Tiebreak, in order: the primitive's team appears in the archetype emphasis (the "first" team beats the "second"); quadrant order Quick Win, Strategic Bet, Foundation, Future; lower id.

## 2. Quadrant

Recompute `quadrant` for every row with the rule in `ai-modes.md` (speedup class from mode, status from the table). Report any row whose quadrant changed since `map-primitives`.

## 3. Present and calibrate

Show the ranked table: rank, id, primitive, team, mode, status, gap, addressability, economic weight, total, quadrant, owner, expected hours recovered per week (base case from economics, or the estimate, labelled). Then ask in one message: "Anything to move up or down, and why? Any primitive that must wait for a hire, a license, or the data profile?" Apply the moves, keep the list (id, from rank, to rank, reason), and re-show only the changed rows.

## 4. Confirm and write

On yes:

1. `update_row('primitives', id, {priority_rank, quadrant})` for every row. Ranks are 1..N with no gaps; Excluded rows rank last in id order.
2. `company-context.md` > Notes and decisions, one dated line: top N ids and names, calibration moves with reasons, economic weight source (economics run or estimated), Index label carried from economics.

## 5. Top-10 document

Write `<state>/outputs/top-10-YYYY-MM-DD.md` (same filename in Founder mode, so `playbook` and `week` find it). Print it in full.

| Section | Content |
|---|---|
| Title and basis | Company, date, scope. One sentence: ranking method, economics source and Index release label, note that speedup and success rate are plugin assumptions. |
| Ranked table | Rank, id, primitive, team, mode, quadrant, owner, expected hours per week. |
| One paragraph per top-N primitive | What changes (from the current status to the AI-first version, in operating terms); the first workflow (one concrete task with its input, the AI step, and the review step); the owner (name or role from the row; "owner needed" if blank); expected hours per week recovered, base case, labelled by source. |
| Not now | One line listing Future and Excluded primitives, so nobody asks why they were skipped. |
| Attribution | The line from `frameworks/SKILL.md`. Never fill the name. |

Founder mode: top 5, and a closing section titled "Your three workflows for Weeks 1-2": three tasks from the top 5 that the founder does personally this week, each with when in the week it happens, a starting prompt, and what "done" looks like. Keep every paragraph to four sentences.

## 6. Next

| Mode | Say |
|---|---|
| Team, Department | "Next: `/ai-adoption:map-personas` (private to leads) and `/ai-adoption:risk-matrix`, then `/ai-adoption:kickoff` in Week 1." |
| Founder | "Week 0 is done. Next: `/ai-adoption:playbook`, then start workflow one tomorrow. `/ai-adoption:weekly-checkin` runs at the end of Week 1." |

Do not run them unless asked.

## Rules

- Every rank is explainable from the three factors plus a recorded calibration move. No silent reordering.
- Economic weight inherits its source label from `economics`; estimated is estimated all the way to the document.
- Excluded primitives are ranked and listed, never deleted.
- Expected hours are the base case with its label; never present them as savings achieved.
- The document ends with `AI-assisted draft. Reviewed by: ________ (name, date)`, blank left empty.
