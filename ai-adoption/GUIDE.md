# The AI adoption program, start to finish

A walk through the whole program as the plugin runs it. Read it once before Week 0; come back to the week you are in. Every step names who does it, what you will be asked, what it produces, and where the plugin stops to let a person decide.

## Who is who

| Role | What they own | Time |
|---|---|---|
| Sponsor | Budget, licences, the kickoff message, presenting the impact report, visible use of the tools in the first two weeks | 1-2 hours a week in Weeks 0-2, then 30 minutes |
| Rollout Lead | Everything else: the weekly cadence, workshop, playbooks, check-ins, scorecard, coaching, escalation. The plugin's default user. | 3-5 hours a week during the program, 2-3 after |
| Team AI leads (optional) | One per function; first point of help; collects use cases | 1 hour a week |

In Founder mode (1-9 people) the Sponsor and Rollout Lead are the same person and the program is six weeks. Team mode (10-50) is the full twelve weeks. Department mode (50+) runs the twelve weeks inside one department at a time.

## Before you start

1. Install the plugin (README, Install section) and open Claude in the folder where the program's files should live.
2. Decide who the Sponsor and Rollout Lead are. If you are a director without budget authority, the plugin assumes you are the Rollout Lead and will write a one-page brief for your Sponsor before anything that costs money.
3. Know your answers to four data-handling questions: government grants or contracts with data terms; defence, export-controlled, or controlled-unclassified work; personal, health, or financial data at scale; customer NDAs or unpublished research.

Ask `/ai-adoption:learn` at any point if a concept is unfamiliar. It gives a short personal path on the five basics: the model, what you feed it, what it can reach, how it runs, whether you can trust it.

## Week 0: foundation

Six skills, run in this order. In Founder mode they run in one session of about an hour. In Team mode allow three or four sessions across the week.

### 1. `/ai-adoption:setup` (20-30 minutes)

Opens with a one-screen picture of the program and what it will do now versus ask later. Then it works first: public research on the company, detection of connected tools, and inference of size mode, archetype, and data-handling signals. You confirm, correct, or reject each finding before it is used. Only then does it ask what is left: the company basics, which of five archetypes is closest (grant-stage hardware, scaling hardware, project developer, software, nonprofit or services), the four data-handling questions, and which connected tools to use. Confirms every inference before writing.

Produces the state folder `./ai-adoption-state/` with `company-context.md` and five tables. Tells you how to schedule the weekly check-in, because the plugin cannot create a recurring task itself.

Stops for a decision: the data profile. Controlled turns cloud connectors off for good. Restricted needs a never-in-prompts list now.

### 2. `/ai-adoption:discover` (optional, 20 minutes)

A research agent gathers what is public about the company and, with your yes per system, the shape of connected tools (names of spaces, channels, recurring meetings; never content). Findings come back six at a time with a source and a confidence. You confirm, correct, or reject each. Only confirmed claims are written, and the later skills skip every question they answer.

### 3. `/ai-adoption:sponsor-brief` (Rollout-Lead-first path only, 10 minutes)

One page for the executive who can approve the program: where you stand, the top five primitives, the economics, what it takes from them, the two-line ask. You send it. When the Sponsor confirms in chat, budget-dependent steps unlock.

### 4. `/ai-adoption:assess-team` (15-20 minutes)

Scores team health 0-5 on seven dimensions from a short intake. Presents the scores for calibration; you correct them. Writes the baseline rows of the scorecard and sets targets for the two dimensions AI moves fastest: communication architecture and enabling structure.

### 5. `/ai-adoption:map-primitives` (15-20 minutes)

Walks your teams and the starter list for your archetype. Keep, rename, drop, add. For each primitive you set the current status; the plugin proposes the AI mode (automation, augmentation, or the four physical modes: edge, capture, analyse, excluded), a speedup band, and a quadrant. Founder mode caps the list at fifteen.

### 6. `/ai-adoption:economics` then `/ai-adoption:prioritize` (15 minutes together)

Economics puts hours at stake against each primitive, applies the plugin's speedup and success-rate assumptions (yours to edit), and compares tooling cost with the hire or contractor spend it defers. Live Economic Index figures when the connector is present, a labelled cached table otherwise. Prioritize ranks everything on gap severity, AI addressability, and economic weight, and writes the top ten with a first workflow and an owner for each. Founder mode: top five and your three workflows for Weeks 1-2.

### Also in Week 0 (Team mode)

