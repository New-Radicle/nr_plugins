# AI adoption state folder

This folder is the program's system of record when no connector is used. Everything here is plain text so it can be moved into Notion, Airtable, a Drive folder, or a spreadsheet at any time.

| File | What it holds | Who writes it |
|---|---|---|
| company-context.md | Identity, roles, data profile, capabilities, assumptions, risk matrix | setup, risk-matrix, economics |
| companies.csv | One row per company (one row unless running a cohort) | setup |
| primitives.csv | Operational primitives with status, AI mode, speedup, priority | map-primitives, prioritize |
| scorecard.csv | Metrics with baseline, target, stretch, actual | assess-team, scorecard, weekly-checkin |
| use_cases.csv | Documented AI use cases with a visibility flag | weekly-checkin, playbook |
| checkins.csv | One row per week; a re-run updates the row rather than adding one | weekly-checkin |
| personas.private.md | Persona map, leads only | map-personas |
| wins.md | Personal wins log (Founder mode) | weekly-checkin |
| outputs/ | Generated documents (sponsor brief, playbooks, handbook, reports) | various |

Keep `personas.private.md` out of any shared drive. Everything else can be shared.
