# R96: equal ensemble of the sealed R92 and R94 readouts

Date: 2026-08-10

## Mechanism

R92 and R94 propagate the same 129 Kerdock bases through the same exact
network graph.  They differ only at the final readout:

- R92 uses the frozen two-fold held-basis correction;
- R94 uses the frozen global `lambda=0.019` correction and independently
  selected final scale.

The candidate is fixed without target access as

```text
prediction = 0.5 * prediction_R92 + 0.5 * prediction_R94.
```

A production implementation may share every trajectory and compute the two
final ReLU means only at the readout.  Its incremental cost must be measured;
it is expected to be small compared with the 129-basis propagation.

## Blocking prior-art preflight

Capsule searches covered `output ensemble`, `readout blend`, `R92`, `R94`,
`BF-XCORR`, `lambda019`, and `moment anchor`.  The nearest negative results
are the output-moment anchor and older multi-arm blends.  Those either add an
external analytic target or combine different propagated arms.  No artifact
combines these two already-sealed, same-trajectory nonlinear readouts.

Outcome: **materially new composition, not a new estimator family**.  Its
ceiling is deliberately fractional, approximately `0--1%` raw.  It cannot
be the requested breakthrough by itself.

## Frozen replay and gates

Use the exact target-free official-Mini100 captures for R92 and R94.  Targets
may be opened only after hashes and the `0.5/0.5` rule are sealed.

Proceed to implementation only if all hold:

1. public, holdout, and pooled raw ratios are below `1` versus the better
   constituent on that split;
2. pooled raw reduction versus the better constituent is at least `0.10%`;
3. at least `45/100` rows improve versus the better constituent;
4. the projected incremental readout cost does not erase the gain.

No coefficient grid, per-row selection, or post-target adjustment is
permitted.  No package, upload, submission, or remote action is authorized.

