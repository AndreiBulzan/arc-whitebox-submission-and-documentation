# Grouped gate-source basis G0/R1 verdict

Status: **complete-basis random sampling has no budget survivor; other source
partitions remain open**.

Evidence label: **component** on Full 640--641 and Generated 88--89.  Costs
are projections.

The exact atom identity closed to `1.33e-15`.  The ideal packet target reduced
pooled raw MSE from `2.7121e-7` to `1.1380e-7`.  But sampling complete bases
adds too much variance:

| bases | uniform iid | oracle norm iid | uniform without replacement |
|---:|---:|---:|---:|
| 4 | `1.830e-6` | `1.515e-6` | `1.787e-6` |
| 8 | `9.148e-7` | `7.575e-7` | `8.647e-7` |
| 32 | `2.287e-7` | `1.894e-7` | `1.733e-7` |

The broad allowance for 35% reduction is only about `6.25e-8`.  Replicate
halves agree; basis-norm Spearman correlations are `0.923--0.940`.

This rejects iid/without-replacement complete-basis sampling at 4--8 bases.
It does not reject layer-vector, centre-vector, correlated, or deterministic
collective sources.

Receipts:

- `runtime/artifacts/grouped_gate_source_basis_g0_r1_targetfree_20260808.json`
- `runtime/artifacts/grouped_gate_source_basis_g0_r1_postseal_20260808.json`

