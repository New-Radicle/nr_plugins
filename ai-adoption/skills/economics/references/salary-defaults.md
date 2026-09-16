# Salary defaults: US national loaded-cost bands

Fallback only. The `economics` skill asks for the company's own figures first (the loaded cost of the hire being deferred, or the actual contractor spend) and uses this table only when the user has none. These are general market ranges, not sourced data. Loaded cost means base pay plus benefits, employer taxes, and overhead, roughly 1.25-1.4x base. Every row must be verified regionally before it appears in a Sponsor-facing document.

| Role | Loaded annual cost (US, wide band) | Hourly or contract band | Verify |
|---|---|---|---|
| Operations manager | $95k-$170k | fractional $80-$200/h | verify regionally |
| Technical writer or knowledge manager | $80k-$150k | $50-$120/h | verify regionally |
| People-ops coordinator | $60k-$110k | $35-$80/h | verify regionally |
| Content marketing manager | $85k-$160k | $60-$150/h | verify regionally |
| Market research analyst | $70k-$140k | $50-$130/h | verify regionally |
| Grant writer | $65k-$125k | $50-$150/h, or per proposal | verify regionally |
| Regulatory or compliance consultant | usually contracted, not hired | $150-$400/h | verify regionally |
| Bookkeeper | $50k-$95k | $40-$100/h, often a monthly retainer | verify regionally |
| Sales development representative | $65k-$120k including variable pay | $30-$70/h | verify regionally |
| Customer success manager | $80k-$150k | $50-$110/h | verify regionally |

## How the skill uses this table

1. Ask first: "What would this role cost you loaded, or what do you pay the contractor today?" Use the answer.
2. Only if there is no answer, take the band above. Use the low end for the conservative case and the midpoint for the base case. Never the high end.
3. Record the source in `company-context.md` as "general market range, verify regionally".
4. Outside the US, ask for a local figure. Do not scale these bands.
5. Loaded cost per FTE for valuing recovered hours comes from the same question, asked about the people who do the work today, not from this table.
