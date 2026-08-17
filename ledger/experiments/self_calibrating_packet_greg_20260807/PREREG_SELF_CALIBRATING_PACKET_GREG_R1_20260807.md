# Self-calibrating partition GREG R1 preregistration

Date: 2026-08-07

## Question

Can an exactly block-centred candidate feature span learn enough of the
supplied network's *within-line* packet-output variation that one uniformly
random candidate pair per each of the 33,024 K129 lines, corrected by an
honest cross-fitted generalized-regression estimator, retains at least 70% of
the measured Gaussian-packet gain?

This round tests feature-span capacity and one-observation-per-line
learnability.  It does not implement an estimator, price a production graph,
open a FlopScope session, package, upload, or submit.

## Prior-art preflight

Searches covered `GREG`, generalized regression/difference, cross-fitting,
control variates, within-MLP kernels, block centring, balanced sampling, and
one-candidate-per-line selection.

Nearest controlled negatives are:

- `matched_orientation_trajectory_20260729`: within-MLP kernel/GREG predicted
  an orientation-population proxy, not a randomized finite packet population;
- `h1_joint_frame_greg_20260731` and the H1 polynomial controls: regressed
  complete-frame or analytic moment summaries whose means were already fixed
  by K129, not candidate identities with a known zero within-line feature
  total;
- packet-cubature R2/R3: optimized the native NNGP/Nystrom metric and failed
  to balance actual outputs, but did not fit the best finite-network linear
  map from those feature spans to candidate-output deviations.

The new observable is materially different: every line supplies a randomized
candidate identity, all candidate features are known and sum exactly to zero
within that line, and the selected pair's actual 256-output network response
is already part of the one-frame estimator.  This identifies the aggregate
feature/output cross-moment without requiring the other seven responses.

The old statement that polynomial input controls integrate to zero does not
close this mechanism: zero is precisely the known auxiliary population mean
used by the generalized-difference correction.

## Sealed inputs and rows

Use the exact packet construction and rows already fixed by:

- `runtime/artifacts/partition_balanced_packet_output_oracle_r1_targetfree_20260807.*`;
- `runtime/artifacts/partition_balanced_packet_nngp_selector_r2_targetfree_20260807.*`;
- `runtime/artifacts/partition_balanced_packet_first_layer_tail_kernel_r3_targetfree_20260807.*`.

Rows are Full `640..647` and Generated `88..95`.  The packet radius, K129
line ordering, candidate seed, antipodal pairing, and float32 network replay
must associate with the sealed captures.  This is a reused regression bank,
not a virgin holdout.

The complete candidate-output tensor may be recreated in memory but must not
be retained as a new training corpus.  Only sufficient statistics,
cross-fitted aggregate predictions, diagnostics, and hashes are written.

## Fixed feature variants

Four existing feature spans are tested without changing their landmarks,
kernel recurrence, or candidate construction:

1. `U8R64`: universal even depth-32 NNGP difference features, `m=8`,
   `p=64`, `r=64` (former P8 spelling);
2. `U8R128`: universal even depth-32 NNGP difference features, `m=8`,
   `p=128`, `r=128` (former D8b spelling);
3. `F4R64`: exact first-layer pair states followed by the conditional
   depth-31 NNGP difference map, `m=4`, `p=128`, `r=64`;
4. `F4R256`: the same first-layer-conditioned construction with `m=4`,
   `p=256`, `r=256`.

Every feature tensor is centred separately within each line in float64 and
must have maximum absolute line-sum drift at most `2e-10`.

No feature family or rank is selected using truth targets.

## Oracle feature-span calculation

For one network and feature variant, let `X[i,s]` be the centred feature and
let `V[i,s] = A[i,s] - mean_s A[i,s]` be the centred actual candidate-pair
output.  Define

```text
B_star = pinv(sum_{i,s} X[i,s]^T X[i,s])
         @ sum_{i,s} X[i,s]^T V[i,s]
```

