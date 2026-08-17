# Preregistration: regularized polar complete frame R2

Date: 2026-08-04

Evidence sought: target-free **component** selection on independent Full100 /
Generated128 complete-frame banks, followed by a sealed **broad statistical**
official-Mini100 blocking score. No FlopScope, package, or remote action is
authorized.

## Reopening boundary

R1's ordinary polar barycenter was novel and retained `7.42%` Mini100 raw
gain, but failed its stability gate. Its selected polar input was nearly
singular (median minimum singular value `0.001406`), explaining strong
pilot-to-Mini drift. R2 changes the controlling assumption directly: it
regularizes the weak polar subspace toward the exact q0 frame. This is a
concrete mechanism invalidating R1's instability diagnosis, not another
Mini100 coefficient fit.

The capsule was searched again for regularized polar factors, q0-anchored
orthogonal means, geodesic shrinkage, and nullspace-stabilized frame averages.
No prior attempt exists. Partial-frame, selector, static-weight, and
first-layer harmonic negatives do not construct a complete orthogonal frame
and do not control this test.

## Fixed rule

Let `Q0`, `Qr`, and `Qd2` denote the complete q0, right-Gram, and depth-2
`J J^T` frames. For a fixed anchor weight `a`,

```text
Q(a) = polar(a Q0 + (1-a)/2 Qr + (1-a)/2 Qd2).
```

The only frozen candidates are `a in {1/3, 0.45, 0.60}`. Predictions for all
three are first captured on the existing Full100/Generated128 row manifests.
Selection reads no challenge target: choose the `a` minimizing the worst
family MSE to the fixed equal four-complete-frame teacher. Freeze that one
value and its hashes. Only then capture one Mini100 prediction and score it.

## Gates

Remote R31 to `1.1e-7` allows total score ratio `0.95013`. At the R1 central
raw ratio `0.92582`, at most about `2.6%` extra effective compute is available.
R2 earns exact deployment pricing only if official Mini100 has:

```text
raw ratio to q0                    <= 0.92
rows improved                     >= 60 / 100
paired-bootstrap ratio upper 95%   < 1.00
```

If it passes, the later physical gate must prove total frame-construction and
trajectory overhead `<=3.0B` versus R78 before any `<=1.1e-7` projection is
promoted. Failure kills this exact anchored-polar grid; no coefficient may be
chosen from Mini100.

