---
name: risk-matrix
description: >
  Fill the "Risk matrix" section of company-context.md from the archetype's
  default risks, the data-handling profile rules, and the seven failure modes
  assessed for this company, then take edits to severity, likelihood, owner,
  and mitigation and write the section back. Adds the shared error log as a
  standing mitigation. Use in Week 0 after setup, or when someone says "fill
  in the risk matrix", "what are our AI risks", "review the risks", "add a
  risk", or "who owns the IP risk".
disable-model-invocation: true
---

# Risk matrix

Pre-filled so the leads edit rather than invent. Twelve to twenty rows is normal; more than that and nobody reads it.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: `<state>/company-context.md` (archetype, data profile, never-in-prompts list, compliance owner, headcount, capabilities, Sponsor status); `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/references/<archetype>.md` (Default risks table); `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/failure-modes.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/defining-moments.md`; `<state>/primitives.csv` if it exists.

| Mode | What changes |
|---|---|
| Founder | Not in the reduced set. If asked, write the archetype defaults plus the data-profile rows and skip the failure-mode assessment. |
| Team | Full run. |
| Department | Rows scoped to the department, plus one row for cross-department dependencies (logged, not solved). Data-profile rows are company-level and are never lowered for a department. |

## 1. Build the pre-fill

Columns: Risk, Severity (High / Medium / Low), Likelihood (High / Medium / Low), Owner, Mitigation, Source.

### Archetype defaults

Copy every row from the archetype file's Default risks table. Source = "archetype default". Where the archetype gives no mitigation, take it from the matching failure mode or barrier.

### Data-profile rules

Apply cumulatively, in this order.

| Profile | Rule |
|---|---|
| Open | One row: "Company IP or confidential material in prompts", Medium / Medium; mitigation: the tool's terms confirmed, handbook data section |
| Restricted | Add "An item on the never-in-prompts list appears in a prompt", High / Medium; mitigation: handbook data section before Week 3, prepared sample inputs for the workshop |
| Controlled | Set the IP row to High / High. Add "Cloud tool or connector used outside the approved terms", High / Medium, owner compliance owner; mitigation: files-only state, no-training and data-residency terms confirmed before any use |
| Regulated | Add "Personal, health, or financial data exposed through a prompt or connector", High / Medium; mitigation: anonymization step in every workflow that touches the data, connector allow-list only |

If the profile includes Controlled or Regulated and no compliance owner is named, stop and ask for one before continuing.

### Seven failure modes

Assess each of the seven for this company. Likelihood comes from the "High risk when" column matched against company-context facts: headcount, archetype, capabilities, whether the Sponsor is confirmed, whether primitives include safety-critical or regulated work. Severity is High for the modes that stop the program (Crowdsource Trap, Skills Gap Abyss, Measurement Void) and Medium for the rest unless the facts raise it. Mitigation is the Fix column made specific: name the three workflows, the workshop week, the check-in day. Source = "failure mode N".

Default owners: Sponsor for Crowdsource Trap, Culture Collision, and any autonomy-threat row; compliance owner for data rows; Rollout Lead for everything else. Owners are roles from the People table, never names in the Risk column.

## 2. Present for edits

Show the full table. Ask for changes in two passes, one message each: severity and likelihood first, then owner and mitigation. Accept added rows. Drop nothing the data profile requires; if the user wants a data-profile row lowered, say it needs the Sponsor's yes in chat and record who said yes.

Sort the final table by severity, then likelihood.

## 3. Standing mitigation: the shared error log

Add one row: "AI errors go unreported, or are used as proof the program failed", Medium / High, owner Rollout Lead, mitigation: the shared error log and the first-error script from `defining-moments.md`.

Create `<state>/errors.md` if it does not exist, with this header and nothing else:

```
# AI error log (shared)

An error found here means the review worked. Log it, thank the finder, fix the workflow.

| Date | Found by | Primitive | What was wrong | How it was caught | What changed |
|---|---|---|---|---|---|
```

Tell the user the log is shared with the whole team, read at every office hours, and referenced in the handbook. It is the one place where names and errors sit together on purpose.

## 4. Write back

Replace the "Risk matrix" section of `<state>/company-context.md` with the table, preceded by one line: "Reviewed on YYYY-MM-DD by the Rollout Lead; archetype defaults and data-profile rows are minimums." Keep every other heading untouched. Append a dated line to "Notes and decisions": rows added, rows edited, any data-profile row changed and who approved it.

Print the top three risks with their mitigations; `sponsor-brief`, `workshop`, and `handbook` read these.

## Rules

- Never lower a data-profile row without the Sponsor's yes in this chat.
- Never write the matrix anywhere but `company-context.md`; the handbook copies from it later.
- If `primitives.csv` lists safety-relevant work (test reports, work instructions, regulatory filings, field safety), raise the over-trust row to High severity and say why.
- Re-run at Week 8 with the mid-point survey; rows whose likelihood has changed get a dated note in the Mitigation cell.
