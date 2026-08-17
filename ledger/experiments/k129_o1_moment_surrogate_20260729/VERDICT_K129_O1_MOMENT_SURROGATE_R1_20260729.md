# K129 O1 moment surrogate R1 — killed

Date: 2026-07-29.

Evidence label: **component** for reconstruction and development scoring;
**projection** for incremental count. No GPU acquisition, FlopScope session,
physical row, estimator edit, package, upload, or remote action occurred.

## Bottom line

The valuable O1 Edgeworth signal is real, but it is not recoverable from the
already-paid O0 marginal moments, `q0`, and analytic proxy by a shared
permutation-equivariant ridge.

With the fixed composition

```text
1.000025 * (q0(lambda=.0075) + .0735 * (E1hat - q0))
```

the strongest target-free reconstruction reverses on Generated64:

| candidate | Full100 raw ratio | Generated64 corrected raw ratio |
|---|---:|---:|
| O0 Edgeworth identity | `0.999620` | `1.000315` |
| reconstructed O1 | `0.992699` | `1.004318` |
| exact O1 reference | `0.941621` | `0.928301` |

The required gate was `<=0.97` on both families. It fails decisively.

## What was tested

First, the disjoint 275-row Full extension fitted only the O1-minus-O0
Edgeworth readout from O0 raw moments and simple shared local/pooled
features. Its reconstruction MSE ratios were:

```text
held extension   1.007584
Full100          1.004633
Generated64      1.004184
```

Thus the O0 marginal moments alone contain no transferable orientation
delta.

A stronger screen then added the already-paid `q0(lambda=.0075)` and exact
analytic q4 proxy. Ridge strength was selected only by O1 reconstruction,
never by challenge targets. It reduced O1-delta reconstruction MSE to
`0.983816` on Generated64, but the recovered component is not the
target-aligned component: its challenge score worsens by `0.432%`.

Directly regressing the four O1 raw-moment deltas was still less stable:
small raw-moment errors are magnified by the Edgeworth formula and produced
multi-fold readout reconstruction error. That variant was rejected before
target scoring.

## Economics and decision

Forming the four O0 raw moments from the existing 66,048-row final cloud has
a conservative count projection of `0.118608B`, below the requested `0.5B`
count ceiling. Residual/effective cost was not physically measured because
the accuracy gate failed first.

Do not implement this surrogate. The `5.84%`/`7.17%` exact-O1 raw gains show
that the live O1 Edgeworth mechanism deserves attention, but its
orientation-specific innovation must be measured through an actual O1
trajectory or a new state observable; O0 marginal recalibration cannot
replace it.

## Reproduction

- `screen_k129_o1_moment_surrogate_r1_20260729.py`
- `k129_o1_moment_surrogate_r1_20260729.json`