- `/ai-adoption:map-personas`: the Rollout Lead's 1:1 notes become a persona per person, written only to a private file the team never sees. Shared outputs use counts.
- `/ai-adoption:risk-matrix`: pre-filled from the archetype and the data profile; you edit severity, likelihood, owner, mitigation. Creates the shared error log.
- `/ai-adoption:scorecard baseline` after the baseline survey.

## Weeks 1-2: first win

Goal: every person has one positive AI experience within ten days.

- `/ai-adoption:kickoff` drafts the Sponsor's message from the Sponsor's own real use cases. It refuses to invent them. The Sponsor sends it; the plugin never posts on its own.
- `/ai-adoption:workshop` produces a 60-minute hands-on plan: ten minutes of the Sponsor demonstrating, thirty-five of per-role exercises on real material, ten of posting first wins, five on the data rules.
- `/ai-adoption:weekly-checkin` from Week 1, every week, on the day you scheduled. See "the weekly loop".

Founder mode: no kickoff or workshop. You do your three workflows daily and log wins in `wins.md`.

## Weeks 3-4: workflow layer

- `/ai-adoption:playbook` writes a one-page playbook per role: three workflows, each with a starter prompt, what to check before trusting the output, and the data rule. It regenerates as use cases accumulate and tells you what changed.
- Connect the first tools if you have not. Setup can be re-run for capabilities only.
- If the data profile is Restricted, run `/ai-adoption:handbook data-section` before Week 3 ends.

## Weeks 5-8: depth

- Advanced tools for technical roles; the first automations.
- `/ai-adoption:office-hours` in Week 7 builds the agenda from the last six weeks of blockers, with a demo per theme and a segment for the sceptics' questions framed as the quality gate.
- Week 8 mid-point survey through the check-in.
- Expect the first error somewhere in here. The right response is scripted in the frameworks: "Good catch. This is why we have review. The system worked."

## Weeks 9-12: institutional layer

- Week 9 `/ai-adoption:handbook`: when to use AI and when not, the data rules, review standards by document type, the attribution line as policy, the error log, who to ask.
- Week 10: the use-case library fills from check-ins; the playbooks regenerate.
- Week 11 `/ai-adoption:impact-report`: results against targets, hours and FTE-equivalent labelled as assumptions where they are, the five best use cases, what did not work, and the defining moments as they actually happened. The Sponsor presents it.
- Week 12 `/ai-adoption:plan-next`: three options (sustain, expand, deepen) with cost, owner, and what the scorecard must show by month six. One recommendation. Sets the post-program cadence.

Founder mode ends at Week 6 with the impact report as a one-page investor or board note.

## The weekly loop

`/ai-adoption:weekly-checkin` runs the same five moves every week and is safe to run late or twice:

1. Collect: survey replies or a few questions for the phase; new use cases; wins.
2. Update: scorecard actuals and statuses.
3. Diagnose: which failure mode is active, if any. Quiet channel with high usage means hidden use. Quality pushback means culture collision. No use cases by Week 4 means a measurement void.
4. Act: one intervention tied to the week, with an owner.
5. Digest: a draft for the team channel with wins, one tip, and next week's ask. It is shown to you and stops. It posts only after you say yes.

`/ai-adoption:week` at any time tells you the week, what is due, who owns it, and the one skill to run next. `/ai-adoption:scorecard` shows or updates the numbers.

## Where the plugin stops and a person decides

| Moment | Who decides |
|---|---|
| Data profile and connector choices at setup | Rollout Lead; lowering a profile needs the Sponsor |
| Every discovery claim | Rollout Lead or Sponsor, per claim |
| Team scores, primitive statuses, priorities | Rollout Lead, in calibration |
| Anything posted to a channel or sent by email | Sponsor or Rollout Lead, exact text, every time |
| Any calendar event with attendees | Same |
| The reviewer's name on every generated document | A person fills it in; the plugin never does |

## What lives where

Everything the program creates stays in the company's state folder or its own connected systems. Persona notes live in one private file that never syncs. Nothing is sent to the plugin's authors. The whole state exports to CSV at any time.

## After the program

Monthly: the check-in, the office hours, a refreshed impact report. Quarterly: revisit the plan-next options. When headcount passes ten, the check-in prompts a Founder-to-Team upgrade. When a second department is ready, plan-next drafts its scope.

AI-assisted draft. Reviewed by: ________ (name, date)
