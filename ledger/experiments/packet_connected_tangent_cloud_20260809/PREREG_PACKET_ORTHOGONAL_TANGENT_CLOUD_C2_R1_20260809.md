# Packet orthogonal-tangent cloud C2/R1 preregistration

Date frozen: 2026-08-09

Evidence class: target-free **component** oracle.

## Reopen justification

C1's independent-sign tangent cloud failed, but its largest cell still had
about `1.1%` relative error in the analytically known layer-one variance and
only `0.056` layer-one correction fidelity.  The new observable removes that
error exactly; it is not a rank/support sweep.

For every tangent or boundary-birth ensemble, arrange the samples into full
`256 x 256` Sylvester-Hadamard blocks.  Random row permutations and column
signs preserve exact block covariance while decorrelating the blocks from
the fixed Kerdock support.  At layer one, bypass estimation entirely and use

```text
v_1 = tau^2 * diag(W_1^T W_1).
```

Later layers use the same centre-conditioned soft-gate recurrence and exact
aggregate square-root marginal balancing as C1, but all fresh orthogonal
births have exact unweighted covariance.

This invalidates C1's controlling finite-random-covariance assumption.  It
does not invalidate the failures of block closure, shared dense covariance,
or total-second-moment subtraction; the centre-specific gate/tangent
interaction remains present.

## Frozen evidence and configurations

Reuse C1's sealed packet population, exact metrics, support grid
`k=1,2,4,8`, tangent ranks `r=1,2,4`, four support/noise replicates, and all
Full8/Generated8 rows.  The only changed operations are orthogonal sign
generation and analytic layer-one variance.

The pass gates are identical to C1: a primary `k<=4,r<=2` or fallback cell
must have median pooled fidelity `>=0.70`, cosine `>=0.85`, both family
fidelities `>=0.60`, at least `3/4` replicates above `0.50`, and layer-eight
fidelity `>=0.60`, with exact Q0 association and finite values.

If no cell passes, close this connected-tangent/shared-marginal recurrence.
Do not add probes, bases, shrinkage, or a learned calibration.  Passing only
licenses post-seal target association and meter-aware design.

