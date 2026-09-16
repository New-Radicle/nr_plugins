# Seven failure modes

Program-level patterns. Diagnose which one is active before prescribing a fix.

| # | Failure mode | What it looks like | High risk when | Fix |
|---|---|---|---|---|
| 1 | Crowdsource Trap | Everyone experiments; nothing is chosen; no strategy | Larger teams, no top-down tool selection | Sponsor picks the tools and the first three workflows. Say no to the rest for 12 weeks. |
| 2 | Pilot Purgatory | A proof of concept works and never scales | "Innovation theater" culture; no owner past the demo | Every pilot gets a 4-week deadline: scale or kill. |
| 3 | Skills Gap Abyss | Tools are deployed and unused | Technical team that has never used AI for their own specific work | Role-specific playbooks. Hands-on workshop, not a lecture. |
| 4 | Integration Wall | Tools do not connect to the systems people already use | Many legacy tools with no API or connector | Prefer tools with connectors; accept manual bridging in the first month. |
| 5 | Trust Deficit | Over-checking every output, or over-trusting it | Safety-critical, regulated, or precision work | Start low-risk. Human-in-the-loop by design. Track accuracy in a shared error log. |
| 6 | Culture Collision | AI is read as an insult to expertise | Craft-proud professionals, deep specialists | Frame AI as removing the administrative tax. Experts spend more time on expert work. |
| 7 | Measurement Void | No baseline, no tracking, no way to show value | Move-fast cultures that skip measurement | Baseline every scorecard metric in Week 0. Weekly check-in is not optional. |

## Diagnostic shortcuts

- One person resisting: persona-level coaching (see `personas.md`).
- Several people resisting the same way: Culture Collision or Trust Deficit.
- Signed up, tried once, stopped: Skills Gap Abyss.
- Channel quiet, usage high: underground pattern (see `personas.md`).
- Usage flat since Week 4 with no documented use cases: Measurement Void plus Skills Gap Abyss.
- Leaders unavailable for two consecutive weeks: sponsor bottleneck; the Rollout Lead escalates once, then proceeds on what does not need the Sponsor.
