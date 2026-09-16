---
name: kickoff
description: >
  Draft the Sponsor's Week 1 kickoff message for the team channel from the
  Sponsor's own real AI use cases, the program's why in operating terms, what
  happens in the next ten days, and the single ask: one win posted by day 10.
  Also drafts the Day-10 highlights skeleton. Use at the start of Week 1 or
  when someone says "write the kickoff message", "announce the program",
  "sponsor's launch post", "draft the day 10 highlights", or "what does the
  CEO say to the team".
disable-model-invocation: true
---

# Kickoff

The first visible act of the program. Conformists, the largest group, decide from this message whether AI is optional here. It works only when the Sponsor describes things they actually did.

Follow `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/show-first.md`: do the work first from state and what is already known, show it (widget when available, table otherwise), and ask only for the gaps, with tappable options.

Read first: `<state>/company-context.md` (Sponsor, Sponsor confirmed status, emphasis, data profile, capabilities, start date); `<state>/primitives.csv` (top three by priority_rank); `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/program.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/references/defining-moments.md`; `${CLAUDE_PLUGIN_ROOT}/skills/frameworks/SKILL.md` (principles). Check `<state>/outputs/` for a workshop plan with a date.

| Mode | What changes |
|---|---|
| Founder | Skipped. There is no team to address; the founder's Week 1 is daily use of three workflows. |
| Team | Full run. |
| Department | The message goes to the department's channel from the department head. Also draft a two-line note to the company-level Sponsor saying the department has started; print it, do not send it. |

## 1. The Sponsor's real use cases

Ask, addressed to the Sponsor (relay through the Rollout Lead if the Sponsor is not in chat): "Name two or three things you personally did with AI in the last two weeks: the task, the tool, what came out, and what you checked before using it."

| Answer | Action |
|---|---|
| Two or three concrete tasks | Continue |
| One | Continue, and ask for a second by the workshop |
| None, or "just say I've been using it" | Stop. Do not invent examples. Suggest two tasks from the top three primitives for the Sponsor to do this week, with starter prompts, and offer to re-run after. |

Never write an example the Sponsor did not confirm. A Skeptic will ask about it in the channel, and the Sponsor must be able to answer.

## 2. Draft the kickoff message

Written in the Sponsor's first person, 200-300 words, plain. Fill each part from the source listed.

| Part | Content | Source |
|---|---|---|
| What I did | The two or three real tasks, one line each, including what was checked | Step 1 |
| Why we are doing this | Two lines in operating terms: the bottlenecks and the work it frees up. No AI vocabulary, no productivity percentages. | Program emphasis, top three primitives |
| The craft line | One sentence: this removes the administrative tax; the expert work stays with the experts | Principle 3 |
| Next ten days | Workshop date and length, the channel, tools and licenses, who to ask (Rollout Lead, team AI leads) | Capabilities, workshop plan, People table |
| The data rule | One line on what never goes in a prompt | Data-handling profile |
| The one ask | "By day 10, post one thing you did with AI in the channel: the task, the tool, the time it took, what you checked." Nothing else is asked. | `program.md` First Win |
| Errors | One line: when it gets something wrong, say so in the channel; that is the review working | `defining-moments.md` |

Do not add a tool list, a policy, or a vision paragraph. Do not mention personas or the private file.

## 3. Draft the Day-10 highlights skeleton

A second message, sent by the Sponsor on day 10, with blanks the Rollout Lead fills from the channel and `use_cases`:

| Part | Placeholder |
|---|---|
| Count | "__ of __ people posted a win" |
| Three wins | Task, tool, time, quoted with the author's permission; the use case's `visibility` must allow it |
| One that did not work | The task, what was wrong, who caught it, thanked by name |
| Next | Workshop follow-ups; the first role playbook date (Week 3-4) |

## 4. Output and delivery

Write both drafts to `<state>/outputs/kickoff-YYYY-MM-DD.md`, ending with `AI-assisted draft. Reviewed by: ________ (name, date)` (blank left empty). Print both in full.

Delivery follows who is in chat:

| Who is in chat | Channel adapter present | Action |
|---|---|---|
| Sponsor | Yes | Offer `post_message` under the approval gate: show the exact text and the channel name, stop, post only on the Sponsor's yes |
| Sponsor | No | Print under "Paste this into your team channel" |
| Rollout Lead only | Either | Print for the Sponsor to paste. Do not post on the Sponsor's behalf; the message must come from the Sponsor's own account or it does not count as leadership-led. |

Append a dated line to "Notes and decisions" in `company-context.md`: kickoff drafted; sent by whom and when, if known.

## Rules

- Approval gate before any post. One approval covers one post; the Day-10 message needs its own.
- Never invent a Sponsor use case, a number, or a quote.
- If the Sponsor is not confirmed in `company-context.md`, stop and say the kickoff waits for a confirmed Sponsor.
- If the Sponsor asks to make the ask mandatory or add a deadline with consequences, decline and cite principle 1: leadership-led, not mandated.
