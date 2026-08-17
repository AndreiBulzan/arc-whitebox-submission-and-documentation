# Preregistration: deterministic antithetic transversal cubature N3/R1

Date: 2026-08-09

Evidence scope: target-free CUDA dictionary capture, target-free positive
cubature fitting, then a post-seal component score.  No FlopScope session,
physical row, package, Mini100 run, upload, submission, or remote action is
authorized.

## Question

N1 found a large complete opposite-frame correction and N2 showed that random
independent and six natural synchronized samples do not retain it.  Is the
256-line correction dictionary nevertheless compressible by one universal,
deterministically selected positive quadrature rule?

The rule is trained to reproduce the *complete rotated-frame correction*, not
the benchmark expectation target.  At evaluation time it needs only its
selected line transversals.

## Frozen capture

- Rows: the same sealed Full16 and Generated16 component rows as N1/N2.
- Angles: `pi/8` and `pi/4`, the two strongest complete-pair angles satisfying
  the N1 cross-family capacity gate.
- Transversal maps: the same six maps preregistered in N2.
- Seal every 256-vector transversal contribution for every
  row/angle/map/index before any target access.
- Require each dictionary mean to agree with the complete endpoint correction
  within `2e-5` maximum absolute error.

## Target-free fitting

For each angle/map, concatenate the Full16 line vectors over networks and
output coordinates.  Fit a positive simplex cubature by fully-corrective
Frank--Wolfe:

1. start with the line nearest the complete mean;
2. refit nonnegative weights summing to one on the active set by deterministic
   SLSQP;
3. add the inactive line with minimum objective gradient;
4. seal candidates at `l in {4,8,12,16,24,32}`.

The Generated16 dictionary is never used to fit weights.  It supplies a
target-free family-transfer diagnostic.  Seal every candidate correction,
weights, indices, train/validation relative correction MSE, cosine and norm
ratio before opening targets.

## Post-seal score and selection

The angle shrinkages are frozen from the already-sealed N1 complete-pair
screen:

```text
pi/8: 0.7580967151546559
pi/4: 0.6693308621190078
```

No new scalar is fitted in N3.  Score all sealed cubatures with those fixed
values.  A candidate passes the component gate only if:

1. `l <= 32` and no more than 40 complete-basis equivalents;
2. target-free relative correction MSE is at most `0.25` on both Full and
   Generated and correction cosine is at least `0.85` on both;
3. raw MSE ratio is at most `0.65` separately on Full and Generated;
4. projected adjusted score is at most `9.0e-8` separately on both; and
5. at least 11/16 rows improve in each family.

If multiple candidates pass, choose the lowest worst-family projected score,
then the lowest line count, then the frozen angle/map order.  Freeze that
single rule for a new target-free capture on the remaining Full84 and
Generated112 rows.  The broad gate is `<=8.5e-8` on both families before any
production or exact Mini100 work.

Failure closes this positive universal-cubature spelling for the six maps and
two angles.  It does not close network-adaptive target-free cubature.
