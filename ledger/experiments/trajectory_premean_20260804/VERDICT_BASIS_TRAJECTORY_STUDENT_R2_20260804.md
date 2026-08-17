# Basis-trajectory student R2 verdict

Date: 2026-08-04. Evidence label: **component**.

**Kill this exact learned trajectory spelling.** The fixed whole-network,
cross-family screen used the canonical 129-basis endpoint sequence plus the
matched L4/L8 response-projected per-basis sequences. It opened no challenge
target or Mini100 row.

```text
source -> held family       pooled ratio   rows improved   row-ratio p95
Full-A -> Generated              1.2934          17.2%             2.2268
Generated -> Full-B              1.4805          12.0%             2.7241
required                         <=0.60         >=60.0%            <=1.20
```

The failure already appears on both source-family development splits. More
epochs worsen it. This closes the two-checkpoint response-projected
per-basis MLP spelling; do not retry it with more epochs, width, or scalar
tuning. A reopening needs a new causal state, not another decoder of these
same cached sequences.

Authoritative receipt:
`basis_trajectory_student_r2_targetfree_20260804.json`.

