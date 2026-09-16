# Files adapter (zero-connector)

The default and the reference implementation. State is a folder of CSV and markdown files, created from `templates/` by setup. Default location `./ai-adoption-state/`; the user can name another folder, recorded in `company-context.md`.

## Layout

```
ai-adoption-state/
├── company-context.md
├── companies.csv
├── primitives.csv
├── scorecard.csv
├── use_cases.csv
├── checkins.csv
├── personas.private.md      # leads only; never shared
├── wins.md
├── errors.md                # shared AI error log, created at first use
├── outputs/                 # generated documents
└── README.md
```

## Operations

| Operation | How |
|---|---|
| `read_table(name)` | Read `<name>.csv`. Header row is the schema. Empty file means header only. |
| `write_row(name, row)` | Append one line. Quote any field containing a comma, quote, or newline. Generate ids as described below. |
| `update_row(name, key, changes)` | Rewrite the file with the matching row changed. Never change the header. |
| `query(name, filter)` | Read the table and filter in memory. |
| `post_message` / `post_digest` | Print the text under a heading "Paste this into your team channel". Do not write it anywhere else. |
| `read_recent` | Ask the user to paste recent channel highlights, or read `wins.md` in Founder mode. |
| `schedule_event` | Print title, time, attendees, notes under "Add this to your calendar". |
| `send_survey` | Print the survey under "Send this to the team". Offer a short form: five questions maximum. |
| `collect_replies` | Ask the user to paste replies; parse them into the check-in. |
| `send_report` | Write the report to `outputs/` and print the path. |

## Keys and idempotency

| Table | Key | Rule |
|---|---|---|
| companies | `id` | slug of the company name, e.g. `acme-co` |
| primitives | `id` | `<team-number>.<n>`, e.g. `2.4`; stable once assigned |
| scorecard | `company_id` + `week` + `metric` | one row per metric per week; a re-run updates `actual` and `updated` |
| use_cases | `id` | `uc-<zero-padded n>`; never reused |
| checkins | `company_id` + `week` | one row per week; a re-run updates the row and sets `status` to `rerun` |

The re-run rule is what makes `weekly-checkin` safe to run late or twice.

## Dates and week numbers

`start_date` in `companies.csv` is the Monday of Week 0. Current week = floor((today - start_date) / 7). Weeks beyond `program_weeks` are "post-program"; monthly skills still run.

## Generated documents

Every file written to `outputs/` ends with:

`AI-assisted draft. Reviewed by: ________ (name, date)`

The adapter never fills the blank.

## Privacy

`personas.private.md` is the only file skills may write persona names to. If the user asks to sync the folder to a shared drive, exclude it and say so.
