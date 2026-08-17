# R90 late-basis calibration: BF-XCORR and LMCBR E1 preregistration

Date: 2026-08-09

## Evidence boundary

This is an ordinary-CUDA, target-free capture followed by post-seal oracle
scoring on the official Mini100 dataset.  Its results are **component** and
**broad statistical** evidence only.  Compute and adjusted-score arithmetic
remain **projections**.  It is not a FlopScope physical row, an exact packaged
R90 prediction, or remote evidence.

No upload, submission, or remote action is authorized.

## Live owner and target ceiling

The sealed R90 manifest reconciliation established that the numerical owner is
the 129-basis O0-only graph with 66,048 rows and the pre-ReLU endpoint
correction `lambda=0.0075`.  O1 and the historical outer blend are absent.

The target is not a small local improvement.  A candidate must expose enough
raw-MSE headroom to support at least a 15--20% frozen improvement before any
packaged implementation is justified.  The BF-XCORR mechanism is rejected if
its target-aware grid has less than 20% capacity.  LMCBR is rejected if stable
target-aware shared basis weights have less than 35% pooled capacity or less
than 25% on either Mini100 half.

## Prior-art preflight and claimed new observable

The following negative evidence is controlling:

- fixed universal supervised basis weights collapsed to uniform;
- covariance-only 129-basis GLS became uniform to numerical precision;
- robust endpoint means, delete-one endpoint jackknives, global-lambda grids,
  sparse supports, checkpoint moment herding, and dynamic cloud compression
  failed;
- checkpoint pruning/calibration changed the propagated support and worsened
  raw error materially.

BF-XCORR differs narrowly: it uses a complement-derived shift **before the
final ReLU**, recentres the fold shifts to preserve the incumbent global
first-order correction, and can change only basis/correction self-correlation
and final gate crossings.

LMCBR differs narrowly: all 129 bases have already traversed the supplied
finite network.  It uses the realised per-basis final preactivation matrix
`M`, the realised post-correction endpoint matrix `Y`, and the per-network
analytic preactivation total `p`.  The earlier failed methods did not test
this all-bases, late, per-network auxiliary-total calibration.  This is a
narrow reopen, not permission to revisit generic basis weighting.

## Target-free capture

For every official Mini100 row, capture at the existing O0 `gamma_readout`:

- `M[b,j]`: radially scaled paired final preactivation mean for basis `b`;
- `Y[b,j]`: radially scaled, output-scaled endpoint after the incumbent global
  `lambda=0.0075` correction;
- `p[j]`: the analytic final preactivation proxy;
- the incumbent q0 prediction, which must equal `mean_b Y[b]` to `1e-10`;
- all BF-XCORR grid predictions described below.

The capture opens weights only.  It must be sealed and hash-pinned before the
post-seal scorer opens `targets.npy`.

## BF-XCORR frozen grid

Use the fixed permutation

```text
default_rng(2026080901).permutation(129)
```

and contiguous `array_split` folds for

```text
J = 2, 4, 8, 129
lambda = 0, .0025, .005, .0075, .01, .015, .025,
         .05, .1, .25, .5, 1
```

For fold `f`, form the complement preactivation mean and

```text
c_raw[f] = lambda * (p_unit - mean(M_unit outside f)).
```

Recenter the fold shifts so their size-weighted average is exactly the
incumbent global shift `lambda*(p_unit-mean(M_unit))`.  Apply each shift only
to the raw final preactivations belonging to that fold, then average all 129
post-ReLU basis endpoints uniformly.

The target-aware capacity report may inspect the complete fixed grid.  The
transfer test selects one `(J,lambda)` using rows 0--49 only and applies it
unchanged to rows 50--99.

BF-XCORR proceeds only if:

- best public-half raw reduction is at least 25%;
- frozen holdout-half raw reduction is at least 15%;
- neither half materially regresses; and
- pooled frozen reduction is at least 20%.

If target-aware grid capacity is below 20%, reject the mechanism outright.

## LMCBR E1 necessary-condition oracle

For each row, and for each fixed stability cell, solve the target-aware convex
problem

```text
min_w ||Y.T @ w - target||_2^2
subject to sum(w)=1, w>=0,
           max(w)<=w_max, sum(w^2)<=1/ESS_min.
```

Use the fixed grid

```text
ESS_min = 32, 64, 96
w_max   = 2/129, 4/129, 8/129.
```

One shared `w` is used across all 256 output coordinates.  Infeasible or
numerically unconverged cells fail rather than being silently relaxed.  Report
every fixed cell separately; do not choose a different constraint cell per
row for the primary gate.

LMCBR proceeds to a target-free final-only calibration only if at least one
fixed stability cell achieves:

- pooled raw reduction at least 35%;
- raw reduction at least 25% on rows 0--49; and
- raw reduction at least 25% on rows 50--99.

Failure closes the all-bases late-calibration family at the final endpoint.
Checkpoint extensions are not attempted after an E1 failure because they
would lack the necessary stable output-space capacity.

## Association and integrity gates

- Official Mini100 SHA-256 must equal
  `914aeb73cc7e39f6c110022d87ac8374f81c6340db616afb3f7a79af56bc6397`.
- The capture source, output, inputs, fold permutation, and grids are hashed.
- `mean_b(Y)` must reproduce the capture's q0 prediction within `1e-10`.
- The capture must contain only finite arrays and record zero target, physical,
  FlopScope, upload, submission, and remote accesses.
- The scorer must verify the target-free receipt before opening targets.

## Stop rule and next action

If BF-XCORR fails, do not tune another fold construction.  If LMCBR E1 fails,
do not add checkpoints or build a target-free solver.  Only a passing mechanism
is rebuilt inside the sealed R90 graph and then subjected to exact-archive
Mini100 association, physical accounting, and score-lineage gates.
