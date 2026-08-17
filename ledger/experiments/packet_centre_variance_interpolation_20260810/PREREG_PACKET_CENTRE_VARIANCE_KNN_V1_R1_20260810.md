# Packet centre-dependent variance interpolation V1/R1 preregistration

Date frozen: 2026-08-10

Evidence class: target-free **component** oracle.  No benchmark expectation,
Mini100 target, FlopScope session, physical row, package, upload, submission,
or remote action is in scope.

## Question

Can a small number of exact full-covariance Gaussian anchor trajectories
supply the *centre-dependent* marginal-variance schedule needed by the
integral-preserving packet estimator?

The sealed packet teacher reduces the canonical error materially, and the
micro oracle showed that its marginal preactivation-variance field is highly
compressible across centres.  The broad fixed-anchor experiment nevertheless
failed by orders of magnitude because it averaged the anchor schedules and
broadcast one common vector to all 66,048 centres.  The surviving packet
correction is a tiny signed common mode after a large cancellation, so that
centre averaging is not an innocuous approximation.

## Prior-art and target-ceiling preflight

Capsule searches covered `packet`, `fullcov anchor`, `variance field`,
`centre-dependent`, `interpolation`, `Nyström`, `nearest`, `shared
covariance`, `tangent cloud`, `Price`, and `Wick`.

Nearest controlled results:

- V1: exact teacher variances have strong low-dimensional centre structure;
- A1: one antipodal full-covariance pair works locally;
- A2/B1: one shared schedule fails globally;
- R3/R5: diagonal/block and stochastic low-rank covariance dynamics fail;
- PW2: a complete *shared*-covariance Price/Wick series converges while
  missing the global signed mode;
- C1/C2: first-Hermite tangent laws fail from layer two;
- local packet GP: two deep antipodal values predict large local sources but
  miss the cancellation common mode.

No capsule artifact transports one exact variance vector per anchor and uses
the current centre's realized conditional mean to select/interpolate a
different schedule at every centre and layer.  This is therefore a materially
new observable, not another shared covariance, low-rank neural state, or
candidate-output predictor.

The ceiling is sufficient.  A faithful packet correction has roughly 50%
raw-error headroom.  Even a production projection containing one K129
conditional-mean cloud, 8--16 exact antipodal anchor pairs, and centre/anchor
similarities can remain score-positive if it retains at least 70% of that
correction.  This component run makes no compute or score claim.

## Frozen geometry and recurrence

Use the sealed Full8/Generated8, 64-replicate Gaussian-packet population and
the same packet constants as A2.  Reconstruct all 66,048 oriented K129
centres.  Select sixteen target-blind, Kerdock-spread lines; always include
both signs.  Evaluate prefixes of 4, 8, and 16 antipodal pairs.

For every anchor, propagate the exact nonzero-mean full-covariance Gaussian
ReLU recurrence.  Retain at each layer:

- its exact preactivation mean vector;
- its exact marginal preactivation variance vector.

For a cloud centre at layer `l`, use its currently propagated conditional
preactivation mean as the only interpolation feature.  Standardize every
feature coordinate by the anchor mean and RMS at that layer, normalize each
feature row, and use cosine nearest neighbours among the active anchors.
Predict the centre's variance vector as the geometric mean of its neighbours'
exact variance vectors.

The neighbour count is selected target-free at every layer from
`k in {1,2,4,8}` by leave-one-anchor-out log-variance MSE.  Ties choose the
smaller `k`.  No packet output, benchmark target, fitted cross-network
coefficient, amplitude, shrinkage, or post-score choice enters this rule.

Propagate the complete conditional-mean cloud with the exact univariate
Gaussian-ReLU mean under the predicted centrewise variances.  This uses one
cloud matrix product per layer; it does not require a second literal cloud in
a production spelling.  The literal cloud is computed here only as an
association checksum.

## Frozen decision gates

Compare each prefix candidate with the independent packet-replicate mean,
subtracting the packet-mean Monte Carlo variance exactly as in A2.

A primary pass requires one identical prefix to satisfy all of:

- pooled final correction fidelity `>= 0.70`;
- Full and Generated final fidelity `>= 0.60` each;
- pooled final correction cosine `>= 0.85`;
- unbiased residual MSE `<= 3.1e-8`;
- literal Q0 association maximum absolute error `<= 2e-6`;
- finite predictions and variances;
- median selected leave-one-anchor-out log-variance MSE improves on the
  shared-geometric-mean baseline.

A near pass is pooled fidelity `>= 0.50`, cosine `>= 0.75`, and neither
family fidelity below `0.35`.  A pass licenses a separately preregistered
official-Mini100/physical-cost program.  A failure kills this exact
nearest-neighbour variance-interpolation law, not arbitrary centre-dependent
full-covariance compression.

