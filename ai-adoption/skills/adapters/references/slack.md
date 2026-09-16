# Slack adapter (team channel)

The team channel capability: post the weekly digest and program messages, and read recent channel activity for wins and blockers. Slack is never a state store; rows stay in whichever state adapter the company chose.

Tool names in Claude Code are `mcp__<server>__<tool>` and the server segment varies per installation. Always match the tool whose name ends in the suffix given below; never hard-code the prefix.

## Gates

| Gate | Rule |
|---|---|
| Data profile | Controlled: this adapter is never offered, even if connected. Regulated: only if Slack is on the allow-list in `company-context.md` > Data-handling profile > Cloud connectors allowed. |
| Approval | Before every `post_message`, `post_digest`, and canvas creation: show the exact text and the channel name, stop, and proceed only when the Sponsor or Rollout Lead says yes in this chat. This holds when the user asked for the post in the same message. One approval covers one post. |
| Persona data | Persona names never appear in a post, a canvas, or a search query. `personas.private.md` stays in the local folder. Digests report counts, not who is which persona. |

## Setup

1. Ask the user for the program channel name. Match `slack_search_channels` with that name and confirm the channel id with the user.
2. Record the channel name and id in `company-context.md` > Capabilities > Team channel notes. Skills read the id from there, never by searching again.
3. Founder mode has no channel; do not set this adapter up for it.
4. Surveys are not sent through Slack; see `gmail.md` or the paste fallback.

## Operations

| Operation | Tool suffix | Parameters |
|---|---|---|
| `post_message(text)` | `slack_send_message` | channel id from company-context; text as approved, unchanged; after posting, record the timestamp in `company-context.md` > Notes and decisions |
| `post_digest(text)` | `slack_send_message` | same; then `update_row("checkins", key, {status: "posted"})`; if the user declines, set `status` to `drafted` and print the text for pasting |
| `read_recent(days)` | `slack_read_channel` | channel id and the day window; summarise wins, blockers, and use cases; quote nothing longer than one line |
| Follow a digest thread | `slack_read_thread` | the digest message timestamp; collect replies into `blockers` and `wins` for the next check-in |
| Find wins across channels | `slack_search_public_and_private` | query on the win convention the team agreed (for example a `#aiwin` tag) limited to the program window; `slack_search_public` when private channels are out of scope |
| Resolve a mention in a draft | `slack_search_users` | only to turn a name the user typed into a mention; never to list people or build a persona map |
| Use-case library page (Week 10) | `slack_create_canvas` | after the gate: title "AI use-case library", body from `use_cases` rows whose `visibility` allows team viewing; end with the review line |

## Digest shape

| Section | Source |
|---|---|
| Headline | `checkins.hours_saved_avg`, `checkins.active_pct`, week number |
| Wins | `checkins.wins`, at most three |
| Blockers and the one intervention | `checkins.blockers`, the action chosen in `weekly-checkin` |
| Ask | one request, for example "post one use case by Friday" |

Keep it under 200 words. No persona labels, no individual usage numbers, no names beyond the authors of wins they posted themselves.

## Keys and idempotency

The digest for a week is posted at most once. Before posting, read `checkins` for `company_id` + `week`; if `status` is `posted`, say so and offer a correction post instead of a repeat. `read_recent` is idempotent; running it twice adds nothing to the tables until `weekly-checkin` writes.

## Generated documents

A canvas is a generated document. It ends with:

`AI-assisted draft. Reviewed by: ________ (name, date)`

The adapter never fills the blank. Digests are messages, not documents, and carry no review line.

## Privacy

Channel reads are evidence, not a record. Copy into `use_cases` only what a person posted about their own work, with their name as author, and keep the `visibility` default the team chose. Never copy usage counts per person into any table. Never search direct messages.

## Failure paths

| Condition | What to do |
|---|---|
| No tool ending in `slack_send_message` in this session | Say the Slack adapter is not connected this session; print the text under "Paste this into your team channel"; set `checkins.status` to `drafted` |
| Error mentions unauthorized, expired, or token | Say the Slack authorization has expired and needs reconnecting; use the paste fallback for this run |
| Channel not found or the app is not in the channel | Say which channel id was refused; ask the user to add the app to the channel; use the paste fallback for this run |
| Post succeeds but the tool returns no timestamp | Record `posted` with today's date and say the timestamp is unknown |
| Rate limit or transient error | Report it; retry once only if the user says so; never retry silently, because a silent retry can double-post |

Microsoft 365 is not covered: its connector is search-only in this release, so Teams users use the paste fallback for the channel and the files adapter for state.
