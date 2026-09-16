---
name: setup
description: >
  Start an AI adoption program for a company. Opens with a picture of what the
  program is and how it works, then does the work first: public research on the
  company, detection of connected tools, and inference of size mode, archetype,
  and data-handling profile. Shows everything found for confirmation, asks only
  what is still unknown, and creates the state folder. Use when someone says
  "set up the AI adoption program", "start the rollout", "onboard my company",
  "initialize ai-adoption", or when no state folder exists and any other
  /ai-adoption skill is invoked.
disable-model-invocation: true
argument-hint: "[company website or state folder path]"
---

# Setup

People get information before they are asked for any. Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md` throughout: orient, work and show, ask only the gaps. The person's first answer should arrive after they have seen what you already know.

Read first: `show-first.md` (above); `${CLAUDE_PLUGIN_ROOT}/skills/adapters/SKILL.md` (detection and gates); `${CLAUDE_PLUGIN_ROOT}/skills/archetypes/SKILL.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/program.md`; `${CLAUDE_PLUGIN_ROOT}/skills/discover/SKILL.md`.

## 0. Existing state

If `$1` is a path, use it as the state folder. Otherwise look for `./ai-adoption-state/company-context.md`. If it exists, show its company and week and offer: continue, add a second company, or start over. Never overwrite without an explicit yes.

## 1. Orient (no questions yet)

Render the orientation widget (blueprint A in `show-first.md`), or its markdown equivalent:

- One line: "A six- or twelve-week program that makes AI how this company works, run from your own tools, with a person deciding at every step that matters."
- The phase strip: Foundation, First Win, Workflow Layer, Depth, Institutional Layer. Current: Foundation.
- "I will do now": research what is public about the company; check which tools are connected; work out size mode, archetype, and data-handling signals; draft the program calendar.
- "I will ask you later": your role and the Rollout Lead; anything research could not settle; four data-handling questions if the signals are unclear; the start date.
- Buttons: "Start" and "How does this work?" (the second loads `${CLAUDE_PLUGIN_ROOT}/skills/foundations/SKILL.md` and answers from it, then returns here).

If `$1` is a website or the person named one, do not wait for "Start" to begin the work in section 2; start it and show the orientation while it runs.

## 2. Work first, without asking

Run these together where the environment allows, and say in one line that they are running:

| Work | How | Result |
|---|---|---|
| Public research | The `discover` skill's public-source pass via the `researcher` agent, all five areas, from `$1` or the website the person named. If no website is known, search by company name; if there is no name, this is the one thing to ask before starting. | Claims with source and confidence |
| Tool detection | Detection procedure in `adapters/SKILL.md`, names only, no data read | Capability table |
| Inference | From the claims and detection: size mode from headcount band; archetype from what the company does; data-profile signals from grants, contracts, sectors, and regulated data mentions; likely Sponsor and Rollout Lead pattern from size | Each marked "inferred" with its basis |
| Calendar draft | Week 0 on next Monday; end date from the inferred mode | Dates |

Connected-system shape reads (the opt-in part of `discover`) do not run here; they are offered after the data profile is confirmed.

## 3. Show what was found

Render findings-review widgets (blueprint B), six rows each, in this order: identity and size, archetype signals, funding and obligations, data-profile signals, tools and systems, detected connectors. Every row carries a source or the reason for an inference, and a confidence. Confirm, Correct, Reject per row; "Confirm all shown" per widget.

Rules of evidence from `discover`: absence is not evidence; two sources for High; rejections are logged and never re-proposed. A corrected value replaces the claim with the person as source.

After the last widget, show the inferred summary as one card: size mode, archetype (plus any added teams), data profile, connectors that will be used, program dates. Each line marked confirmed or inferred.

## 4. Ask only the gaps

Question cards (blueprint C), three per batch at most, only for what is still unknown or inferred at Low confidence:

- Who is opening the plugin: "Founder, CEO, or executive with budget" / "Director, manager, or lead without budget authority" / "Accelerator, incubator, studio, or fund". This sets the entry point (Sponsor first, Rollout Lead first, cohort operator) exactly as before: Rollout-Lead-first ends with `sponsor-brief` and nothing that costs money runs until a Sponsor confirms; cohort operators are told cohort mode ships later and offered one company now.
- Sponsor and Rollout Lead names, if not the same person.
- Headcount, only if research found no band.
- Archetype, only if inference was Low; show the five one-line options. Then: any team from another archetype to add.
- The four data-handling questions, only where research gave no signal. Where it did, ask the person to confirm the derived profile instead. Cumulative rules: grants or NDAs = Restricted; defence, export-controlled, or controlled-unclassified work = Controlled; personal, health, or financial data at scale = Regulated; none = Open. State the effects (never-in-prompts list now for Restricted; connectors off, compliance owner, IP row High/High for Controlled; allow-list and anonymisation step for Regulated). The Rollout Lead may raise the profile, never lower it without the Sponsor.
- Connector choices, as a confirmation of the detected table with the data-profile gate already applied. If nothing is connected, say files work end to end and connectors can be added later with no migration.
- Start date, defaulting to the drafted one.

Say, once: the plugin cannot create a recurring task; to make the weekly check-in automatic, create a weekly Routine or calendar reminder that runs `/ai-adoption:weekly-checkin`; it is safe to run late or twice. If a calendar adapter is in use, offer to draft the Week 0 alignment and Week 1 kickoff events, behind the approval gate.

## 5. Confirm and write

One summary table, every line marked confirmed. On yes:

1. Create the state folder from `${CLAUDE_PLUGIN_ROOT}/templates/` (`STATE-README.md` becomes `README.md`).
2. Fill `company-context.md`. Write the `## Discovery (confirmed)` section from confirmed claims with sources. Leave the risk matrix and economics assumptions at the archetype and framework defaults, labelled "default, editable".
3. Write `<state>/outputs/discovery-YYYY-MM-DD.md` with every claim and its verdict, the rejected list, and the sources, ending with `AI-assisted draft. Reviewed by: ________ (name, date)` (blank left empty).
4. Write the `companies` row.
5. Seed `scorecard` with the metric list for the mode, targets scaled to headcount; no channel-post metrics in Founder mode.
6. Append a dated line to Notes and decisions: who set up, entry point, profile, counts of confirmed, corrected, rejected claims.

If a connector state store was chosen, create the five tables there per `adapters/references/<backend>.md`, and still write `company-context.md` locally.

## 6. What happens next

Render a status strip (blueprint D) with the next action as the button:

- Rollout Lead first: `/ai-adoption:sponsor-brief`, then `assess-team` once the Sponsor confirms.
- Sponsor first: `/ai-adoption:assess-team`, then `map-primitives`, `economics`, `prioritize`; one session in Founder mode.
- Everyone: `/ai-adoption:week` at any time. Offer the connected-system shape reads from `discover` now that the profile is set, if the profile allows.

Do not run those skills unless asked.

## Rules

- No question before the orientation and the findings review, except the company name when nothing else is known.
- Never post, send, or schedule without the approval gate.
- Never write persona guesses during setup; persona mapping happens in `map-personas`.
- Public research sends the company name and website to search tools; say so once in the orientation.
- No company-specific facts go into any plugin file; everything lives in the state folder.
- If the person is not the Sponsor and asks to skip the Sponsor Brief, do it, record "Sponsor not yet confirmed", and remind at every write that costs money or goes team-wide.
