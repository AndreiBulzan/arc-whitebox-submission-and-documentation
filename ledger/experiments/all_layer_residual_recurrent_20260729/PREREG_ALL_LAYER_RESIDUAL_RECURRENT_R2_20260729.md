# All-layer residual recurrent R2 — centered-gate correction

Date: 2026-07-29

Evidence sought: **component**. This is one narrow offline rerun, not a
FlopScope run, physical receipt, candidate, package, upload, submission, or
remote action.

R1 accidentally treated capture feature 5 as `active_fraction`, although
the frozen capture stores `active_fraction - 0.5`. R2 changes exactly:

```text
gate = clamp(feature[..., 5] + 0.5, 0, 1)
```

Everything else is identical to R1: source capture, `40/8/8` Full
train/dev/held split, Generated `0..7` transfer guard, seed, 240 steps,
batching, optimizer, cell, messages, checkpoint objective, and held gates.
Held predictions are written before the separate scorer opens held labels.

Hard stop after this score regardless of outcome. Promotion still requires
both final ratios `<=0.70`, both late-8 ratios `<=0.75`, at least half of
rows improved in each family, and finite predictions.

