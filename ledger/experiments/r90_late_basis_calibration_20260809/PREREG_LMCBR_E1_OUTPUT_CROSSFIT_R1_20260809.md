# Preregistration: LMCBR E1 output-coordinate cross-fit R1

Date: 2026-08-09

## Question

Does the spectacular in-sample target-aware shared-weight capacity survive
when the 129 weights are fitted on one frozen half of the 256 output
coordinates and evaluated only on the unseen half?

The prior E1 fit used 129 constrained parameters against 256 outputs. This
test distinguishes a network-level basis-quality direction from
output-coordinate interpolation.

## Frozen cells and folds

- cells: ESS floor 32 with max-weight caps `2/129`, `4/129`, and `8/129`
- folds: even coordinates train / odd coordinates score, then odd train /
  even score
- one nonnegative shared weight vector per row and training fold
- same SLSQP tolerance, iteration limit, and feasibility checks as E1

## Gate

A cell demonstrates held-output capacity only if its assembled cross-fitted
prediction achieves:

- at least 35% pooled raw reduction;
- at least 25% raw reduction on each official Mini100 half;
- successful constrained solves for every row and both coordinate folds.

Failure does not alter the already-recorded in-sample capacity number. It
changes its interpretation from a reusable shared direction to an
in-sample target-aware interpolation capacity.

This is a post-seal **broad statistical** oracle. It authorizes no estimator
change, physical run, package, upload, submission, or remote action.

