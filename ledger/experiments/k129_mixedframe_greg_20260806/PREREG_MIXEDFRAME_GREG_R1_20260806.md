# Mixed-frame exact-moment GREG R1

Date: 2026-08-06

Evidence sought: target-free **component** capture followed by **broad
statistical** scoring on the already-open Full100 and Generated128
development banks.  This experiment authorizes no FlopScope run, package,
upload, or submission.

## Prior-work boundary

Repository searches covered TAP/cavity, harmonic controls, geodesic and
Schur frames, phase interlacing, moment restoration, GREG, Hermite controls,
and mixed-frame control variates.

The following nearby spellings are closed and are not repeated:

- one-site and query-conditioned cavity/TAP closures;
- fixed harmonic decoders and degree-6 support controls;
- whole-basis phase interlaces selected by generic fourth-moment defect;
- signed cross-frame designs, partial-frame coresets, and endpoint MLMC;
- exact-moment GREG reweighting *within the canonical q0 frame*.

No prior artifact combines a total-K129 multi-frame Latin support with an
explicit exact-moment GREG correction.  The distinction is causal: earlier
interlaces tried to avoid the low-degree defect introduced by mixing frames;
R1 permits broader angular coverage and then explicitly calibrates a known
second-layer moment.

## Frozen construction

For every MLP use the four already sealed complete frames in this order:

1. q0;
2. right-Gram;
3. depth-two Jacobian;
4. depth-two right-Jacobian.

Basis `b` uses frame `b mod 4`, giving exactly 129 propagated bases total.
No offset, support, frame, or assignment is selected per network.

For every selected basis compute the 256 diagonal second moments of
`z2 = ReLU(W0 x) W1`.  Its exact spherical expectation is obtained from the
arc-cosine kernel of the realized `W0` and contraction through `W1`.
Apply the already frozen q0-GREG spelling:

```text
ridge ratio = 0.01
shrink      = 0.25
positivity-preserving step
sum weights = 1
```

Apply those weights to the selected final per-basis endpoints.  Record q0,
the unweighted Latin mix, the GREG mix, and the four-complete-frame teacher.
All predictions and weights must be sealed before targets are opened.

The endpoint assembly is a capacity proxy: cached per-basis endpoints were
captured inside their complete parent frames, so a passing result would
license a literal mixed-cloud capture; it would not itself license an
estimator implementation.

## Gate

Promote only if the unchanged GREG mix:

- has raw-MSE ratio at most `0.85` to q0 in both Full100 and Generated128;
- strictly improves over the unweighted Latin mix in both families;
- improves at least 60% of rows in each family; and
- has row-ratio p95 at most `1.25` in each family.

Failure kills mixed-frame exact-`z2` GREG.  Do not scan offsets, ridges,
shrinks, alternate moments, or target-fitted coefficients after failure.
