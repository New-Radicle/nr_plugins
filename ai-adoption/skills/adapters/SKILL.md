---
name: adapters
description: >
  The capability contract every AI adoption skill uses to read and write state,
  post to a team channel, schedule events, and send surveys, plus the recipe for
  each backend (files, Notion, Airtable, Google Drive, Slack, Gmail, Google
  Calendar). Load whenever a skill needs to detect which connectors are
  available, read or write a program table, post a digest, or decide whether the
  data-handling profile allows a cloud connector. Also triggers on "which
  connectors", "where is the data stored", "use Notion instead", "switch to
  files", or "export to CSV".
---

# Adapters

Skills never name a product. They call one of the operations below; this skill maps the operation to whatever is connected, and to files when nothing is.

## Capabilities and operations

| Capability | Operations | Required |
|---|---|---|
| State store | `read_table(name)`, `write_row(name, row)`, `update_row(name, key, changes)`, `query(name, filter)` | Yes; files fallback |
| Team channel | `post_message(text)`, `post_digest(text)`, `read_recent(days)` | Yes; draft-and-paste fallback |
| Calendar | `schedule_event(title, when, attendees, notes)`, `list_upcoming(days)` | Yes; user-adds-by-hand fallback |
| Inbox | `send_survey(recipients, questions)`, `collect_replies(since)`, `send_report(recipients, doc)` | Yes; draft-and-paste fallback |
| CRM, task tracker, HRIS, code host | read-only evidence and optional writes | Optional |
| Economic Index | `occupation_usage(query)`, `category_shares()` | Optional; cached table fallback |

Tables are the five in the data model (`companies`, `primitives`, `scorecard`, `use_cases`, `checkins`) with the columns in `templates/*.csv`. Every adapter maps those columns one to one so a company can move backends without losing anything.

## Detection procedure

Run at setup and again at the start of any skill that writes:

1. List the tools available in this session. Match by name pattern, case-insensitive:
   - State store: `*notion*` (create-pages, query-data-sources), `*airtable*`, `*drive*` (create_file, update_file, read_file_content), `*atlassian*` or `*confluence*`
   - Channel: `*slack*send*`
   - Calendar: `*calendar*create_event*`
   - Inbox: `*gmail*` (create_draft, send_message, search_threads)
   - Economic Index: `*econ_index*`
   - Optional: `*hubspot*`, `*salesforce*`, `*attio*`, `*linear*`, `*jira*`, `*asana*`, `*monday*`, `*clickup*`, `*gusto*`, `*bamboohr*`, `*github*`
2. Read `company-context.md` > Data-handling profile. If it contains Controlled, use files for everything and do not offer cloud adapters, even if they are connected. If it contains Regulated, only adapters on the allow-list in company-context may be used.
3. Read `company-context.md` > Capabilities for the adapter the company already chose. Do not switch adapters silently; if a chosen adapter's tools are missing this session, say so and offer files for this run.
4. Microsoft 365 (SharePoint, Outlook, Teams) is search-only through its connector as of this release: it can be read for evidence but cannot be a state store, channel, or inbox. Use the paste fallback and say why.

## Approval gate

Before any `post_message`, `post_digest`, `send_survey`, `send_report`, or `schedule_event` with attendees: show the exact text, recipients, and destination, and stop. Proceed only when the Sponsor or Rollout Lead says yes in this chat. This holds even when the user asked for the post in the same message. One approval covers one send.

## Fallbacks

| Capability | When nothing is connected |
|---|---|
| State store | Files in `./ai-adoption-state/` (see `references/files.md`) |
| Team channel | Print the message; the user pastes it. Record in `checkins` that the digest was drafted, not posted. |
| Calendar | Print the event details; the user adds it by hand. |
| Inbox | Print the survey as a message; the user sends it and pastes replies back. The skill parses pasted replies. |
| Economic Index | Use the cached table in `skills/economics/references/econ-index-cached.md` and label every figure "cached, release YYYY-MM". |

## Recipes

| Backend | File |
|---|---|
| Files (zero-connector, the default) | `references/files.md` |
| Notion | `references/notion.md` |
| Airtable | `references/airtable.md` |
| Google Drive (CSV and markdown in a folder) | `references/drive.md` |
| Slack | `references/slack.md` |
| Gmail | `references/gmail.md` |
| Google Calendar | `references/google-calendar.md` |

Adding a backend means adding one file here with, per operation, the tool to call, the parameter mapping, and what to do on failure.

## Export

`export_all()` writes every table as CSV plus `company-context.md` into `./ai-adoption-state/export-YYYY-MM-DD/`, whatever the backend. Any skill can be asked to export.
