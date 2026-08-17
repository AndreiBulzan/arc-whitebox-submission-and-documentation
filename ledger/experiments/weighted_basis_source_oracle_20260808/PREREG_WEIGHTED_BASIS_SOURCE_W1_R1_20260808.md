# Weighted complete-basis source W1/R1 preregistration

Date: 2026-08-08

Evidence scope: target-free, unavailable-output **component** capacity oracle
on the sealed complete-basis packet-source capture, followed by a post-seal
development-target score.  Costs are projections.  No Mini100, physical
row, FlopScope session, package, upload, or remote action is authorized.

## Question

The prior complete-basis oracle required the selected bases to have equal
Horvitz--Thompson weights.  Is that restriction hiding a sparse collective
representation?

For the 129 exact complete-basis output-source vectors `Y_a`, let
`Delta = sum_a Y_a`.  For `S in {1,2,4,8,16}`, choose a support and arbitrary
signed least-squares weights to minimize

```text
|| sum_{a in support} w_a Y_a - Delta ||^2 / 256.
```

Signed weights are a strict capacity relaxation: failure also rejects
nonnegative weights for the same support budget.  Success is not a lawful
estimator by itself because the complete output-source vectors are
unavailable in production.

## Fixed solver

Use the sealed 32-replicate basis capture on Full640--641 and
Generated88--89.  Fit separately to the full replicate set and both
16-replicate halves.

For each row and for the four-row concatenated universal problem:

- build an orthogonal-matching-pursuit path to 16 atoms;
- at each requested `S`, apply exact least-squares one-for-one swap descent;
- use 32 additional deterministic random starts, each refined by the same
  swap descent;
- retain the smallest source residual;
- record support, weights, condition number, L1 weight norm, and residual.

Evaluate half-trained supports and weights on the opposite replicate half.
Selection never reads benchmark expectation targets.

## Gates

A capacity survivor requires, after post-seal scoring:

- at least 35% pooled raw-MSE reduction;
- positive reduction in both Full and Generated families;
- `S<=8`;
- the same sign of improvement when the winning half-trained rule is applied
  to the opposite replicate half.

An `S=16` result is diagnostic only because the simple complete-basis
propagation projection is already about `34.4B`.

If the signed per-row output oracle fails by a large margin through `S=8`,
close sparse weighted complete-basis source reconstruction at that budget.
If it is close to the gate, retain the mechanism as solver-inconclusive rather
than treating heuristic sparse search as a theorem.

## Integrity

- Refuse overwrite and hash-pin preregistration, source, grouped capture,
  grouped report, and output.
- The scorer must verify all seals before opening the fixed development
  targets.

