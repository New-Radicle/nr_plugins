# Airtable adapter (state store)

The five program tables live as five tables in one base named "AI Adoption". `company-context.md`, `wins.md`, `errors.md`, `outputs/`, and `personas.private.md` stay in the local `./ai-adoption-state/` folder; Airtable holds rows, not documents.

Tool names in Claude Code are `mcp__<server>__<tool>` and the server segment varies per installation. Always match the tool whose name ends in the suffix given below; never hard-code the prefix.

## Gates

| Gate | Rule |
|---|---|
| Data profile | Controlled: this adapter is never offered, even if connected. Regulated: only if Airtable is on the allow-list in `company-context.md` > Data-handling profile > Cloud connectors allowed. |
| Approval | No Airtable operation posts, sends, or invites, so row writes are not gated. If a share or notify tool appears in a future connector release, show the exact text, recipients, and destination, stop, and proceed only when the Sponsor or Rollout Lead says yes in this chat. `send_report` is not an Airtable operation; reports go to `outputs/` or another adapter, which keep their own gate. |
| Persona data | `personas.private.md` is never written to Airtable. `persona_shifts` and `persona_breakdown` hold counts per persona, never names. |

## Layout and setup

1. Match `ping` to confirm the connector answers. Then `list_bases` and look for a base named "AI Adoption".
2. The connector cannot create a base. If none exists, ask the user to create an empty base with that name, then continue.
3. `list_tables_for_base`. For each missing table run `create_table` with the fields below. For an existing table run `get_table_schema` and add any missing column with `create_field`; never delete or rename a field.
4. If no update-records tool is present, add a singleLineText field `supersedes` to every table (see Operations).
5. Record the base id and the five table ids in `company-context.md` > Capabilities > State store notes. Skills read ids from there.

## Fields per table

The key column is the primary field. Single-select options are added on first write; never rename an option.

| companies | Type |
|---|---|
| id | singleLineText (primary) |
| name, sponsor, rollout_lead, cohort_id | singleLineText |
| archetype | singleSelect (A to E) |
| size_mode | singleSelect (Founder, Team, Department) |
| data_profile | multipleSelects (Open, Restricted, Controlled, Regulated); exported joined with "; " |
| start_date | date |
| program_weeks | number (integer) |
| status | singleSelect |

| primitives | Type |
|---|---|
| id | singleLineText (primary) |
| company_id, team, name, owner, occupation_match | singleLineText |
| notes | multilineText |
| status | singleSelect (the status vocabulary in `frameworks/references/ai-modes.md`) |
| ai_mode | singleSelect (Automation, Augmentation, Edge, Capture, Analyze, Excluded) |
| speedup, success_rate, hours_wk | number (2 decimals) |
| hours_method | singleSelect (stated, estimated) |
| priority_rank | number (integer) |
| quadrant | singleSelect (the quadrant names in `ai-modes.md`) |

| scorecard | Type |
|---|---|
| key | singleLineText (primary), derived: `<company_id>-w<week>-<metric>`; dropped on export |
| company_id, metric | singleLineText |
| persona_breakdown | multilineText (JSON string) |
| week | number (integer) |
| baseline, target, stretch, actual | number (2 decimals) |
| category | singleSelect (the four scorecard categories) |
| status | singleSelect (Exceeded, On Track, At Risk, Behind) |
| updated | date |

| use_cases | Type |
|---|---|
| id | singleLineText (primary) |
| company_id, author, role, tool, primitive_id | singleLineText |
| task, prompt_summary | multilineText |
| hours_saved | number (2 decimals) |
| week | number (integer) |
| visibility | singleSelect; only the author may change it |
| added | date |

| checkins | Type |
|---|---|
| key | singleLineText (primary), derived: `<company_id>-w<week>`; dropped on export |
| company_id | singleLineText |
| blockers, wins, persona_shifts, digest_text | multilineText |
| week | number (integer) |
| hours_saved_avg, active_pct | number (2 decimals) |
| ran_on | date |
| status | singleSelect (drafted, posted, rerun) |

## Operations

| Operation | Tool suffix | Parameters |
|---|---|---|
| `read_table(name)` | `list_records_for_table` | base id and table id from company-context; follow the offset until exhausted; drop any record whose id appears in another record's `supersedes`; map fields to CSV columns in template order |
| `write_row(name, row)` | `create_records_for_table` | one record with a field per column; run the key query first and refuse a duplicate key |
| `update_row(name, key, changes)` | an update-records tool if present (name contains `update` and `record`) | record id from the key query; send only the changed fields |
| `update_row` without an update tool | `create_records_for_table` | read the record, create a new one with the full row and the changes applied, and set `supersedes` to the old record id; the old record is never modified. Tell the user once per session that the table will hold superseded rows. |
| `query(name, filter)` | `list_records_for_table` | pass a filter formula when the tool accepts one; otherwise read the table and filter in memory |
| `export_all()` | `list_records_for_table` x5 | write each table as CSV into `./ai-adoption-state/export-YYYY-MM-DD/` |
| A search-records tool, if present | optional | same as `query`; never required |

`post_message`, `post_digest`, `schedule_event`, `send_survey`, and `send_report` are not Airtable operations; see `slack.md`, `google-calendar.md`, `gmail.md`, or the paste fallback.

## Keys and idempotency

Same keys and re-run rules as `files.md`. The Airtable record id is a handle, never a key. When two records share a key because of the read-modify-create path, the record named in `supersedes` is dropped on read and on export; if the chain is broken, the one with the latest `updated` (scorecard) or `ran_on` (checkins) wins.

## Generated documents

Airtable holds no documents. Reports and playbooks are written to the local `outputs/` folder and end with:

`AI-assisted draft. Reviewed by: ________ (name, date)`

The adapter never fills the blank.

## Privacy

`personas.private.md` never leaves the local folder and is excluded from every sync or export. Names appear only in the columns the CSV already has (sponsor, rollout_lead, owner, author). If the base is shared with collaborators outside the company, say so before the first write.

## Failure paths

| Condition | What to do |
|---|---|
| No tool ending in `list_records_for_table` or `create_records_for_table` in this session | Say the Airtable adapter is not connected this session; offer files for this run; do not switch the adapter in company-context |
| `ping` fails or the error mentions unauthorized, expired, or token | Say the Airtable authorization has expired and needs reconnecting; fall back to files for this run |
| Base or table not found | Say which id was refused; ask the user to check the base name and sharing; fall back to files for this run |
| Field type mismatch on write | Report the field and the value; do not coerce silently; ask before changing the schema |
| Rate limit or transient error | Report it; retry once only if the user says so; never retry silently |

Microsoft 365 is not covered: its connector is search-only in this release, so SharePoint, Outlook, and Teams users use the files adapter and the paste fallbacks.
