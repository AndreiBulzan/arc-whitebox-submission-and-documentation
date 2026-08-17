# Live R40 addition-reduction R1 verdict

Evidence label: **component** (target-free exact algebra; no MLP, physical
benchmark, package, upload, or submission).

The live scaled ternary `<6,3,3:40>` tensor was exported from the frozen
production schedules and accepted by `ternary_addition_reducer` at pinned
commit `e59693512f095a4704521f1185c592445af9e058`.  The adapter transposes
each production output matrix into the reducer's `(n3,n1)` coordinate order;
the local validator was changed only to accept the live tensor's uniform
scale factor 8.  Addition reduction itself is factor-local and therefore
preserves the represented linear forms exactly.

The reducer's best result with 32 reducers, eight threads, seed `8202401`,
and 12 no-improvement iterations was:

```text
                 A/U   B/V   C/W   total
production SLP    108    54   132     294
reducer best      117    64   143     324
```

The production straight-line programs are strictly better on every factor,
including the expensive activation sides (`A+C = 240` versus `260`).  This
lane cannot reduce the live circuit and is killed.  The result does not rule
out a genuinely different rank-40 bilinear decomposition; it rules out
re-synthesizing the incumbent factors with this public reducer.
