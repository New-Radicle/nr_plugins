# AI Adoption plugin

Runs a structured AI adoption program inside a company's own tools. Any size from one founder to a multi-department organization. Works with no connectors at all (state lives in a folder of CSV and markdown files) or with Notion, Airtable, Google Drive, Slack, Gmail, and Google Calendar when they are connected.

The plugin ships method and frameworks. It never holds a company's data: everything specific to a company lives in that company's state folder or its own systems.

## Install

Add the New Radicle marketplace once, then install:

```bash
claude plugin marketplace add New-Radicle/nr_plugins
```

```bash
claude plugin install ai-adoption@nr-plugins
```

In the Claude desktop app, add the same marketplace (`New-Radicle/nr_plugins`) in the plugin browser under the Code tab and install from there. If you received this plugin as a `.plugin` file instead, accept it in the desktop app; you will not get updates that way.

For local testing from a checkout:

```bash
claude --plugin-dir ./ai-adoption
```

## Start

```
/ai-adoption:setup
```

Setup asks who you are (Rollout Lead, Sponsor, or cohort operator), which of five company archetypes is closest, your headcount, and four yes/no data-handling questions. It detects connected tools, confirms every choice, and creates `./ai-adoption-state/` with `company-context.md` and five tables. Every other skill reads that file first.

## Skills

| Skill | When | What it does |
|---|---|---|
| `setup` | Week 0 | Archetype, size mode, data profile, capabilities, entry point, state folder |
| `sponsor-brief` | Week 0 | One page for the executive who can approve the program |
| `assess-team` | Week 0 | Scores team health on seven dimensions, sets baselines |
| `map-primitives` | Week 0 | Catalogs operational primitives with status, AI mode, speedup band, quadrant |
| `economics` | Week 0 | Hours at stake, FTE-equivalent, cost comparison, Economic Index mapping |
| `prioritize` | Week 0 | Three-factor ranking, top 10 with first workflows |
| `map-personas` | Week 0-1 | Adoption personas per person, private to the leads |
| `risk-matrix` | Week 0 | Pre-filled risk matrix from archetype defaults and data profile |
| `learn` | Any time | Personal learning path on the five foundations, tied to the person's role and task |
| `week` | Any time | What week it is, what is due, who owns it |
| `kickoff` | Week 1 | Sponsor's kickoff message from the Sponsor's own use cases |
| `workshop` | Week 1-2 | 60-minute hands-on workshop plan with per-role exercises |
| `playbook` | Week 3-4, then as needed | One-page role playbooks, regenerated as use cases accumulate |
| `weekly-checkin` | Every week | Collect, update scorecard, diagnose, act, digest. Safe to run late or twice. |
| `scorecard` | Any time | View, update, or baseline the scorecard |
| `office-hours` | Week 7, then monthly | Agenda built from recent blockers |
| `handbook` | Week 9 (data section earlier when required) | AI handbook draft from the risk matrix and workflows |
| `impact-report` | Week 11, then monthly | Results against targets for the Sponsor to present |
| `plan-next` | Week 12 | Options memo for what comes after the program |

Reference skills loaded automatically when needed: `foundations` (the five layers everyone needs: the model, what you feed it, what it can reach, how it runs, whether you can trust it; plus a glossary and twenty-minute exercises), `frameworks` (personas, failure modes, barriers, defining moments, team rubric, AI modes, scorecard model, program calendar), `archetypes` (five starter libraries plus physical-work primitives), `adapters` (the capability contract and one recipe per backend).

Agent: `adoption-coach`, for resistance, stalled adoption, or "where do I start" questions.

## Modes

| Mode | Headcount | Length | Difference |
|---|---|---|---|
| Founder | 1-9 | 6 weeks | No workshop or channel, one-page playbook, three-metric scorecard, primitives capped at 15 |
| Team | 10-50 | 12 weeks | The full program |
| Department | 50+ | 12 weeks per department, staggered | Runs inside one function first |

## Data handling

Setup derives a profile from four questions. Restricted writes a never-in-prompts list and makes the handbook's data section mandatory before Week 3. Controlled turns cloud connectors off and does not offer them again. Regulated allows only an allow-list of connectors and adds an anonymization step to every workflow that touches the data.

## Guardrails built in

1. Persona assignments are never written to a shared surface.
2. Nothing is posted to a channel or sent to an inbox until the Sponsor or Rollout Lead approves the exact text in chat.
3. Controlled profile disables cloud adapters; the plugin does not offer to re-enable them.
4. Use cases carry a visibility flag only the author changes.
5. Every generated document ends with an "AI-assisted draft. Reviewed by:" line the plugin never fills in.
6. Economic figures cite the Economic Index release used and are labelled cached when the connector is absent. Speedup bands and success rates are the plugin's own assumptions, editable per company.

## Scheduling

The plugin cannot create recurring tasks. Setup tells you how to add a weekly Routine or calendar reminder that runs `/ai-adoption:weekly-checkin`. The check-in is idempotent: one row per week, updated on re-run.

## Layout

```
ai-adoption/
├── .claude-plugin/plugin.json
├── hooks/hooks.json            # SessionStart pointer to the state folder
├── skills/<name>/SKILL.md      # one per skill above
├── skills/frameworks/references/
├── skills/archetypes/references/
├── skills/adapters/references/ # files, notion, airtable, drive, slack, gmail, google-calendar
├── agents/adoption-coach.md
├── templates/                  # copied into the state folder by setup
└── evals/                      # claude plugin eval cases
```

## Roadmap

Cohort mode for accelerators and portfolio operators (dashboard, anonymized archetype benchmarks, office-hours agenda across companies, cohort impact report), Department roll-up, and optional CRM, task-tracker, HRIS, and code-host adapters.
