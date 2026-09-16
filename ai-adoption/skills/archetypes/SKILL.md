---
name: archetypes
description: >
  Reference libraries for the five company archetypes the AI adoption program
  supports: A grant-stage hardware R&D, B venture-backed scaling hardware,
  C infrastructure or project developer, D climate or vertical software,
  E nonprofit, incubator, or services organization. Each library lists teams,
  starter primitives with default AI modes, program emphasis, persona priors,
  default risks, and candidate occupations for Economic Index mapping. Load
  when setup asks "which of these is closest to you", when map-primitives
  needs a starter list, or when a user asks "what primitives does a company
  like ours have".
---

# Archetypes

One question at setup: "Which of these is closest to you?" The answer loads a starter primitives library, persona priors, default risks, and a rollout emphasis. Starter lists are prompts for the intake conversation, not the answer; the company's real primitives come from what people actually do.

| Code | Archetype | Typical size | File |
|---|---|---|---|
| A | Grant-stage hardware R&D | 1-25 | `references/a-hardware-rnd.md` |
| B | Venture-backed scaling hardware | 25-150 | `references/b-scaling-hardware.md` |
| C | Infrastructure or project developer | 50+ | `references/c-project-developer.md` |
| D | Climate or vertical software | 5-50 | `references/d-software.md` |
| E | Nonprofit, incubator, or services organization | any | `references/e-nonprofit-services.md` |
| add-on | Physical-work primitives (added to any archetype with field, lab, or plant work) | any | `references/physical-primitives.md` |

## Mixed companies

Pick a primary archetype and add teams from another. A company with a services arm and a hardware arm loads A or B plus the Programs team from E. Record the additions in `company-context.md` under "Added teams from other archetypes".

## How to use a library

1. Present the teams and ask which exist, which are missing, and which one person covers several of.
2. Walk the starter primitives per team; keep, rename, drop, add. Set status from what the user says, never from the default.
3. Default AI mode is a starting point. Reclassify any primitive the user describes as physical using the four physical modes.
4. Carry the emphasis, persona prior, and default risks into `company-context.md` and the risk matrix.
5. Occupation candidates are names to query in the Economic Index connector when it is available. They are guesses until the query confirms a published occupation.
