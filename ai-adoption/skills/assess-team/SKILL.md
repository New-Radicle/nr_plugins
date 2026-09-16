---
name: assess-team
description: >
  Score a team's health on the seven-primitive rubric (Compelling Direction,
  Shared Mental Models, Mutual Trust, Psychological Safety, Adaptive Capacity,
  Communication Architecture, Enabling Structure) from a short structured
  interview, present the scores for calibration, and record the Week 0
  baseline in the scorecard. Use when someone says "assess the team", "score
  our team", "team assessment", "baseline the team", "how healthy is the
  team", "run the rubric", after setup on the Sponsor-first path, or once the
  Sponsor has confirmed on the Rollout-Lead-first path.
disable-model-invocation: true
argument-hint: "[team or department name]"
---

# Assess team

Pass 1. Thirty to forty minutes in Team and Department modes; ten in Founder mode. The output is a baseline, not a verdict. The person running the program knows things an interview will not surface, so every score is presented for calibration before anything is written.

Read first: the state folder's `company-context.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/team-rubric.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/scorecard-model.md`; `${CLAUDE_PLUGIN_ROOT}/skills/adapters/SKILL.md`. Run the adapter detection procedure before writing.

## 0. Scope and existing state

| Situation | Action |
|---|---|
| No state folder | Stop and say to run `/ai-adoption:setup` first. |
| `$1` given | Assess that team or department. Metric names carry the scope as a prefix (step 5) so rows do not collide with company-level rows. |
| Department mode, no `$1` | Assess the department recorded as scope in `company-context.md`. |
| Founder or Team mode, no `$1` | Assess the whole company. |
| `scorecard` already holds Week 0 Team rows for this scope | Show them. Ask: re-assess (rows are updated, not duplicated) or stop. |
| Sponsor not yet confirmed | Proceed; the assessment costs nothing. Say the baseline becomes official when the Sponsor confirms. |

## 1. Intake

Four batches, three or four questions each, one message per batch. Skip any question `company-context.md` already answers (headcount, roles, archetype, data profile). Ask for examples, not ratings; the rubric turns examples into scores. If the user offers a persona guess about a person, say "persona mapping happens in `map-personas`" and move on.

| Batch | Questions | Feeds |
|---|---|---|
| 1. Who the team is | Which roles exist and how many people hold each? How long have most people worked together? Who decides what, and where is that unclear? Which functions have nobody owning them? | Enabling Structure, Mutual Trust |
| 2. How it works today | How does a new person learn how things work? Where do decisions get recorded? What are the weekly rituals (standups, reviews, retros)? How does work hand off between functions? | Communication Architecture, Shared Mental Models |
| 3. Where the gaps are | What regularly falls through the cracks? Tell me about the last mistake that mattered and what happened next. What breaks if one specific person is out for a month? When did the team last change direction, and how did it go? | Psychological Safety, Adaptive Capacity, Mutual Trust |
| 4. AI readiness | Roughly how many people already use AI tools, and in which functions? What has leadership said about AI, if anything? What would happen the first time an AI-assisted document contained an error? Would everyone describe the mission the same way? | Compelling Direction, Psychological Safety, Adaptive Capacity |

Public material (website, recent announcements) may validate Compelling Direction only, and only if the user agrees. Do not add claims from it.

Founder mode: collapse to two batches (1 with 2, 3 with 4), four questions each, and accept one-line answers.

## 2. Score

Apply the rubric anchors (5 / 3 / 1, interpolate; halves allowed). For each primitive record a score, a confidence, and the one or two signals that drove it.

| Confidence | Rule |
|---|---|
| High | Three or more clear signals from intake or public material |
| Medium | One or two signals |
| Low | Inferred from archetype norms or from silence; say so |

Present one table:

| # | Primitive | Weight | Score | Confidence | Signals |
|---|---|---|---|---|---|

Then the composite (sum of score x weight, two decimals) and the band from the rubric. Name the two lowest and the two highest primitives in one sentence each.

## 3. Calibrate

Ask, in one message: "Which of these would you move, and why? Anything over- or under-weighted?" Mention the two calibration patterns from the rubric (self-scores run high on Psychological Safety and low on Communication Architecture) without pre-judging the answer.

Apply the changes and recompute composite and band. Keep a list: primitive, from, to, stated reason. Do not move a score without a reason you can record.

## 4. Targets

Communication Architecture and Enabling Structure are the two primitives the program moves fastest. Propose targets for those two and the composite; all editable, all confirmed by the user.

| Metric | Month 3 (target) | Month 6 (stretch) | Month 12 |
|---|---|---|---|
| Communication Architecture score | baseline +0.5 | baseline +0.9 | next band boundary or +1.3, whichever is lower; cap 5.0 |
| Enabling Structure score | baseline +0.5 | baseline +0.9 | same rule |
| Composite | baseline +0.25 | baseline +0.5 | recomputed from the two above |

If Psychological Safety or Mutual Trust scores below 2.5, say that the Sponsor must script the first-error response before Week 1, and carry that into the summary line.

## 5. Confirm and write

Summarize: scope, eight scores with confidence, composite and band, calibration changes, targets. On yes:

1. `scorecard`: eight rows, `week` 0, `category` Team. Metrics are the seven primitive names plus `Composite`, prefixed `<scope>: ` when `$1` or a department scope applies. `baseline` = score. `target` and `stretch` filled for Communication Architecture, Enabling Structure, and Composite; blank for the rest. `status` blank; `updated` today. Rows setup seeded for these metrics take `update_row`; the others take `write_row`.
2. `company-context.md` > Notes and decisions, one dated line: "Team assessment (<scope>): composite X.XX, <band>; lowest: A (n), B (n); Low-confidence: ...; calibration moved: ...; month-12 targets: CA n, ES n; first-error script needed: yes/no."

Founder mode: the three-metric scorecard does not track Team rows weekly. Write the eight rows anyway; `impact-report` reads them as the baseline.

## 6. Next

Say: "Next: `/ai-adoption:map-primitives`." In Founder mode continue in the same session. Do not run it unless asked.

## Rules

- Never write a score that has not been shown for calibration.
- No names next to scores or signals. The signals column describes behaviour, not people.
- Low-confidence scores stay labelled Low in the summary line so the mid-point survey can revisit them.
- The scorecard is the baseline of record. If it is not there, it did not happen.
