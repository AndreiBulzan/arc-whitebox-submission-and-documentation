# Supervised fixed K146 basis weights R1 — verdict

Date: 2026-07-29.

Evidence label: **component development diagnostic** for accuracy and
**projection** for incremental count.

## Verdict

**Kill fixed supervised quadrature weights.  Do not integrate, recapture,
physically benchmark, or tune this spelling.**

The Full train-to-development selector chose the strongest preregistered
ridge, `100`, and collapsed the final refit to essentially uniform weights:

```text
minimum / uniform       0.995862
maximum / uniform       1.003753
L2 distance             0.00013938
```

Every less-shrunk development candidate was worse than the literal uniform
K146 control.  The unregularized simplex fit worsened development MSE by
`6.849%`; even ridge `10` worsened it by `0.231%`.

The predictions were sealed before either held family was opened.  Held
results were:

| held bank | corrected candidate/control | row-ratio p95 | rows improved |
|---|---:|---:|---:|
| Full200, `index mod 5 == 0` | `1.000061415` | `1.002498` | `51.0%` |
| Synthetic1024, noise-corrected | `0.999928057` | `1.002136` | `50.88%` |

The preregistered central gate was `<=0.94` independently in both families.
The negligible Synthetic decrease is paired with a Full reversal, is about
three orders of magnitude smaller than the required effect, and cannot
survive current-estimator association differences.

## What this closes

R1 used one global, nonnegative, constant-exact 146-vector over the complete
O0 arm and the current 17 O1 bases.  It had no row, family, output-coordinate,
or dynamic network feature.  It trained on 600 whole Full MLPs, selected
ridge on 200 disjoint Full MLPs, refit on those 800, then transferred to 200
held Full and 1,024 process-separated Synthetic MLPs.

This differs from the wildcard target-free cubature: that experiment learned
weights/supports against a target-free endpoint objective at much smaller K.
It also differs from the nonlinear basis decoder: that model had
row/coordinate features and a nonlinear correction.  The present convex
experiment asks the cleanest possible supervised fixed-weight question and
answers it negatively.

It is consistent with the earlier signed-preactivation global-ridge result
and the target-trained K238 support reversal: basis-label cancellations are
not stable universal biases across independent weight draws.  Reopening
requires a lawful per-MLP observable that predicts which basis errors matter;
another fixed coefficient fit is not justified.

## Scope and economics

- Existing cached per-basis final-ReLU responses only.
- No CUDA or FlopScope run.
- No physical row, estimator/package change, network action, upload, or
  submission.
- Conservative incremental count remained a **projection** of `0.25B`; no
  physical ledger was taken because accuracy failed.
- The historical response corpus is not bit-identical to the current
  compressed K146 graph.  That association caveat cannot rescue a
  cross-family result this close to exactly uniform.

## Authoritative artifacts

- `PREREG_SUPERVISED_BASIS_WEIGHTS_R1_20260729.md`
- `fit_and_seal_supervised_basis_weights_r1_20260729.py`
- `supervised_basis_weights_r1_seal_20260729.npz`
  (`8cb87770b40969e775a136c0cc1b0c748c2674c825a98216b8b61b94ead1b13a`)
- `supervised_basis_weights_r1_seal_20260729.json`
- `score_supervised_basis_weights_r1_postseal_20260729.py`
- `supervised_basis_weights_r1_postseal_score_20260729.json`
