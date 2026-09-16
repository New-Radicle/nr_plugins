# Economic Index: cached snapshot

Use only when no `econ_index_*` tool is available in the session. Label every figure taken from this file "cached, release 2026-05". When the connector is present, query it instead and cite the live release period.

| Field | Value |
|---|---|
| Source | Anthropic Economic Index, https://www.anthropic.com/economic-index |
| Release period | 2026-05 |
| Snapshot taken | 2026-06-24 |
| License | CC BY 4.0 |
| Coverage | 22 job categories, 718 published occupations |

## What the dataset measures

Observed usage of Claude, matched from conversation content to job tasks. It describes conversation patterns, not people, jobs, or the labor market. It supports no inference about job risk, displacement, or reassurance.

| Term | Definition |
|---|---|
| Augmentation | The person stays actively involved |
| Automation | The person directs Claude to complete the task |
| Both | Conversation styles, not job outcomes |

## Headline figures (global, release 2026-05)

| Figure | Value |
|---|---|
| Augmentation share | 51.38% |
| Automation share | 48.62% |
| Use-case split: work | 43.36% |
| Use-case split: personal | 40.2% |
| Use-case split: coursework | 16.45% |

## Category shares of global usage

| Job category | Share of global usage (%) |
|---|---|
| Computer and Mathematical | 23.8 |
| Arts, Design, Entertainment, Sports, and Media | 13.55 |
| Educational Instruction and Library | 12.79 |
| Sales and Related | 9.14 |
| Office and Administrative Support | 7.89 |
| Management | 5.9 |
| Business and Financial Operations | 5.77 |
| Life, Physical, and Social Science | 4.51 |
| Architecture and Engineering | 3.56 |
| Healthcare Practitioners and Technical | 3.31 |
| Community and Social Service | 2.57 |
| Personal Care and Service | 1.23 |
| Production | 1.04 |
| Legal | 1.02 |
| Installation, Maintenance, and Repair | 0.62 |
| Healthcare Support | 0.62 |
| Food Preparation and Serving Related | 0.48 |
| Transportation and Material Moving | 0.34 |
| Protective Service | 0.33 |
| Building and Grounds Cleaning and Maintenance | 0.13 |
| Construction and Extraction | 0.1 |
| Farming, Fishing, and Forestry | 0.04 |

## Not in this dataset

| Quantity | Status | Where the plugin gets it |
|---|---|---|
| Task success rate | Not published | Plugin default in `frameworks/references/ai-modes.md`, editable in `company-context.md` |
| Speedup multiplier | Not published | Plugin default band in `ai-modes.md`, editable in `company-context.md` |
| Per-occupation figures | Not cached here; live connector only | Query `econ_index_get_occupation_usage`, or use the global split and say so |
| Hours, salaries, headcount | Not published | User input; `salary-defaults.md` as fallback |

## Team function to job category

| Team function | Primary job category | Secondary category |
|---|---|---|
| Engineering (software, hardware, R&D) | Computer and Mathematical | Architecture and Engineering; Life, Physical, and Social Science |
| Sales / business development | Sales and Related | Business and Financial Operations |
| Finance / strategy | Business and Financial Operations | Management |
| Marketing / communications | Arts, Design, Entertainment, Sports, and Media | Sales and Related |
| Operations / people | Office and Administrative Support | Management; Business and Financial Operations |
| Product | Computer and Mathematical | Management |

Field, lab, and plant teams map to Installation, Maintenance, and Repair; Production; Construction and Extraction; or Life, Physical, and Social Science. Those categories carry small usage shares; report the share and say nothing about what it means for the work. Occupation candidates per team come from the archetype file, not from this table.

## Language rules

- "AI is used for tasks commonly done by [occupation]", never "[occupation]s use AI".
- "Conversations matched to [category] tasks", never "[category] workers are adopting AI".
- "Labor market", never "job market".
- No job-risk, displacement, or reassurance conclusions, in either direction.
- Every figure: "cached, release 2026-05", with the link above.
