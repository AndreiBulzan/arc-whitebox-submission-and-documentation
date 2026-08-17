# Deterministic basis balancing D1/R1 verdict

Status: **equal-weight complete-basis selection through 8 bases is rejected;
larger output-oracle subsets demonstrate only expensive capacity**.

Evidence label: target-free output-oracle selection plus post-seal
**component** scoring on four development rows.  Costs are projections.

Per-network output-oracle results from the full 32-replicate atoms:

| bases | projected cost | pooled raw reduction |
|---:|---:|---:|
| 4 | `8.6B` | `-76.0%` |
| 8 | `17.2B` | `8.13%` |
| 16 | `34.4B` | `36.64%` |
| 32 | `68.8B` | `54.25%` |

At 16 bases the Generated-family reduction is only `20.10%`; at 32 it is
`39.26%`.  The universal four-network support reaches `37.56%` only at 32
bases.  Supports learned on replicate halves show the same cost scale.

This rejects equal-weight deterministic complete-basis selection at the
4--8 basis budget.  It does not reject unequal weights, layer grouping,
correlated estimators, or other partitions.

Receipts:

- `runtime/artifacts/deterministic_basis_balancing_d1_r1_targetfree_20260808.json`
- `runtime/artifacts/deterministic_basis_balancing_d1_r1_postseal_20260808.json`

