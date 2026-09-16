---
name: handbook
description: >
  Draft the company's AI handbook in Week 9 from the risk matrix, the
  data-handling profile, and the AI-first workflows in primitives and use
  cases: when to use AI and when not, the data rules, review standards by
  document type, the attribution line as policy, the error log, and who to
  ask. Can run early in data-section-only mode, which is mandatory before
  Week 3 for a Restricted profile. Use when someone says "write the AI
  handbook", "AI policy", "acceptable use", "what can we put in prompts",
  "data rules for AI", or "handbook data section".
disable-model-invocation: true
argument-hint: "[data-section]"
---

# Handbook

Institutional layer. The handbook writes down what the team has been doing since Week 1 so it survives the people who started it. It is short, specific to this company, and built from the state folder, not from a template of generic policy.

Read first: `<state>/company-context.md` (data profile, never-in-prompts list, connectors allowed, compliance owner, capabilities, the Risk matrix section); `<state>/primitives.csv` (status, ai_mode, priority_rank); `<state>/use_cases.csv` (rows with `visibility` team or shared); `<state>/errors.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/SKILL.md` (principles and attribution line); `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/defining-moments.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/ai-modes.md`.

| Mode | What changes |
|---|---|
| Founder | Skipped. The one-page playbook from `playbook` holds the data rule and the review line; nothing else is needed at that size. |
| Team | Full run at Week 9; data-section mode earlier when the profile requires it. |
| Department | Scoped to the department's workflows and document types. Data rules are company-level; mark them as such and name the company-level owner. |

## Two modes

| Argument | Runs when | Produces |
|---|---|---|
| none | Week 9 | The full handbook, sections 1-8 |
| `data-section` | Before Week 3 for Restricted; any time on request for Controlled or Regulated | Sections 3, 6, and 7 only, as `<state>/outputs/ai-handbook-data-section-YYYY-MM-DD.md`; the full run later folds it in unchanged unless the compliance owner edits it |

If the profile is Restricted, the current week is 2 or less, and no data-section file exists in `<state>/outputs/`, say so and offer to run in data-section mode now.

## Sections

| # | Section | Source | Content |
|---|---|---|---|
| 1 | Why we use AI | Principles 1, 3 | Three lines in operating terms. AI removes administrative load; expertise stays with the experts; leaders use it first. |
| 2 | When to use AI, when not | `primitives` by ai_mode | Table: task type, mode, what AI does, what the person does. Excluded primitives listed as "no AI role" so the boundary is explicit. |
| 3 | Data rules | Data-handling profile | See below |
| 4 | Review standards by document type | `primitives`, risk matrix | See below |
| 5 | Attribution | `frameworks/SKILL.md` | Policy: every AI-assisted document carries the attribution line, a named person fills it, and no tool may fill it |
| 6 | The error log | `errors.md`, `defining-moments.md` | Where it is, how to log an entry, the response script for a found error, and the rule that a logged error is the review working |
| 7 | Who to ask | People table | Rollout Lead for how-to, team AI leads per function, compliance owner for data, Sponsor for role and budget questions |
| 8 | Approved tools | Capabilities, Controlled terms | The tools in use, the terms they run under, and how a new tool gets added (through the Rollout Lead, checked against the data profile) |

### Data rules (section 3)

| Profile | Must include |
|---|---|
| Open | Standard IP guidance: nothing you would not email to an outside vendor |
| Restricted | The never-in-prompts list verbatim from `company-context.md`; how to build a sample or redacted input; who confirms a grey case |
| Controlled | Tool restrictions: only the named tools under confirmed no-training and data-residency terms; cloud connectors off; the compliance owner's sign-off before any new tool |
| Regulated | The anonymization step, written as a procedure, applied to every workflow in section 2 that touches personal, health, or financial data; the connector allow-list |

Profiles are cumulative; include every block that applies.

### Review standards (section 4)

Derive document types from `primitives` names and group them:

| Document type | Reviewer | Check | Sign-off |
|---|---|---|---|
| External submissions (proposals, filings, funder and lender reports) | Owner plus one second reader | Every citation, number, and claim against source | Named second reader |
| Safety-relevant technical content (specs, test reports, work instructions, field safety) | Domain expert | Full read; AI never the last step | Named expert |
| Customer and partner communications | Owner | Facts and tone; nothing promised the company has not agreed | Owner |
| Internal documents, notes, summaries | Owner | Meaning preserved; flagged additions read | Owner |
| Field and lab capture (logs, incident reports) | The person who observed | Inferred fields confirmed or removed | Observer |

Keep only rows that match this company's primitives and add rows for document types the risk matrix names.

## Draft and present

Draft in the company's own words, two to four pages. Use `use_cases` rows to illustrate section 2 with real workflows: task, mode, what the person checks. Quote nothing without `visibility` team or shared, and no author names in the handbook.

Present section by section and take edits. The compliance owner must see section 3 before it is called final; say so, and record in "Notes and decisions" whether they have.

## Output

Write `<state>/outputs/ai-handbook-YYYY-MM-DD.md` (or the data-section file name above). End with `AI-assisted draft. Reviewed by: ________ (name, date)`; the skill never fills the reviewer. Print the full draft.

Offer, under the approval gate, `send_report` to the Sponsor and compliance owner. Do not post the handbook to the channel from this skill; the Rollout Lead announces it after review. Append a dated line to "Notes and decisions".

## Rules

- No persona data, counts included, in the handbook.
- Never soften a data rule below what the profile requires; edits that lower it need the Sponsor's yes in chat and are recorded.
- The handbook says what people do here, not what AI is. Cut any paragraph that would be true at another company.
- Re-run at month 6 post-program; the diff is the change log.
