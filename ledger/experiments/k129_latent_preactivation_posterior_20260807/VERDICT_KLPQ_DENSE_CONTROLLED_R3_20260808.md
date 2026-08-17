# Dense chaos-controlled KLPQ posterior integral — R3 verdict

Date: 2026-08-08. Evidence label: **component** on fixed Full8 and
Generated8 rows. No FlopScope session, physical row, production edit,
package, upload, submission, or remote action occurred.

## Decision

**Reject pure depth-31 isotropic KLPQ decisively.** Close H2 repair, LOBO,
pure-K31 query-rule optimization, and production compression. Do not spend
generic pilots on this kernel absent positive off-design continuation shape.

The primary `ml` amplitude, averaged across the two independently rotated
dense query rules, produced:

```text
canonical K129 raw MSE                    3.4621648278e-7
controlled dense KLPQ raw MSE             1.9357730678e-6
candidate / canonical ratio               5.5912215740
rows improved                             0 / 16
correction RMS                            1.2546263395e-3
actual missing-signal RMS                 5.8840163390e-4
correction / needed cosine               -0.0104774
```

The process-separated ratios were `5.5318103` on Full8 and `5.6285738` on
Generated8, with zero improved rows in each family. The direct posterior
ReLU query average, which does not use the H2 control decomposition, agreed:
its pooled ratio was `5.5876032`. The `theory` and `energy` amplitudes were
still worse, at ratios `170.4908` and `577.6861`.

## Why this is model failure, not query failure

The target-free numerical gate passed by a wide margin:

```text
pooled half squared disagreement of the two rules   4.4054967471e-10
preregistered numerical-query allowance             3.3411000000e-8
```

Each rule used four complete Haar orthogonal frames plus antipodes, or
2,048 directions; their average used 4,096 directions. Their first and
second spherical moments passed at numerical precision. Their posterior
variance means also reproduced the analytic value
`0.0230655411625428`.

Thus the failure is not sparse numerical integration of the posterior
mean. The two independent controlled rules agree closely with one another,
the direct and controlled formulations agree, yet the resulting correction
is essentially orthogonal to the needed correction and worsens every row.
The depth-31 isotropic NNGP posterior is confidently continuing the realized
finite network in the wrong off-design direction.

## Scope of the rejection

This result rejects:

- the analytic H2 integral as an estimator;
- pure-K31 dense latent-preactivation posterior quadrature;
- LOBO as a decision test for that same continuation;
- optimized 257/512-point query rules for the same failed model;
- pilot calibration as the next experiment under the preregistered gate,
  because the uncalibrated correction has no positive shape to calibrate.

It does not prove that every kernel built from generic deep observations
must fail. A genuinely different finite-network kernel would be a new
mechanism, not a small calibration of this result, and would need a fresh
capacity argument before consuming pilot rows.

The seemingly strong facts behind KLPQ remain true but insufficient:
Kerdock conditioning explains about `97.6934%` of pointwise latent variance,
the association-scheme inverse is exact and full rank, and the posterior
ReLU moment is analytic. None of those facts controls the signed integral of
the finite-width, weight-conditioned high harmonics.

## Artifacts

- preregistration:
  `PREREG_KLPQ_DENSE_CONTROLLED_R3_20260808.md`
- target-free source:
  `run_klpq_dense_controlled_r3_targetfree_20260808.py`
- post-seal scorer:
  `score_klpq_dense_controlled_r3_postseal_20260808.py`
- sealed target-free capture:
  `runtime/artifacts/klpq_dense_controlled_r3_targetfree_20260808.npz`
- sealed target-free report:
  `runtime/artifacts/klpq_dense_controlled_r3_targetfree_20260808.json`
- post-seal receipt:
  `runtime/artifacts/klpq_dense_controlled_r3_postseal_20260808.json`

The post-seal receipt pins the preregistration, both sources, target-free
capture and report hashes. It records one post-seal target opening and no
physical or remote action.
