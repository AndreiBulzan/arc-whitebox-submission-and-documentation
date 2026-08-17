# Cubic H1 parity-shrink R3 preregistration

Evidence target: **component**.  This is a deterministic post-seal follow-up
to the already captured, target-free rank-16 cubic repaired-H1 controls.  It
does not constitute broad statistical or remote evidence.

## Fixed estimator

For each control prefix `p in {4, 8, 12, 16}`, split the 129 frame endpoints
into the fixed even and odd index sets.  On each half independently, compute
the exact eta=1 GREG calibration weights already specified by the external
cubic-H1 experiment.  Let `b_e, b_o` be the uniform endpoint means and
`g_e, g_o` the calibrated endpoint means on the two halves.  Define

```
d_e = g_e - b_e
d_o = g_o - b_o
d   = (d_e + d_o) / 2
h   = (d_e - d_o) / 2
s   = max(0, 1 - dot(h,h) / max(dot(d,d), 1e-30))
prediction = full_uniform_endpoint_mean + s * d
```

The scalar is the positive-part split-half signal-energy estimator.  It is
computed independently for every MLP from predictions and exact analytic
control moments only.  It never reads a target and has no fitted constant.
No target-open scalar retuning, routing, rank selection, or per-row override
is permitted.

## Decision rule

Promote a prefix to a broad, fresh-family screen only if all are true on the
fixed 16-row pilot:

- pooled raw-MSE ratio is at most `0.91`;
- at least 10 of 16 rows improve;
- median row ratio is below `1.0`;
- maximum row ratio is at most `1.35`.

The 0.91 threshold is the approximate accuracy gain needed for the R27
remote-safe chassis to cross 1.1e-7 before accounting for the control's own
compute.  Passing this component gate therefore does not itself establish a
1.1e-7 submission candidate; the control must subsequently be costed and
validated on fresh Full and Generated rows.