using an eigenvalue cutoff of `1e-10` times the largest eigenvalue.  Record

```text
delta_random = sum ||V||^2 / (256 * M^2 * m)
delta_oracle = sum ||V - X B_star||^2 / (256 * M^2 * m)
R2_oracle = 1 - delta_oracle / delta_random.
```

These are exact conditional independent-selection variances.  Also record the
rank-`q` output-energy fractions for `q = 1,2,4,8,16,32,64,128,256` from the
`256 x 256` candidate-output covariance.  Frobenius energy is diagnostic;
the controlling metric is `delta_oracle`.

## Honest one-observation-per-line simulation

For each network and each of 16 fixed selection seeds:

1. choose every line's label independently and uniformly from its `m`
   candidates;
2. assign deterministic folds by `line_index mod 4`;
3. for held fold `k`, fit `B[-k]` only from the selected features and outputs
   in the other three folds;
4. use ridge `alpha = 1e-3 * trace(X_train^T X_train) / r`;
5. return

```text
mu_hat = mean_i [A[i,S_i] - X[i,S_i]^T B[-fold(i)]].
```

The four fold label vectors are independent.  Therefore `B[-k]` is
independent of the held fold's labels, and uniform candidate marginals give
exact design unbiasedness for the finite pool.  Merely marginally uniform
labels from one globally coupled cube design would not justify this proof and
are explicitly excluded from R1.

Record independent-random and GREG selected means, their target-free MSE to
the exact finite-pool mean, seed distributions, coefficient norms, condition
numbers, and cross-fit variance reduction.  The other candidates' outputs may
be used to score this simulation and form `B_star`, but never to fit a
cross-fitted `B[-k]`.

## Target ceiling

On this already opened reused bank, the sealed `m=8` figures are:

```text
q0 raw       = 3.4621648278e-7
ideal raw    = 1.6073304519e-7
m=8 pool raw = 1.6797569543e-7
```

Seventy percent ideal-gain retention corresponds to raw
`2.1637807647e-7`, hence permits at most `4.8402381038e-8` additional
selection variance over the finite pool on this bank.  To leave room for
one-sample coefficient estimation, the target-free oracle-span promotion
limit is `4.0e-8` pooled.  This is a preregistered ceiling, not a new score
claim.

## Gates

A feature span passes capacity only if all hold:

- pooled mean `delta_oracle <= 4.0e-8`;
- Full and Generated mean `delta_oracle <= 4.8402381038e-8` separately;
- pooled `R2_oracle >= 0.70`;
- all associations, finite-value checks, and line-centering checks pass.

A feature span passes one-frame learnability only if, in addition:

- pooled mean cross-fitted pool-reconstruction MSE across the 16 seeds is
  `<= 4.8402381038e-8`;
- Full and Generated means are each `<= 6.0e-8`;
- cross-fitting retains at least half of the oracle variance reduction;
- the squared bias of the seed-mean prediction from the pool is no more than
  `10%` of its cross-fitted reconstruction MSE;
- at least 12 of 16 selection seeds have pooled reconstruction MSE below the
  corresponding independent-random result.

Only a target-free capacity-and-learnability pass licenses a post-seal target
score.  Post-seal promotion then requires at least 70% ideal packet-gain
retention pooled, at least 60% separately on Full and Generated, and at least
12/16 networks improved versus q0 after averaging over selection seeds.

Failure of every `B_star` capacity gate kills metric-calibration and
one-frame GREG for these existing feature spans and licenses only the sparse
exact layer-two probe feature oracle.  An oracle-span pass followed by a
cross-fit failure licenses a small complete-candidate pilot study, not deeper
features.  A full pass licenses a production-shaped cost audit, not estimator
implementation.

## Evidence boundary

The target-free tensor/projection and cross-fit simulations are **component**
evidence.  Any post-seal score is **broad statistical exploratory on a reused
bank**.  No result here is a measured whole, remote result, Mini100
validation, production compute projection, package authorization, upload, or
submission authorization.
