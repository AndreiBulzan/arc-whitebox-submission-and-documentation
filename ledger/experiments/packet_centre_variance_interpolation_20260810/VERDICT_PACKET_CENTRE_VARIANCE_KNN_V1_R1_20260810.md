# Packet centre-dependent variance interpolation V1/R1 verdict

Date: 2026-08-10

Verdict: **kill this exact anchor/KNN variance-interpolation law**.

Evidence label: **component**, target-free.  No benchmark expectation,
Mini100 target, FlopScope session, physical row, package, upload, submission,
or remote action was used.

The experiment propagated 4, 8, and 16 fixed antipodal pairs through the
exact nonzero-mean full-covariance Gaussian recurrence.  At every layer a
leave-one-anchor-out rule selected `k in {1,2,4,8}`, then the current
centre's realized conditional preactivation mean selected a geometric mean
of exact anchor variance vectors.  The complete 66,048-centre conditional
mean cloud was propagated nonlinearly under those centrewise schedules.

Mechanical checks passed:

- literal Q0 association maximum absolute error: `0`;
- all predictions and variances finite;
- the 16-pair leave-one-anchor-out variance loss improved on a shared
  geometric-mean schedule;
- no target or packet output entered neighbour selection.

The global correction did not pass:

| antipodal pairs | pooled fidelity | cosine | unbiased residual MSE |
|---:|---:|---:|---:|
| 4 | `-137.7509` | `0.01233` | `1.0480e-5` |
| 8 | `-75.4317` | `0.02961` | `5.7732e-6` |
| 16 | `-52.9679` | `0.05568` | `4.0764e-6` |

The packet-correction energy is only `7.5534e-8`.  Centre-dependent KNN
interpolation approximately halves the residual of the prior 16-anchor
shared schedule (`8.426e-6`), but it remains about 54 times the entire useful
correction and has negligible signed alignment.  Neither family is close to
a gate.

This closes nearest-neighbour interpolation from exact anchor premeans to
exact anchor marginal variances at score-compatible anchor counts.  Do not
tune distance metrics, neighbour counts, softmax temperatures, or add a few
more anchors: the cancellation precision gap is orders of magnitude.  The
result does not prove that an analytic exact collective boundary contraction
or a radically different centre-dependent representation is impossible.

Artifacts:

- `runtime/artifacts/packet_centre_variance_knn_v1_r1_targetfree_20260810.json`
- `runtime/artifacts/packet_centre_variance_knn_v1_r1_targetfree_20260810.npz`

