# Schur pair exact-displacement selector R1

Date: 2026-08-05

Evidence sought: target-free **component** capture followed by broad
statistical scoring on the already sealed Full100 and Generated128 rows.
This pass authorizes no FlopScope run, package, upload, or submission.

## Prior-work boundary

The capsule has tested angle-rank alternation, unconstrained energy
partitioning, first-layer moment descent, and the pair-stratified family

`sum(coordinate^2 * downstream_sensitivity^gamma)`

for `gamma in {0, .25, .5, .75, 1}`.  A repository-wide search found no
test that prices the actual displacement caused by selecting a Schur plane.

## Frozen rule

Keep the successful R83 construction unchanged: real Schur blocks are
sorted by absolute angle and paired consecutively, and exactly one block per
pair is selected.  Let `theta_j` be a block's signed Schur angle.  Select the
larger value of

`2 * (1 - cos(theta_j)) * sum(coordinate_j^2 * sensitivity^0.5)`.

The factor is not fitted.  For a two-dimensional rotation `R(theta)`,

`||(R(theta) - I)x||^2 = 2 * (1 - cos(theta)) * ||x||^2`,

so this is the existing downstream-weighted metric multiplied by the exact
squared perturbation made by activating that plane.  One-dimensional `+1`
blocks receive factor zero and `-1` blocks factor four through the same
formula.  There are no alternative exponents or post-target choices.

## Gate

Compare against q0 and the already sealed `gamma=.5` incumbent.  Promote the
unchanged rule to official Mini100 only if:

- its q0 raw-MSE ratio is at most `.935` on both Full100 and Generated128;
- its pooled q0 ratio is at most `.925`;
- it does not increase aggregate raw MSE versus gamma=.5 on either family;
- and it improves at least 50/100 Full and 64/128 Generated rows versus q0.

Failure kills only this exact displacement-weighted selector.
