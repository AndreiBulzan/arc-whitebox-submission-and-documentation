# RADAR-K4 carried-pair graph gate preregistration

Date: 2026-07-28

Evidence class: offline **component** only.  This experiment must not invoke
FlopScope, run a physical row, edit a shared estimator, package, upload, or
submit anything.

## Question

Can exact teacher-forced angular pair state at layer `l-1`
(`cov,c21,c22,c31`), conditioned on the realised next weight matrix, predict
enough of the missing all-distinct/mixed-polarisation marginal K3/K4 at layer
`l` to be worth carrying in a reduced-compute estimator?

The previous repeated-view scout treated each output column independently.
This successor adds one new observable: messages between the 256 realised
next-layer queries under their exact preactivation correlation graph.
For a homogeneous cubic or quartic query, the natural interpolation kernels
are respectively `rho**3` and `rho**4`.

## Frozen protocol

- Full-process training MLPs: `0..63`.
- Full-process diagnostic MLPs: `64..95`.
- Full-process evaluation MLPs: `100..199`, excluding exploratory row `128`.
- Transition layers: `1,3,7,15,23,31`.
- Whole MLP is the split unit.
- Angular moments are obtained by exact Gaussian-radius de-mixing before
  connected cumulants are formed.
- Held capture opens only current post members and weight matrices.
  `pre_*` and `official_alm` are opened only by the scorer after predictions
  have been frozen.

The model inputs are:

1. exact pair-supported K3/K4 contractions and their diagonal/off-diagonal
   pieces;
2. query mean, variance, Edgeworth sensitivities, and elementary weight
   invariants;
3. row-normalised signed/absolute/cubic/quartic correlation-graph messages
   of those scalar channels.

Two fixed shared maps are frozen together:

- ridge (`lambda=1`);
- a two-hidden-layer `64x64` SiLU MLP trained for 60 fixed epochs with seed
  `2026072807`.

Both predict standardized marginal K3, standardized marginal K4, and the
direct normalized ReLU correction.  No held split chooses architecture,
features, epoch count, or seed.

## Gates

The primary teacher-forced gate is evaluated at layer 31 on held MLPs:

1. the best pre-frozen arm must reduce one-step ReLU-mean MSE by at least
   9% versus the strongest same-split non-graph baseline;
2. the gain must have the same sign in diagnostic and evaluation strata;
3. rowwise p90 must not worsen by more than 5%;
4. predicted marginal K3 and K4 must both have finite positive held
   correlation.

A stricter reduced-chassis relevance check is allowed only as an analysis:
on the already-existing K162 held prediction bank, fit one scalar affine
blend on source indices below 100 and report it on sources `100..199`
(excluding row 128).  A ratio `<=0.91` is the promotion bar.  This is
target-open component analysis, not a new sealed broad receipt.

There is no independent-process pair-state bank on disk.  Therefore no
Generated/Test500 family-transfer pass may be claimed.  The closest
available family stress is depth: a second map trained without layer 31 is
frozen and scored on layer 31.  Failure of that arm is recorded explicitly,
not relabelled as process-family evidence.

Only if the primary gate passes may work continue to a 2--4 layer free
rollout.  Otherwise the carried pair/correlation-graph representation is
killed under this model class.

## Economics boundary

The report must separate query-head cost from the unsolved cost of producing
and transporting exact pair state.  It may project dense covariance-query
and graph-message arithmetic, but it must not describe those projections as
a FlopScope receipt or a complete estimator price.

