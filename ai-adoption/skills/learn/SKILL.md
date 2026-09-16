---
name: learn
description: >
  Personal learning path for working with AI, built on the five foundations
  layers and the person's own role. Use when someone asks "where do I start",
  "what do I need to learn", "what do I need to know to do X with AI",
  "explain the basics for my role", "am I ready to automate this", "teach me",
  or "what should I read first". Read-only; prints the path and points to
  exercises the person can do on their own material in twenty minutes.
argument-hint: "[topic or task, e.g. 'write grant reports' or 'automate the weekly digest']"
---

# Learn

Turns "what do I need to learn?" into a short, ordered path. Five layers, scored quickly, then the two that matter most for this person's work. No lecture. Every step ends in something they can try in twenty minutes on their own material.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: `${CLAUDE_PLUGIN_ROOT}/skills/foundations/SKILL.md` and its references; `<state>/company-context.md` if a state folder exists (role, data profile, capabilities); `<state>/primitives.csv` if present (the person's team and top primitives). Without a state folder, work from what the person says.

## 1. Who is asking, and about what

If `$1` is a task ("automate the weekly digest", "write grant reports with AI"), map it to layers before anything else:

| Task shape | Layers that decide it |
|---|---|
| Draft, summarise, rewrite, translate | 1, 2, 5 |
| Analyse data, check numbers, compare documents | 1, 2, 5 |
| Work inside a system (notes, mail, calendar, CRM, tracker) | 3, 4, 5 |
| Run something on a schedule or without watching | 4, 5, then 3 |
| Decide what tools or seats to buy | 1, 3, 4 |
| Set rules for the team | 5, 2, 4 |

Then ask, in one message, three things: their role and what they produce most weeks; what they have already done with AI (nothing, chat only, connected tools, built something); and the one task they wish took less time. Skip what company-context already answers.

## 2. Quick score

One question per layer, answered yes or no by the person, or inferred from their answers above. Do not quiz; fold these into the conversation.

| Layer | You have it if you can say... |
|---|---|
| 1 The model | Which model you used for your last task, why, and what it could not have known |
| 2 What you feed it | What examples or format you gave it last time, and where your saved instructions live |
| 3 What it can reach | What your assistant is connected to and what each connection can do |
| 4 How it runs | For one automated task, what it may touch and where it stops to ask |
| 5 Whether you can trust it | For your last AI-assisted document, who checked it against what, and what stayed out of the prompt |

Score each 0 (no), 1 (partly), 2 (yes). Present the five scores in one table with a one-line reason each. Ask if any feels wrong.

## 3. The path

Pick the two lowest-scoring layers among those that matter for their task shape. Layer 5 is always on the path if it scored under 2; nobody skips it. Order: 5 first if it is on the path, then the layer with the biggest gap between where they are and what their task needs.

For each layer on the path, give:

1. **Why this one, in their terms.** One sentence tied to the task they named.
2. **The three ideas to hold.** Pulled from the layer's section in `foundations/SKILL.md`, rephrased for their role. Not the whole section.
3. **The twenty-minute exercise.** From `foundations/references/exercises.md`, the row for their kind of work, adapted to the material they named. Say what to notice.
4. **What "done" looks like.** The "you know this layer when" line from the foundations skill.
5. **What to skip for now.** One line. Reassurance that the rest can wait.

Then one line on the layers they already have, so they know what they are good at.

## 4. If they asked about a specific task

After the path, answer the task directly in three parts: what they can do today with the layers they have (a concrete first step, with a starter prompt if it is a layer 1-2 task); what they need first before the rest (the path above); and what the program's skills do for them (for example `playbook` for starter prompts, `weekly-checkin` for the digest, `handbook` for the rules), so they do not learn what the plugin already carries.

## 5. Output

Print the path in chat. Do not write files. If the person wants to keep it, tell them to paste it into their notes, or, if a Rollout Lead is asking on behalf of the team, suggest running `/ai-adoption:workshop`, which turns the same exercises into a session.

## Rules

- Never reveal or guess anyone's persona; this skill is for the learner, and persona notes are private to the leads. If the learner is a Rollout Lead asking about the team, talk in counts and roles.
- Respect the data profile: exercises use the person's own material only where the never-in-prompts list allows; say so when it does not.
- No product names. Concepts only; the glossary gives the industry term in parentheses where one exists.
- Keep it under a page. Two layers, not five. They can come back.
