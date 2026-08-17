# Gate-boundary adaptive cubature R1

Date: 2026-07-29

Evidence boundary: target-free **component** screen.  This is not a challenge
score, a physical run, a package, or a remote result.

## Question

Can an inexpensive, weight-conditioned pilot consisting of the literal first
preactivation gate cells of all 129 matched O0/O1 bases select 17 O1 bases
whose signed-final-preactivation mean better matches the frozen S109
reference than the current fixed S17?

## Fixed observable

For basis `b`, matched positive-half rows `r`, first-layer neuron `j`, and
normalized first preactivations `u0`, `u1`, define four gate-cell innovations:

1. net tangent crossing: `mean(1[u1>0] - 1[u0>0])`;
2. signed crossing margin:
   `mean((1[u1>0] - 1[u0>0]) * min(abs(u0),abs(u1)))`;
3. exact ReLU-curvature difference: `0.5*mean(abs(u1)-abs(u0))`;
4. unsigned cell disagreement: `mean(1[(u1>0)!=(u0>0)])`.

Each neuron coordinate is multiplied by a target-free downstream
mean-square Price adjoint derived only from `W1..W31`: reverse squared-weight
transport with a `1/sqrt(2)` ReLU derivative-energy factor on hidden gates,
renormalized after each layer.  Each of the four 256-coordinate channels is
then divided by one across-basis scalar RMS.  There are no fitted
coefficients, endpoint features, target moments, or challenge targets.

## Fixed selection and evaluation

For each MLP independently:

1. form the S109 centroid of the 1,024-dimensional innovation;
2. greedily add 17 distinct bases minimizing the squared distance of the
   partial equal-weight centroid to the S109 centroid, with lower basis ID as
   the tie break;
3. only after the support is fixed, open the frozen O1 per-basis signed final
   preactivation endpoint;
4. compare the selected mean and the current fixed-S17 mean with the same
   S109 endpoint mean.

The primary corpus is the frozen Full held 100.  Generated held 64 is a
transfer corpus and may not select a feature, scale, rule, or threshold.

Also report the Pearson and rank correlations between each basis's
gate-feature distance to the S109 feature centroid and its exact endpoint
distance to the S109 endpoint centroid.  These correlations are descriptive;
they cannot alter the selection rule.

## Kill gate

Kill the mechanism unless both families satisfy all of:

- pooled candidate/fixed endpoint MSE ratio `< 0.70`;
- row-ratio p95 `< 1.0`;
- at least 75% of rows improve;
- the pooled risk/error correlation has the same positive sign.

The computation opens only the `weights.npy` member of each weight archive,
the immutable compact Kerdock geometry, the frozen support/split file, and
the atlas indices/completion/endpoint members.  It performs no FlopScope,
benchmark, package, network, upload, or submission action.

