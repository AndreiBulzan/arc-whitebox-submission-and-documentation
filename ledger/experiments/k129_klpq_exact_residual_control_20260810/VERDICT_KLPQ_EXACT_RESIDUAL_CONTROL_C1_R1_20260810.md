# KLPQ exact-residual control C1/R1 — killed

Date: 2026-08-10

Evidence label: **component** for the fixed 8 Full plus 8 Generated rows.  No
FlopScope session, physical row, official-Mini100 run, package, upload, or
remote action occurred.

## Result

The preregistered estimator was

```text
I_hat = I_dense(KLPQ surrogate) + Q_exact[f - surrogate].
```

The target-free capture used two independent frozen orthogonal-antipodal
query rules.  Their averaged residual corrections were much noisier than the
integral defect:

| complete frames per rule | exact rows per rule | pooled raw ratio vs q0 | rows improved |
|---:|---:|---:|---:|
| 1 | 512 | 98.4107 | 0/16 |
| 2 | 1,024 | 39.9440 | 0/16 |
| 4 | 2,048 | 22.7025 | 0/16 |

At four frames, the two-rule disagreement MSE remains
`1.3571469691e-5`, whereas the baseline integral MSE is only of order
`3e-7`.  Direct query averaging and the KLPQ-controlled query average are
nearly identical, demonstrating that KLPQ does not remove the pointwise
finite-network query variance that dominates this construction.

Even an unavailable target-dependent scalar applied to the four-frame
correction reduces pooled raw MSE by only about `2.1%`; Full prefers a scalar
near zero.  Therefore scaling this query count cannot reach the 35--45%
preregistered accuracy gate and cannot be score-positive at its projected
compute.

## Decision

**Kill sparse exact KLPQ residual control.**  Do not purchase more exact
off-design rows for this mechanism.  The result does not invalidate the
latent-GP identities; it shows that their surrogate leaves essentially all
of the sparse-query finite-network variance in the residual.

Authoritative artifacts:

- `PREREG_KLPQ_EXACT_RESIDUAL_CONTROL_C1_R1_20260810.md`
- `runtime/artifacts/klpq_exact_residual_control_c1_r1_targetfree_20260810.npz`
- `runtime/artifacts/klpq_exact_residual_control_c1_r1_targetfree_20260810.json`
- `runtime/artifacts/klpq_exact_residual_control_c1_r1_postseal_20260810.json`

