# Google Drive adapter (state store)

The same CSV and markdown files as the files adapter, held in one Drive folder named "AI Adoption State" instead of a local folder. The layout, keys, and re-run rules are those of `files.md`; this recipe only changes where the files live and how they are read and written.

Tool names in Claude Code are `mcp__<server>__<tool>` and the server segment varies per installation. Always match the tool whose name ends in the suffix given below; never hard-code the prefix.

## Gates

| Gate | Rule |
|---|---|
| Data profile | Controlled: this adapter is never offered, even if connected. Regulated: only if Google Drive is on the allow-list in `company-context.md` > Data-handling profile > Cloud connectors allowed. |
| Approval | `share_file` sends an invitation, so it is gated: show the file, the recipients, and the role, stop, and proceed only when the Sponsor or Rollout Lead says yes in this chat. One approval covers one share. Writing files into the folder is not gated. |
| Persona data | `personas.private.md` is never uploaded. It stays in the local `./ai-adoption-state/` folder and is skipped by every sync and export. |

## Layout and setup

```
AI Adoption State/            (Drive folder)
├── company-context.md
├── companies.csv
├── primitives.csv
├── scorecard.csv
├── use_cases.csv
├── checkins.csv
├── wins.md
├── errors.md
├── outputs/                  (subfolder when the tool can create one; else files prefixed "output-")
└── README.md
```

1. Match `search_files` with the folder name. If found, `get_file_metadata` on it and record its id. If not, `create_file` the folder (or ask the user to create it and share the link) and record the id.
2. `create_file` each CSV from `templates/` (header row only) plus `company-context.md`, `wins.md`, and `README.md`. Never create `personas.private.md` here.
3. `get_file_permissions` on the folder. If it is open to anyone with the link, say so and ask before writing anything.
4. Record the folder id and each file id in `company-context.md` > Capabilities > State store notes. Skills read ids from there, never by searching again.
5. There is no cell-level Sheets tool in this connector. The plugin reads and writes whole CSV files. The user can open any CSV in Sheets or export a spreadsheet from it at any time; the plugin never writes to a spreadsheet.

## Operations

| Operation | Tool suffix | Parameters |
|---|---|---|
| `read_table(name)` | `read_file_content` | file id of `<name>.csv`; header row is the schema; empty body means header only |
| `write_row(name, row)` | `read_file_content` then `update_file` | re-read the file, append one quoted line, write the whole CSV back; generate ids as in `files.md` |
| `update_row(name, key, changes)` | `read_file_content` then `update_file` | re-read the file, change the matching row, write the whole CSV back; never change the header |
| `query(name, filter)` | `read_file_content` | read the table and filter in memory |
| `read_recent` (Founder mode) | `read_file_content` | read `wins.md` |
| `send_report(recipients, doc)` | `create_file` | write the document into `outputs/`; print the link; `share_file` only after the gate |
| `export_all()` | `read_file_content` x5, then `copy_file` or local write | write CSVs into `./ai-adoption-state/export-YYYY-MM-DD/` locally; `copy_file` when the user wants the snapshot in Drive too |
| Detect outside edits | `get_file_metadata` or `list_recent_files` | compare `modifiedTime` with the last write this session before writing |
| `download_file_content` | rarely | only for a non-text file the user placed in the folder as evidence |

`post_message`, `post_digest`, `schedule_event`, and `send_survey` are not Drive operations; see `slack.md`, `google-calendar.md`, `gmail.md`, or the paste fallback.

## Race risk

`update_file` replaces the whole file. Two writers, or one writer and a person editing in Sheets, can lose rows. Rules:

| Rule | Why |
|---|---|
| Re-read immediately before every write, not at the start of the skill | A read at the start of `weekly-checkin` is stale by the time the digest is approved |
| Check `modifiedTime` before writing; if it changed since the read, re-read and re-apply the change | Cheap detection of a concurrent edit |
| Write one table per call and finish it before starting the next | Keeps the window small |
| Say so when a row count drops between read and write, and stop | Never overwrite a shrinking file silently |

## Keys and idempotency

Identical to `files.md`: `companies.id`, `primitives.id`, `scorecard` on `company_id` + `week` + `metric`, `use_cases.id`, `checkins` on `company_id` + `week`. A re-run updates the row rather than appending one. Drive file ids are handles, never keys.

## Generated documents

Every file written to `outputs/` ends with:

`AI-assisted draft. Reviewed by: ________ (name, date)`

The adapter never fills the blank. Documents are plain markdown or CSV; the plugin does not create Docs or Sheets native files.

## Privacy

`personas.private.md` stays local and is excluded from every upload, copy, and export. `use_cases` rows keep the author's `visibility` flag; only the author changes it. Before the first write, state who can see the folder as reported by `get_file_permissions`.

## Failure paths

| Condition | What to do |
|---|---|
| No tool ending in `read_file_content` or `update_file` in this session | Say the Drive adapter is not connected this session; offer files for this run; do not switch the adapter in company-context |
| Error mentions unauthorized, expired, or token | Say the Google authorization has expired and needs reconnecting; fall back to files for this run |
| File or folder not found | Say which id was refused; ask the user to check the folder and sharing; fall back to files for this run |
| `modifiedTime` changed between read and write | Re-read once, re-apply, and tell the user; if it changes again, stop and ask |
| Quota, rate limit, or transient error | Report it; retry once only if the user says so; never retry silently |

Microsoft 365 is not covered: its connector is search-only in this release, so SharePoint, Outlook, and Teams users use the files adapter and the paste fallbacks.
