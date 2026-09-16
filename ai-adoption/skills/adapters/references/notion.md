# Notion adapter (state store)

The five program tables live as five Notion databases under one parent page. `company-context.md`, `wins.md`, `errors.md`, and `personas.private.md` stay in the local `./ai-adoption-state/` folder; only the tables and generated documents move to Notion.

Tool names in Claude Code are `mcp__<server>__<tool>` and the server segment varies per installation. Always match the tool whose name ends in the suffix given below; never hard-code the prefix.

## Gates

| Gate | Rule |
|---|---|
| Data profile | Controlled: this adapter is never offered, even if connected. Regulated: only if Notion is on the allow-list in `company-context.md` > Data-handling profile > Cloud connectors allowed. |
| Approval | `send_report` to Notion creates a page others can see. Show the full text and the destination page, stop, and proceed only when the Sponsor or Rollout Lead says yes in this chat. One approval covers one page. Row writes are not gated. |
| Persona data | `personas.private.md` is never written to Notion. `persona_shifts` and `persona_breakdown` hold counts per persona, never names. `get-users` is never called. |

## Layout and setup

1. Match `search` with query "AI Adoption Program". If a page with that exact title exists, `fetch` it and read the five database ids from its first table block.
2. If not, ask the user which parent page the integration can see, then `create-pages` with title "AI Adoption Program" under it.
3. `create-database` five times with parent = the program page, titled `companies`, `primitives`, `scorecard`, `use_cases`, `checkins`, with the properties below.
4. Write the program page id and the five data source ids into `company-context.md` > Capabilities > State store notes. Skills read ids from there, never by searching again.
5. Generated documents go under a child page "Outputs" created the same way.

## Properties per table

The key column is the title property. Select options are created by Notion on first write; never rename an option, add a new one instead.

| companies | Type |
|---|---|
| id | title |
| name, sponsor, rollout_lead, cohort_id | rich_text |
| archetype | select (A to E) |
| size_mode | select (Founder, Team, Department) |
| data_profile | multi_select (Open, Restricted, Controlled, Regulated); exported joined with "; " |
| start_date | date |
| program_weeks | number |
| status | select |

| primitives | Type |
|---|---|
| id | title |
| company_id, team, name, owner, occupation_match, notes | rich_text |
| status | select (the status vocabulary in `frameworks/references/ai-modes.md`) |
| ai_mode | select (Automation, Augmentation, Edge, Capture, Analyze, Excluded) |
| speedup, success_rate, hours_wk, priority_rank | number |
| hours_method | select (stated, estimated) |
| quadrant | select (the quadrant names in `ai-modes.md`) |

| scorecard | Type |
|---|---|
| key | title, derived: `<company_id>-w<week>-<metric>`; not a CSV column, dropped on export |
| company_id, metric, persona_breakdown (JSON string) | rich_text |
| week, baseline, target, stretch, actual | number |
| category | select (the four scorecard categories) |
| status | select (Exceeded, On Track, At Risk, Behind) |
| updated | date |

| use_cases | Type |
|---|---|
| id | title |
| company_id, author, role, task, tool, prompt_summary, primitive_id | rich_text |
| hours_saved, week | number |
| visibility | select; only the author may change it |
| added | date |

| checkins | Type |
|---|---|
| key | title, derived: `<company_id>-w<week>`; dropped on export |
| company_id, blockers, wins, persona_shifts, digest_text | rich_text |
| week, hours_saved_avg, active_pct | number |
| ran_on | date |
| status | select (drafted, posted, rerun) |

## Operations

| Operation | Tool suffix | Parameters |
|---|---|---|
| `read_table(name)` | `query-data-sources` | data_source_id from company-context; page through every result; map properties back to CSV columns in template order |
| `write_row(name, row)` | `create-pages` | parent = the data source id; one property per column; run the key query first and refuse a duplicate key |
| `update_row(name, key, changes)` | `query-data-sources` then `update-page` | find the page by key, then send only the changed properties |
| `query(name, filter)` | `query-data-sources` | pass the filter on the property when the tool accepts it; otherwise read the table and filter in memory |
| `send_report(recipients, doc)` | `create-pages` | after the gate: parent = Outputs page, title = document name, body = the document |
| Comments on a use case | `create-comment`, `get-comments` | optional; only on `use_cases` pages, never on checkins |
| `export_all()` | `query-data-sources` x5 | write each table as CSV into `./ai-adoption-state/export-YYYY-MM-DD/` |
| `move-pages`, `duplicate-page`, `update-database` | not used by skills | reserved for a user-directed reorganisation |

`post_message`, `post_digest`, `schedule_event`, and `send_survey` are not Notion operations; see `slack.md`, `google-calendar.md`, `gmail.md`, or the paste fallback.

## Keys and idempotency

Same keys and re-run rules as `files.md`. The Notion page id is a handle, never a key; a row moved to another backend keeps its key. A re-run of `weekly-checkin` updates the existing `checkins` page and sets `status` to `rerun`.

## Generated documents

Every page created under Outputs ends with a paragraph block reading exactly:

`AI-assisted draft. Reviewed by: ________ (name, date)`

The adapter never fills the blank. The local `outputs/` copy remains the record; the Notion page is the shared copy.

## Privacy

`personas.private.md` never leaves the local folder and is excluded from every sync or export the user asks for. Rows carry names only where the CSV column already does (sponsor, rollout_lead, owner, author). If the workspace is shared beyond the company, say so before the first write.

## Failure paths

| Condition | What to do |
|---|---|
| No tool ending in `create-pages` or `query-data-sources` in this session | Say the Notion adapter is not connected this session; offer files for this run; do not switch the adapter in company-context |
| Error mentions unauthorized, expired, or token | Say the Notion authorization has expired and needs reconnecting in the connector settings; fall back to files for this run |
| Error mentions not found or the page is not shared with the integration | Say which page or database was refused; ask the user to share it; fall back to files for this run |
| Rate limit or transient error | Report it; retry once only if the user says so; never retry silently |
| Property missing on a database | Add it with `update-database` (or `update-data-source`) after telling the user; never drop or rename a property |

Microsoft 365 is not covered: its connector is search-only in this release, so SharePoint, Outlook, and Teams users use the files adapter and the paste fallbacks.
