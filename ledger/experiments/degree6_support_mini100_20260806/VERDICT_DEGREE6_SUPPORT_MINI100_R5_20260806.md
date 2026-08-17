# Degree-6 support Mini100 R5 verdict

Date: 2026-08-06.  Decision: **kill**.

The source audit after capture found that the apparent incumbent mismatch was
structural, not merely floating-point association: current R31/R87 is the
complete O0-only `K=129` statistic, whereas the 2026-07-29 degree-6 support
experiment is a `K=146` statistic consisting of O0 K129 plus 17 O1 bases.

On official Mini100:

```text
exact R31 K129 raw                         2.565540597e-7
old replay K146 incumbent raw              2.414328741e-7
K146 degree-6 literal raw                  2.271646467e-7
degree-6 / exact K129 raw ratio                    0.885446
pure sample-count factor 129 / 146                 0.883562
ratio after removing sample-count factor           1.002132
```

Thus virtually the entire attractive raw-MSE reduction is the expected cost
of propagating 17 additional bases.  It is not a free support replacement on
R87 and it slightly worsens error per basis after the simplest normalization.
The paired delta also has the wrong association with exact R31 and may not be
used as a current-chassis correction.

This closes porting the fixed degree-6 support into R87.  The capture remains
useful as a warning that `K146` and `K129` baselines must never be conflated.

Authoritative receipts:

- `runtime/artifacts/k129_degree6_support_mini100_r5_targetfree_20260806.json`
- `runtime/artifacts/k129_degree6_support_mini100_r5_postseal_20260806.json`

