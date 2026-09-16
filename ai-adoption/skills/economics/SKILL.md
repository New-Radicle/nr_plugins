---
name: economics
description: >
  Size the AI opportunity for the company: map each team to an Economic Index
  job category and one to three occupations (live connector when present,
  cached table otherwise), estimate hours at stake per primitive, apply the
  plugin's speedup and success-rate assumptions, convert to FTE-equivalent
  hours recovered, compare tooling cost with the deferred hire or contractor
  spend, and record every assumption in company-context. Use when someone says
  "run the economics", "what is the ROI", "hours at stake", "size the
  opportunity", "cost comparison", "what does this save us", or after
  map-primitives.
disable-model-invocation: true
argument-hint: "[--report]"
---

# Economics

Pass 2. Produces a defensible range, not a promise. Every figure carries a source: the user, the plugin's assumptions, or the Economic Index release used. Nothing here is a measurement until documented use cases replace it after two weeks of check-ins.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: `company-context.md` (Identity, Economics assumptions, Notes); `primitives` via `read_table`; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/<code>.md` (occupation candidates); `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/ai-modes.md`; `${CLAUDE_PLUGIN_ROOT}/skills/economics/references/econ-index-cached.md`; `${CLAUDE_PLUGIN_ROOT}/skills/economics/references/salary-defaults.md`; `${CLAUDE_PLUGIN_ROOT}/skills/adapters/SKILL.md`.

If `primitives` is empty, stop and say to run `/ai-adoption:map-primitives`.

## 0. Index source

| `econ_index_*` tools in session | Do |
|---|---|
| Yes | Call `econ_index_get_dataset_overview` once and record the release period. Call `econ_index_get_occupation_usage` per candidate occupation. Label figures "live, release <period>". |
| No | Use `references/econ-index-cached.md`. Label every figure from it "cached, release 2026-05". |

Either way: cite the release period in every output, link https://www.anthropic.com/economic-index, and state once, plainly: the current Index release publishes usage shares and augmentation/automation splits; it publishes no task success rates and no speedup multipliers. Those two are the plugin's assumptions from `company-context.md`, editable, and are never attributed to the Index.

### Language the Index requires

| Say | Never say |
|---|---|
| "AI is used for tasks commonly done by [occupation]" | "[occupation]s use AI" |
| "conversations matched to [category] tasks" | "[category] workers are adopting AI" |
| "usage share", "augmentation/automation split" | "exposure", "at risk", "replaceable" |
| "labor market" | "job market" |

Draw no conclusion about job risk, displacement, or reassurance. The data describes conversation patterns, not job outcomes.

## 1. Teams to categories and occupations

For each team in `primitives`: pick the job category from the mapping table in `econ-index-cached.md` (or the live overview), then one to three occupations. Start from `occupation_match` where `map-primitives` filled it; otherwise query or look up the archetype's candidates. Present:

| Team | Job category | Occupations (1-3) | Category share of usage | Augmentation / automation | Source and release |
|---|---|---|---|---|---|

Ask the user to correct any team whose work does not match. Confirm before continuing.

## 2. Hours at stake per primitive

Ask, one team per message: "Roughly how many hours a week does this team spend on each of these?" Accept ranges. Where the user cannot say, estimate and mark it:

| Status | Estimate when not stated |
|---|---|
| Strong, Active, Moderate | people involved x 3-6 h/wk |
| Growing, Early, Ad hoc | people involved x 1-3 h/wk |
| Weak | people involved x 1-3 h/wk, plus the hours it should take |
| Absent | the hours the function would need if staffed; ask, default 2-4 h/wk |
| Excluded | count only prep, capture, and write-up hours, never the physical work |

Every row shows its method, `stated` or `estimated`. Write it to the primitive's `hours_wk` and `hours_method` columns.

## 3. Hours recovered

Use the Economics assumptions table in `company-context.md`. If any cell still holds a `{{placeholder}}`, fill it from the `ai-modes.md` defaults and label "plugin default, editable".

| Quantity | Formula |
|---|---|
| Addressable hours | hours at stake x addressable share (Automation, Analyze: 100%; Augmentation, Edge, Capture: ask, default 50%; Excluded: 0%) |
| Hours recovered, conservative | addressable hours x (1 - 1 / low end of the speedup band) x success rate |
| Hours recovered, base | same, with the midpoint of the band |
| FTE-equivalent | total hours recovered / hours in the company's standard week (ask; default 40) |

Show per team and in total, both cases. Round to whole hours and one decimal of FTE.

## 4. Cost comparison

Ask, one message: seats planned (default: headcount in scope), the role being deferred or contracted and its loaded annual cost if known, and any current contractor spend on that work. Use `salary-defaults.md` only when the user gives no figure, and carry its "verify regionally" flag into every line that uses it.

| Line | Value | Source |
|---|---|---|
| Tooling: seats x monthly price x program months | user's seat count; price shown as a variable with "verify current pricing" | never hard-code a price |
| Modest API or automation use | user estimate; otherwise a small monthly allowance labelled assumption | user or plugin |
| Deferred hire or contractor spend | user figure, else the band from `salary-defaults.md` | user, or "general market range, verify regionally" |
| Hours recovered valued at loaded cost | FTE-equivalent x loaded cost per FTE, conservative and base | computed |
| Net | value of hours recovered minus tooling and API, conservative and base | computed |

## 5. Deskilling and upskilling note

One line per role or team: the skill the person keeps exercising (judgment, review, the physical work), what they stop doing by hand, and the review muscle to protect (what a Skeptic in that role would check first). No line implies a role shrinks.

## 6. Confirm and write

Present the category table, the hours table, the cost comparison, and the notes. On yes:

1. `company-context.md` > Economics assumptions: fill every row with value and source; add rows for addressable share, standard week, seat count, loaded cost of the deferred role, and the Index release used ("live, release <period>" or "cached, release 2026-05").
2. `company-context.md` > Notes and decisions, one dated line: total hours at stake, hours recovered (conservative and base), FTE-equivalent, net (both cases), Index source and release, hours method mix (n stated, n estimated).
3. `update_row('primitives', id, {hours_wk, hours_method})` per primitive.
4. If `$1` is `--report` or the user asks: `<state>/outputs/economics-YYYY-MM-DD.md` with the four tables, the deskilling note, the language rules, the Index citation and link, and `AI-assisted draft. Reviewed by: ________ (name, date)` (the attribution line, blank left empty).

Founder mode: ask hours for the mapped 15 only, in one or two messages; skip the deskilling note unless asked; write the report only if asked.

## 7. Next

Say: "Next: `/ai-adoption:prioritize`." Do not run it unless asked.

## Rules

- Speedup bands and success rates never come from the Index or any external dataset. Say so wherever they appear.
- Every Index figure carries its release period and a live-or-cached label (guardrail 7).
- Tool prices are variables with "verify current pricing" beside them.
- Ranges over point estimates. If the user pushes for one number, give the conservative case.
- Nothing here changes `priority_rank`; that is `prioritize`.
