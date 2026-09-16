---
name: setup
description: >
  Start an AI adoption program for a company. Detects connected tools, asks
  which archetype is closest, infers the size mode from headcount, derives the
  data-handling profile from four yes/no questions, records the Sponsor and
  Rollout Lead, and creates the state folder from templates. Use when someone
  says "set up the AI adoption program", "start the rollout", "onboard my
  company", "initialize ai-adoption", or when no state folder exists and any
  other /ai-adoption skill is invoked.
disable-model-invocation: true
argument-hint: "[state folder path]"
---

# Setup

You are starting a program that will run 6 or 12 weeks. Take 20-30 minutes. Ask in batches of two or three questions; never dump the whole list. Confirm the summary before writing anything.

Read first: `${CLAUDE_PLUGIN_ROOT}/skills/adapters/SKILL.md` (detection and gates), `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/SKILL.md`, `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/program.md`.

## 0. Existing state

If `$1` is given, use it as the state folder. Otherwise look for `./ai-adoption-state/company-context.md`. If it exists, say what company and week it holds and ask whether to continue that program, start a second company (cohort), or start over. Never overwrite an existing folder without an explicit yes.

## 1. Who is opening the plugin (entry point)

Ask: "What's your role here?" Map the answer:

| Opener | Path |
|---|---|
| Director, manager, or lead without budget authority | **Rollout Lead first.** Continue setup, then produce a Sponsor Brief (`/ai-adoption:sponsor-brief`) and a draft company-context for the Sponsor to correct. Nothing that costs money, needs licenses, or messages the whole team runs until a Sponsor is named and has confirmed in chat. |
| Founder, CEO, executive with budget | **Sponsor first.** Skip the brief. Name a Rollout Lead. If there is none, ask for two or three candidates who sit between leadership and operations and are self-directed; recommend one. |
| Accelerator, incubator, studio, fund | **Cohort operator.** Say that cohort mode ships in a later release; offer to set up one portfolio company now in their own state folder. |

Record Sponsor and Rollout Lead names and roles.

## 2. Company basics

Offer discovery first: "I can research what is public about the company and pre-fill the next steps; you confirm each finding before it is used. Run `/ai-adoption:discover` now, or answer a few questions instead?" If they choose discovery, pause setup here and resume at this step with the `## Discovery (confirmed)` section filled in.

Ask what discovery did not answer: company name, what it does in one sentence, headcount (people, not FTE), and whether there are distinct departments.

Infer size mode: 1-9 Founder, 10-50 Team, 50+ Department. State the inference and what it changes (from `program.md`), and confirm. In Department mode, ask which department goes first and record it as the scope.

## 3. Archetype

Ask "Which of these is closest to you?" and show the five one-line archetypes from `archetypes/SKILL.md`. Then ask whether any team from another archetype should be added (for example a Programs team). Record archetype and additions.

## 4. Data-handling profile

Ask four yes/no questions, one message:

1. Do you hold government grants or contracts with data management or reporting terms (e.g. DOE, NSF, NIH, or national equivalents)?
2. Do you do defense, export-controlled, or controlled-unclassified work (DoD, ITAR/EAR, CUI, defense customers)?
3. Do you handle personal data at scale, health data, or financial records?
4. Do you work under customer NDAs or with unpublished research?

Derive, cumulatively: Q1 or Q4 yes = Restricted. Q2 yes = Controlled. Q3 yes = Regulated. All no = Open.

State the profile and its effects:

- Restricted: write a "never in prompts" list from the grant or NDA terms (ask for the top items now); the AI handbook's data section is mandatory before Week 3.
- Controlled: cloud connectors are off and will not be offered; files only; enterprise or API terms with no-training and data-residency assurances are required before any use; name a compliance owner now; risk matrix IP row is High/High.
- Regulated: connector allow-list only (ask which); an anonymization step is added to every workflow that touches the data.

The Rollout Lead may raise the profile but not lower it; lowering needs the Sponsor's yes in chat.

## 5. Capabilities

Run the detection procedure from `adapters/SKILL.md`. Present a table: capability, what was detected, what will be used, fallback. Apply the data-profile gate before offering anything. Ask the user to confirm or change each choice. If nothing is connected, say plainly that files work end to end and connectors can be added later with no migration.

## 6. Program calendar and scheduling

Ask for the Week 0 start date (default: next Monday). Compute the end date from the mode.

Say this about scheduling, verbatim in spirit: the plugin cannot create a recurring task for you. To make the weekly check-in automatic, create a weekly Routine (desktop app: Routines > New routine, local) or a calendar reminder that runs `/ai-adoption:weekly-checkin` on the day you choose. The check-in is safe to run late or twice.

If a calendar adapter is in use, offer to draft the Week 0 Sponsor alignment meeting and the Week 1 kickoff; apply the approval gate.

## 7. Confirm and write

Summarize everything in one table. On yes:

1. Create the state folder from `${CLAUDE_PLUGIN_ROOT}/templates/` (copy every file; `STATE-README.md` becomes `README.md`).
2. Fill `company-context.md` placeholders. Leave the risk matrix and economics assumptions sections with the archetype defaults from `archetypes/references/<code>.md` and `frameworks/references/ai-modes.md`, labelled "default, editable".
3. Write the `companies` row.
4. Seed `scorecard` with the metric list for the mode (`frameworks/references/scorecard-model.md`), baseline blank, targets scaled: use-case targets multiply by headcount; channel-post targets are omitted in Founder mode.
5. Append a dated line to "Notes and decisions" in `company-context.md`: who set up, entry point, profile.

If a connector state store was chosen, create the five tables there using the recipe in `adapters/references/<backend>.md` and still write `company-context.md` locally, because every skill reads it from the folder.

## 8. What happens next

Tell the user, by path:

- Rollout Lead first: "Next: `/ai-adoption:sponsor-brief`. Send it. When the Sponsor confirms, run `/ai-adoption:assess-team`."
- Sponsor first: "Next: `/ai-adoption:assess-team`, then `map-primitives`, `economics`, `prioritize`. In Founder mode these run in one session."
- Everyone: "Run `/ai-adoption:week` any time to see what is due."

Do not run those skills now unless asked.

## Rules

- Never post, send, or schedule without the approval gate.
- Never write persona guesses during setup, even if the user volunteers them; note "persona mapping happens in `map-personas`".
- No company-specific facts go into any plugin file; everything lives in the state folder.
- If the user is not the Sponsor and asks to skip the Sponsor Brief, do it, but record "Sponsor not yet confirmed" in `company-context.md` and remind at every write that costs money or goes team-wide.
