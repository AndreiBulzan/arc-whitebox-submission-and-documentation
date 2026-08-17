# Preregistration: dual next-gate probe balancing D2/R1

Date: 2026-08-09

## Evidence-driven change from D1

D1 matched the complete q0/polar checkpoint mean to roughly `2e-5` relative
RMS and retained `69--79%` of the dual gain on Full4, but only `-26--45%` on
Generated4.  This rejects raw checkpoint moments as a sufficient statistic;
it does not reject selecting existing particles from the two-frame union.

D2 observes the first nonlinear operation after the merge.  For selected
actual output columns of the supplied next-layer weight matrix, it computes

```text
relu(h_q @ W_probe) - relu(h_p @ W_probe)
```

for every aligned row and balances those differences jointly with the
checkpoint-state difference.  It then selects one existing state per aligned
row and evaluates one ordinary suffix.  No predicted final output, benchmark
target, fitted model, or unavailable second suffix enters the selection.

This is distinct from a thin shadow network: it does not roll a compressed
state forward or decode an output.  It uses one exact partial next-layer gate
observation solely to preserve the union measure before the real suffix.

## Frozen disjoint pilot

- Full rows: `47, 57, 67, 77`.
- Generated rows: `4, 5, 6, 7`.
- Checkpoint: post-ReLU layer `16` only, the score-compatible surface.
- Antipodal constraint: both branches of a line select the same frame.
- Probe counts: `32, 64, 128`, plus all available next-layer columns as a
  representation-only diagnostic.
- Probe columns: descending squared norm of the actual aligned next-layer
  weight column, stable tie order.
- Balance features: checkpoint first-moment difference plus exact probed
  next-layer ReLU difference; a count coordinate keeps q0/polar allocation
  balanced.

The rows are disjoint from D1 and lie in the independently sealed polar
capture, allowing a complete-frame association check before scoring.

## Gates

After target-free sealing, promote one fixed probe count only if on both
families it:

- has raw ratio to q0 at most `0.70`;
- retains at least `75%` of the complete-dual gain;
- improves at least three of four rows; and
- keeps checkpoint-state mean discrepancy below `1e-4`.

For the 64-probe arm, the extra projected observation cost is approximately
two quarter-width next-layer products.  Production work is licensed only if
the resulting total projection remains at or below the official Mini100
`1.10e-7` compute ceiling after current-R90 savings.  The all-column arm is a
capacity diagnostic and cannot be selected for production merely by passing
accuracy.

Failure kills this one-step weight-norm probe family.  It does not kill an
output-aware kernel coreset or a multi-step probe, which would require a new
capacity and cost argument.

No FlopScope session, package, upload, submission, or remote action is
authorized.
