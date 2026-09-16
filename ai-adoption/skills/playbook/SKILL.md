---
name: playbook
description: >
  Generate the one-page AI playbook for a role or team: its top three AI-first
  workflows drawn from the top-10 primitives and documented use cases, each
  with when to use it, a starter prompt to paste, what to check before
  trusting the output, and the data rule from the company's data-handling
  profile. Regenerates as use cases accumulate and says what changed. Use in
  Weeks 3-4 (Team mode) or Week 1 (Founder mode), or when someone says "write
  the playbook for engineering", "role playbook", "what should my team use AI
  for", "refresh the playbooks", or "one-pager for the ops team".
disable-model-invocation: true
argument-hint: "[role or team]"
---

# Playbook

One page per role or team. Three workflows, not ten. Written so a person who has never opened the tool can paste the first prompt and get something useful in five minutes. Craft-respecting: AI removes the administrative tax; the expert stays the judge.

Read first: the state folder's `company-context.md` (mode, data profile, never-in-prompts list, capabilities); `primitives.csv`; `use_cases.csv`; existing `<state>/outputs/playbook-*.md`; `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/<archetype>.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/ai-modes.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/barriers.md`.

If `primitives.csv` has no `priority_rank` values, stop and point to `/ai-adoption:prioritize`. A playbook built from an unranked list is a guess.

## 0. Scope

| Input | Behaviour |
|---|---|
| `$1` names a team in `primitives.csv` | One playbook for that team |
| `$1` names a role not in the primitives table | Ask which team it belongs to; if the user says "all", loop over every team |
| No argument, Team or Department mode | Ask which team, or offer all teams in the department scope, one page each |
| No argument, Founder mode | One page for the founder; skip the question |

## 1. Choose the three workflows

Rank candidates for the team, take the top three, and say why each made the cut in one line.

| Source | Weight | Rule |
|---|---|---|
| `primitives.csv` rows for the team with `priority_rank` 1-10 | Primary | Lower rank first. Quick Win quadrant beats Strategic Bet at equal rank. |
| `use_cases.csv` rows for the team | Evidence | A primitive with documented use cases moves up one place; three or more moves up two. Use case text appears in the playbook only when `visibility` is `team`. `leads` and `private` rows count as evidence and are never quoted, attributed, or described. |
| Archetype starter library | Fallback | Only when the team has fewer than three ranked primitives. Label it "starter default; replace after the first check-in". |
| `ai_mode` Excluded | Never | Physical work with no AI role is not a workflow. If it is in the top three by rank, skip it and say so. |

Never choose two workflows for the same primitive.

## 2. What each workflow contains

| Field | What to write | Length |
|---|---|---|
| Name | The primitive name in the team's own words, plus the AI mode in brackets | One line |
| When to use it | The trigger in the working week (a document is requested, a test run finishes, a report is due) | One or two lines |
| Starter prompt | A complete prompt the person pastes, with placeholders in square brackets for the inputs they supply. Ask for the output shape they already use. Include a line asking the tool to list what it was unsure about. | 4-8 lines |
| Before you trust it | Two or three concrete checks tied to the mode: Automation gets "verify every figure, name, and citation against the source"; Augmentation gets "read for your voice and for anything it invented"; Capture gets "compare against the raw notes or photo"; Analyze gets "check the trend against one data point you know" | Bullets |
| Data rule | From section 3 | One line, repeated on every workflow |
| Where it goes | The document, log, or system the output lands in, and who reviews it | One line |

Draw starter prompts from `team`-visibility `prompt_summary` values when they exist; those are proven in this company. Otherwise write one from the primitive and the archetype library and mark it "untested".

## 3. Data rule

Read the data-handling profile and write one line that appears at the top of the page and under every workflow.

| Profile | Rule to print |
|---|---|
| Open | "Do not paste customer names, unreleased financials, or anything under NDA." plus the never-in-prompts list if one exists |
| Restricted | "Never in prompts:" followed by the list from `company-context.md`, verbatim. Add "When in doubt, ask <compliance owner or Rollout Lead>." |
| Controlled | The Restricted line plus "Approved tools only: <from company-context>. No cloud connectors." |
| Regulated | The applicable line above plus "Anonymize before pasting: remove names, identifiers, account numbers, health or financial details." |

If the profile is Restricted or above and the never-in-prompts list is empty, stop and ask for it before writing the page. Do not invent one.

## 4. Page layout

Fit on one printed page. Order:

1. Title: "<Team> AI playbook", company name, date, "version N" (N = count of prior playbooks for this team plus one).
2. One sentence on what this team gets back: hours per week from the primitive estimates, labelled "estimate".
3. Data rule box.
4. The three workflows, each as the table in section 2.
5. "Getting help": the team AI lead if one is named in `company-context.md`, the channel if one exists, otherwise the Rollout Lead.
6. "Log what worked": one line telling people where use cases are collected (the weekly check-in), and that they choose the visibility of anything they share.
7. `AI-assisted draft. Reviewed by: ________ (name, date)` (the attribution line, blank left empty).

Plain language. No persona names, no persona labels, nothing from `personas.private.md`.

## 5. Regeneration

If a playbook for the team already exists in `<state>/outputs/`, read the latest one before writing.

| Change | Report as |
|---|---|
| Workflow replaced | "Replaced <old> with <new>: <reason from section 1>" |
| Starter prompt swapped for a proven one | "Prompt for <workflow> now from N documented use cases" |
| Data rule changed | "Data rule updated" with the diff |
| Nothing material changed | Say so; still write the new dated file only if the user wants it |

Print the change list before the page. Never delete the earlier file.

## 6. Founder mode

One page, three workflows, the founder as the only reader. Choose from the top-ranked primitives across all teams, not per team; prefer the archetype emphasis (for example grants first, documentation second). Skip "Getting help". Replace "Log what worked" with "Add a line to `wins.md` when one of these saves you an hour."

In Weeks 5-6 the playbook is revised from `wins.md`: any workflow with no win logged is replaced by the next primitive down.

## 7. Output

Write `<state>/outputs/playbook-<team-slug>-YYYY-MM-DD.md`. Print it in full. If a channel adapter is in use and the user wants to share it, draft a two-line post with the file attached or linked; apply the approval gate. Append a dated line to "Notes and decisions" in `company-context.md`: which playbooks were generated and their version.

Suggest next: distribute in Week 4; `weekly-checkin` collects the use cases that feed the next version.

## Rules

- Three workflows. If the user asks for more, offer a second page for a second role instead.
- Only `team`-visibility use cases are quoted. Only the author changes a visibility flag; the playbook never does.
- No persona names or labels on the page, ever.
- Never fill the reviewed-by blank.
- Speedup figures on the page are the plugin's assumptions until the company's own check-in data replaces them; say which.
