# Packet connected-tangent cloud C1/R1 verdict

Date: 2026-08-09

Verdict: **kill the independent-sign connected-tangent bridge**.

Evidence label: **component**, target-free.  No benchmark expectation,
FlopScope session, physical row, package, upload, submission, or remote action
was used.

The complete frozen Full8/Generated8 capture failed every gate.  The best
cell, `k8_r4`, had median final correction fidelity `-42.4885` and cosine
`0.1667`; the smallest cell was worse.  Q0 association was exact to
`2.22e-16`, all outputs were finite, and the square-root marginal balance
error was only `3.50e-10`, so this is not an association or numerical-stability
failure.

Layer localization exposed a narrower defect.  At `k8_r4`, independent
Rademacher tangent sampling had about `1.1%` RMS relative error in the known
analytic first-layer conditional variance.  The packet correction is much
smaller than that variance error: layer-one fidelity was only `0.0564`, then
fell monotonically to the final failure.  Thus C1 does not yet distinguish
finite tangent-quadrature noise from a failure of the centre-conditioned
Wick covariance recurrence.

One and only one reopen is licensed: complete orthogonal tangent blocks plus
the exact analytic first-layer variance.  This invalidates C1's controlling
random-covariance assumption without increasing the support/rank grid.  If
that variant fails early, close the tangent-cloud family rather than adding
more random probes.

Frozen artifacts:

- `runtime/artifacts/packet_connected_tangent_cloud_c1_r1_targetfree_20260809.npz`
- `runtime/artifacts/packet_connected_tangent_cloud_c1_r1_targetfree_20260809.json`

