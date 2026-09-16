# Program structure by size mode

| Mode | Headcount | Sponsor and Rollout Lead | Length | What changes |
|---|---|---|---|---|
| Founder | 1-9 | Same person | 6 weeks | No workshop, no channel. One-page playbook. Weekly self check-in. Primitives capped at 15. Three-metric scorecard. |
| Team | 10-50 | Different people | 12 weeks | The full program. |
| Department | 50+ | Department head as Sponsor; a lead inside the department as Rollout Lead | 12 weeks per department, staggered | Runs inside one function first. Company-context has a department scope. Cross-department dependencies are logged, not solved. |

Setup infers the mode from headcount and confirms it. A company can move modes; a Founder-mode company that passes 10 people is prompted to upgrade at the next check-in.

## Phases and weeks

| Week | Founder (6 wk) | Team (12 wk) | Department (12 wk) |
|---|---|---|---|
| 0 | Setup, assess, map, prioritize in one session | Foundation: align Sponsor, baseline survey, licenses, channel, 1:1s, scorecard baseline | Same, scoped to the department; company-level Sponsor informed |
| 1-2 | Founder's own three workflows; daily use | First Win: kickoff message from Sponsor, hands-on workshop, everyone posts one win by day 10 | First Win inside the department |
| 3-4 | Automate the top grant or commercial primitive | Workflow Layer: AI-first process per team, role playbooks, first connectors | Same; cross-department dependencies logged |
| 5-6 | Self check-in; playbook revised; impact note for investors or board | Depth: advanced tools for technical roles, first automations | Depth |
| 7-8 | (program ended) | Office hours (Week 7), mid-point survey (Week 8) | Same; department head reports to company Sponsor |
| 9-12 | (program ended) | Institutional Layer: AI handbook (9), use-case library (10), impact report (11), plan next (12) | Same; next department queued |

## Who owns what

| Role | Owns | Time |
|---|---|---|
| Sponsor | Vision, budget, licenses, kickoff message, presenting the impact report, answering autonomy-threat questions | 1-2 hours per week in Weeks 0-2, then 30 minutes |
| Rollout Lead | Everything else: cadence, workshop, playbooks, check-ins, scorecard, coaching, escalation | 3-5 hours per week during the program, 2-3 after |
| Team AI leads (optional, Team and Department modes) | One per function; first point of help; collects use cases | 1 hour per week |

## The weekly loop

Every week from Week 1: collect (use cases, hours, blockers) -> update scorecard -> diagnose (failure mode, persona pattern) -> act (one intervention) -> digest (draft, Rollout Lead approves, post). The `weekly-checkin` skill runs this loop and is safe to run late or twice.
