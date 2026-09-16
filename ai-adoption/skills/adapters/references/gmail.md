# Gmail adapter (inbox)

The inbox capability: send the baseline and mid-point surveys, collect the replies, and send the impact report. Gmail is never a state store; only counts and summaries reach the tables.

Tool names in Claude Code are `mcp__<server>__<tool>` and the server segment varies per installation. Always match the tool whose name ends in the suffix given below; never hard-code the prefix.

## Gates

| Gate | Rule |
|---|---|
| Data profile | Controlled: this adapter is never offered, even if connected. Regulated: only if Gmail is on the allow-list in `company-context.md` > Data-handling profile > Cloud connectors allowed. |
| Approval | Anything that sends (`send_message`, `reply`, `forward`) is gated: show the exact subject, body, and every recipient, stop, and proceed only when the Sponsor or Rollout Lead says yes in this chat. One approval covers one send. Creating a draft is not gated, because the user sends it. |
| Persona data | Persona names never appear in a survey, a report, or a label. `personas.private.md` stays in the local folder. Survey answers are summarised as counts; no individual answer is written to any table. |

## Setup

1. Match `create_label` with the name "AI Adoption Program". If it already exists, reuse it.
2. Record the label name and, if returned, its id in `company-context.md` > Capabilities > Inbox notes.
3. Agree the subject convention with the user: `AI adoption survey, Week <n>` for surveys and `AI adoption impact report` for the report. Subjects are how replies are found.
4. Default mode is draft: the plugin writes the draft and the user presses send. Switch to direct send only when the user asks and the gate passes each time.

## Operations

| Operation | Tool suffix | Parameters |
|---|---|---|
| `send_survey(recipients, questions)` default | `create_draft` | to = recipients, subject per the convention, body = at most five questions, each numbered, with a one-line reply instruction; print "Draft created; send it from Gmail" |
| `send_survey` direct | `send_message` | same fields, after the gate |
| Revise an unsent survey | `list_drafts`, `get_draft`, `update_draft` | find the draft by subject; replace the body; never create a second draft for the same week |
| `collect_replies(since)` | `search_threads` then `get_thread` | query `subject:"AI adoption survey, Week <n>" newer_than:<days>d`; read each thread; parse numbered answers; `label_thread` with the program label so a re-run skips it |
| One reply that needs a follow-up | `get_message` then `reply` | after the gate; only when the user asks |
| `send_report(recipients, doc)` | `create_draft` (default) or `send_message` (after the gate) | subject per the convention; body = the executive summary and a link to the report in the state store or `outputs/`; attach the file only when the tool accepts attachments in this installation |
| `forward` | not used by skills | reserved for the user |

Survey cadence: baseline in Week 0, mid-point in Week 8, and the optional end-of-program survey in Week 12. Founder mode sends no surveys.

## What is written back

| Table | Column | Content |
|---|---|---|
| checkins | `active_pct`, `hours_saved_avg` | computed from the replies for that week |
| checkins | `blockers`, `wins` | themes, not quotes; no names |
| scorecard | `actual` for survey-derived metrics | the aggregate, `persona_breakdown` as counts only |
| company-context.md | Notes and decisions | "Week <n> survey: sent <date>, <k> of <m> replied" |

Reply counts are the only respondent data that reaches the state folder. Raw replies stay in the mailbox.

## Keys and idempotency

One survey per week per company. Before drafting, `list_drafts` and `search_threads` on the subject; if a draft or a sent thread exists, say so and offer to revise or resend instead of duplicating. `collect_replies` skips threads already carrying the program label, so it is safe to run twice; a late reply is picked up on the next run.

## Generated documents

The report body and any attached file end with:

`AI-assisted draft. Reviewed by: ________ (name, date)`

The adapter never fills the blank. Surveys are messages, not documents, and carry no review line.

## Privacy

Recipients come from the user, never from a directory search. Do not read threads outside the survey subject and label. Do not write reply text, reply timing per person, or non-response lists to any table; a non-responder nudge is drafted by the user, not the plugin. If the company is Regulated, survey questions must not ask for personal or health data.

## Failure paths

| Condition | What to do |
|---|---|
| No tool ending in `create_draft` or `search_threads` in this session | Say the Gmail adapter is not connected this session; print the survey under "Send this to the team"; parse pasted replies for this run |
| Error mentions unauthorized, expired, or token | Say the Google authorization has expired and needs reconnecting; use the paste fallback for this run |
| Draft created but not visible in `list_drafts` | Say the draft may not have saved; ask the user to check Gmail before creating another |
| `send_message` errors after approval | Report it; do not resend without a fresh yes; never retry silently, because a silent retry can double-send |
| Rate limit or transient error on reads | Report it; retry once only if the user says so |

Microsoft 365 is not covered: its connector is search-only in this release, so Outlook users use the paste fallback for surveys and reports and the files adapter for state.
