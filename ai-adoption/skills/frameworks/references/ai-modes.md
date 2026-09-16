# Classifying primitives

An operational primitive is a distinct, repeatable, observable, assignable unit of work. Map the function, not the person; one person can own primitives in three functions. Functions that should exist and have no headcount are the highest-value finds, because AI can fill them until a hire is justified.

## Status vocabulary

| Status | Meaning |
|---|---|
| Strong | Established, performing well |
| Active | Happening regularly with a clear owner |
| Moderate | Functional, room to improve |
| Growing | Emerging, not yet consistent |
| Early | Just started, minimal track record |
| Ad hoc | Reactive, no cadence |
| Weak | Should happen; quality or consistency is poor |
| Absent | Needed and not happening |
| Unknown | Cannot determine yet |

## AI modes

Digital work uses two modes. Physical, field, and lab work uses four. A primitive gets exactly one.

| Mode | Definition | Speedup band (plugin default) | Examples |
|---|---|---|---|
| Automation | AI does the work; a person reviews | 5-10x | Competitive scans, first drafts, report assembly, data formatting, scheduling |
| Augmentation | A person does the work; AI assists | 2-4x on the addressable share | Technical docs from notes, strategy synthesis, grant narrative, financial model updates |
| Edge | The work is physical; AI helps before and after | 2-4x on prep and write-up | Pre-job briefings, post-job reports, parts lookup, checklist generation |
| Capture | AI structures what a person observes in the field | 2-4x | Voice-to-log maintenance notes, photo-to-incident report, lab notebook structuring |
| Analyze | AI interprets physical data for a human decision | 3-6x | Performance trend analysis, failure pattern detection, calibration drift |
| Excluded | No AI role; recorded so the estimate is honest | 1x | Soldering, installation, driving, wet lab execution, in-person relationship work |

Speedup bands are the plugin's assumptions, not measurements from any external dataset. They are editable in `company-context.md` and should be replaced by the company's own use-case data after two weeks of check-ins.

## Default success rates

| Mode | Default | Adjust when |
|---|---|---|
| Automation, Augmentation, Edge, Analyze | 65% of first outputs usable with light edits | After ten documented use cases in the primitive |
| Capture | 55%; depends on input quality | After the first two weeks of field use |

## Priority quadrants

| Quadrant | Rule | Action |
|---|---|---|
| Quick Win | High speedup and status Weak, Absent, or Ad hoc | Do first |
| Strategic Bet | High speedup and status Active or Strong | Invest; big gain on something already working |
| Foundation | Moderate speedup and status Weak or Absent | Build the base even if AI impact is modest |
| Future | Low speedup, or already Strong, or Excluded | Leave for now |

High speedup means Automation or Analyze. Moderate means Augmentation, Edge, Capture.

## Three-factor ranking

For the top-10 list, score each primitive 1-5 on gap severity (Absent or Weak = 5, Strong = 1), AI addressability (Automation = 5, Excluded = 0), and economic weight (hours per week at stake times the loaded cost of the people doing it). Sum, then break ties toward the archetype's emphasis.
