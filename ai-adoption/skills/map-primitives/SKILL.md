---
name: map-primitives
description: >
  Build the company's operational primitives table: walk each team, keep or
  rename or drop or add the archetype's starter primitives, set status from
  what the user says, assign one AI mode (Automation, Augmentation, or the
  four physical modes Edge, Capture, Analyze, Excluded), a speedup band, a
  success-rate default, a quadrant, and a stable id. Use when someone says
  "map our primitives", "what does each team actually do", "build the
  primitives list", "inventory the work", "classify our workflows", or after
  assess-team.
disable-model-invocation: true
argument-hint: "[team name to add or redo]"
---

# Map primitives

Pass 1. Map the function, not the person. A primitive is a distinct, repeatable, observable, assignable unit of work. The starter list is a prompt for the conversation; the real list comes from what people actually do, including the work nobody owns.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: the `## Discovery (confirmed)` section of `company-context.md` if present (skip every intake question it already answers, and cite it as the signal); `company-context.md`; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/SKILL.md` and `references/<code>.md` for the archetype plus any added teams; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/physical-primitives.md` when the company has field, lab, plant, or installation work (archetypes A, B, C, or anything the user says in step 1); `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/ai-modes.md`; `${CLAUDE_PLUGIN_ROOT}/skills/adapters/SKILL.md` and `references/files.md` (keys). Run the adapter detection procedure; note whether `econ_index_get_occupation_usage` is in the session.

## 0. Existing state

| Situation | Action |
|---|---|
| No state folder | Stop; say to run `/ai-adoption:setup`. |
| `primitives` is empty | Full walk, all teams. |
| `primitives` has rows and no `$1` | Show the count per team. Ask: add a team, redo one team, or stop. |
| `$1` given | Walk only that team. Existing ids in that team are kept; new primitives take the next `n`. |

## 1. Teams

Present the archetype's teams plus any "Added teams from other archetypes" in one table and ask, in one message: which exist, which are missing, which does one person cover several of, and is any team missing entirely. A function with no owner is still a team (the starter library calls it "Ops (no owner)"); keep it, because it is where AI fills a gap until a hire is justified.

Assign team numbers in the order the user confirms them, starting at 1. Numbers are stable: a team already present in `primitives` keeps its number. Record the team list with numbers and the person or role covering each.

## 2. Primitives per team

One team per message. Show the team's starter primitives with defaults filled and ask for corrections in bulk, not one by one:

| # | Primitive | Keep / rename / drop | Status | AI mode | Owner (name or role) |
|---|---|---|---|---|---|

Then ask: "What else does this team do regularly that is not on the list?" Add those rows.

| Field | How it is set |
|---|---|
| `status` | From the user, using the vocabulary in `ai-modes.md`. Never from the default. Offer `Unknown` rather than guessing. |
| `ai_mode` | Archetype default unless the user describes the work as physical; then one of Edge, Capture, Analyze, Excluded. Digital work gets Automation or Augmentation. Exactly one mode per primitive. |
| `speedup` | The band string from `ai-modes.md` for the mode (for example `5-10x`, `2-4x`, `3-6x`, `1x`). Plugin assumption, editable in `company-context.md`. |
| `success_rate` | `65%` for Automation, Augmentation, Edge, Analyze; `55%` for Capture; blank for Excluded. |
| `quadrant` | Quick Win, Strategic Bet, Foundation, or Future by the rule in `ai-modes.md`. `prioritize` recomputes it. |
| `owner` | Name or role. Blank is a finding, not an error. |
| `priority_rank` | Blank. `prioritize` fills it. |
| `notes` | What the user said that matters: tooling in use, why it is Excluded, who really does it. |

Keep Excluded primitives. They make the hours estimate honest and stop anyone claiming AI savings on bench, field, or plant time.

## 3. Ids

`id` = `<team-number>.<n>`, `n` from 1 in the order primitives are confirmed within the team. Dropped starter primitives are not written and do not consume an `n`. Ids never change once written; a renamed primitive keeps its id.

## 4. Occupation match

| `econ_index_get_occupation_usage` in session | Do |
|---|---|
| Yes | Per team, query the archetype's occupation candidates from `references/<code>.md`. Record in `occupation_match` the published occupation name the query resolves to, or leave blank when nothing resolves. |
| No | Leave `occupation_match` blank. Say that `economics` will use the cached table. |

The match is a label for `economics`. Phrase it as "AI is used for tasks commonly done by [occupation]". Say nothing about job risk or displacement.

## 5. Founder mode

Cap at 15 primitives. Combine steps 1 and 2 into one short message per team (in Founder mode teams are the founder's hats). If the walk produces more than 15, show the full list with the archetype emphasis marked and ask the founder to pick 15. The rest go in the Notes and decisions line as "parked" and can be added later with `$1`.

## 6. Confirm and write

Present the full table (id, team, name, status, ai_mode, speedup, success_rate, quadrant, owner, occupation_match). On yes:

1. `write_row('primitives', row)` per new primitive; `update_row` for existing ids in a redone team. `company_id` from `companies`.
2. `company-context.md` > Notes and decisions, one dated line: teams mapped with numbers, primitives per team, count by mode, functions with no owner, parked list if any.

## 7. Next

Say: "Next: `/ai-adoption:economics`, then `/ai-adoption:prioritize`." In Founder mode continue in the same session. Do not run them unless asked.

## Rules

- Status comes from the user. If they do not know, write `Unknown`.
- Physical work is never Automation or Augmentation. Ask "is this done at a bench, in the field, or on a plant floor?" whenever unsure.
- Speedup and success rate are plugin assumptions; say so once, then stop repeating it.
- No person's name goes anywhere except the `owner` field.
- Do not rank here. Ranking needs the economics pass.
