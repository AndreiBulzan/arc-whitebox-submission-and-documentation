# Quartic-balanced partial frames R4 preregistration

Date: 2026-08-04

Evidence scope: target-free **component** capacity screen on Full rows `7,17`
and Generated rows `0,1`.  No challenge target, FlopScope session, physical
row, package, upload, or submission is permitted.

## Mathematical distinction

One complete K129 Kerdock frame is an antipodal spherical 5-design.  A
partial collection of complete bases remains exact through degree 3 but
introduces a degree-4 harmonic defect, which can swamp the degree-6
orientation complementarity responsible for the four-frame gain.

For a complete orthonormal basis `B`, define

```text
A_B = mean_{b in B} b**tensor(4).
```

Its centered Gram kernel is available without constructing a fourth-order
tensor:

```text
K(B,C) = <A_B-S4, A_C-S4>
       = mean_{b in B,c in C} (b dot c)^4 - 3/[d(d+2)].
```

R4 pools all `3*129` bases from right, d2, and d2right, and greedily chooses
48 equal-weight bases minimizing the exact quartic defect.  It tries only
the three symmetry-distinct zero-basis anchors and chooses by final kernel
norm, never by an endpoint or target.  The estimate is

```text
q0_complete + (3/4) * mean_selected(endpoint_alt - q0_complete).
```

This differs from prior endpoint herding, degree-6 adjoint support, balanced
orientation quotas, D6 proxy regression, and arbitrary/bit-reversal
prefixes.  It directly removes the leading harmonic damage caused by taking
only a bit of each complete frame.

## Gate

This is initially a dynamic-selector capacity bound, not yet a deployable
runtime claim.  Promote only if teacher-reconstruction MSE relative to q0 is
at most `0.75` on every one of the four rows and the mean ratio is at most
`0.60` in both families.  Otherwise quartic balancing alone is insufficient
and the lane stops.

