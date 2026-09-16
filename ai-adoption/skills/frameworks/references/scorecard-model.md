# Scorecard model

Four categories. Every metric has a baseline (Week 0), a target (end of program), and a stretch (three months after). Status is computed, not chosen.

## Team mode (full scorecard)

| Category | Metric | Baseline | Target | Stretch |
|---|---|---|---|---|
| Adoption | % of team using AI weekly | measured | 80% | 95% |
| Adoption | AI sessions per person per week | measured | 8 | 15 |
| Adoption | Documented use cases | 0 | 2 per person | 5 per person |
| Adoption | AI-first workflows established | 0 | 1 per team | 2-3 per team |
| Impact | Hours saved per person per week | 0 | 3-5 | 6-8 |
| Impact | Time from idea to published document | measured | quartered | eighth |
| Impact | Two archetype-specific workflow times (set in setup) | measured | set | set |
| Culture | AI satisfaction (1-5) | survey | 4.0+ | 4.3+ |
| Culture | % who say AI makes their job better | survey | 70% | 85% |
| Culture | Channel posts per week | 0 | 5+ | 10+ |
| Culture | Persona migrations (count only) | 0 | 1-2 | 3+ |
| Team | Communication Architecture score | rubric | +0.5 | +0.9 |
| Team | Enabling Structure score | rubric | +0.5 | +0.9 |
| Team | Composite | rubric | +0.25 | +0.5 |

Targets are defaults. Setup scales them to headcount and archetype; the Sponsor confirms.

## Founder mode (three metrics)

| Metric | Baseline | Target (Week 6) |
|---|---|---|
| Hours saved per week | 0 | 5+ |
| Workflows moved to AI-first | 0 | 3 |
| Documented wins in `wins.md` | 0 | 12 |

## Status rules

Applied weekly by `scorecard` and `weekly-checkin`, per metric, from actual versus the straight-line path from baseline to target:

| Status | Rule |
|---|---|
| Exceeded | actual at or beyond target |
| On Track | actual at or above 85% of the expected value this week |
| At Risk | 60-85% of expected |
| Behind | below 60%, or no actual recorded for two consecutive weeks |

Program health is the worst status among Adoption metrics, because adoption drives everything else.

## Persona breakdown

Adoption metrics carry an optional persona breakdown as counts (for example `{"Conformist": 6, "Skeptic": 2}`). Never names. Used to detect uneven adoption: if any persona group has 0% weekly use by Week 6, the check-in flags it.
