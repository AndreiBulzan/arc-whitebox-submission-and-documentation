# Prefix-telescoping control R1 preregistration

Date: 2026-07-29

Evidence sought: **component** on the already-open eight-orientation
Full200/Generated128 atlas.  Count and score economics are **projection**.
This authorizes no FlopScope session, physical/timed row, estimator package,
upload, submission, or remote action.

## One architecture

R1 tests a depth-multilevel control, not another endpoint fit.  For one
complete Kerdock orientation, let:

```text
Y_b       = full-depth signed-preactivation endpoint of basis b
H4_b      = post-ReLU basis mean after layer 4
R4        = the target-blind mean-gated response from layer 4 to W31
S_b       = H4_b @ R4
A         = all 129 bases
P         = fixed 17-basis support
```

The candidate is the literal coefficient-one telescope

```text
T4 = mean_A(S_b) + mean_P(Y_b - S_b).
```

At inference, all 129 bases run only through the nonlinear L4 prefix.  The
17 pilot bases continue through the exact nonlinear suffix.  The large
prefix supplies a low-noise mean of a weight-specific nonlinear surrogate;
the pilot estimates only the suffix residual.  No target-fitted coefficient,
cross-network learner, endpoint calibration, adaptive support, or
same-realization regression is used.

The fixed pilot is the already sealed support:

```text
6, 9, 11, 37, 41, 42, 48, 52, 55,
58, 64, 65, 79, 108, 111, 112, 128
```

Only orientation 0 is used.  This avoids selecting an orientation or blend
after seeing the result.

## Why this is a new observable

- Gaussian closure discards every realized prefix gate cell.  `H4_b`
  retains the actual nonlinear Kerdock trajectory.
- Endpoint Edgeworth/PTCC and learned endpoint corrections act after the
  cloud has already paid for the full suffix.  R1 instead replaces most
  full-suffix propagation.
- Sigma-point/recubature methods replace the distribution at a boundary.
  R1 does not: it uses an exact multilevel identity with a correlated
  coarse path and estimates only its residual.
- The killed width-multilevel experiment used a cheaper width path and a
  sparse high-width correction at the endpoint.  R1 coarsens in **depth**,
  collapses the coarse suffix to one response, and preserves the actual
  nonlinear L4 prefix for every large-support basis.
- Mean-gated path controls used analytic/pointwise paths.  `S_b` is indexed
  by complete Kerdock basis replicates and begins from their realized
  nonlinear L4 states.

## Frozen screen

The target-free phase reads only:

```text
indices.npy
completion.npy
observer_basis_postmean.npy
response_matrix.npy
final_basis_premean.npy
```

from the hash-pinned Full200 and Generated128 atlases.  It freezes, for
every row:

```text
full_O0 = mean_A(Y_b)
pilot_O0 = mean_P(Y_b)
telescope_O0 = T4
```

Only after that NPZ and its hashes exist may a separate scorer open the
atlas `target_final_premean.npy`.

## Fixed gates

The mechanism survives only if both Full200 and Generated128 satisfy all
of:

```text
MSE(telescope_O0, full_O0) / MSE(pilot_O0, full_O0) <= 0.20
pooled signed-target MSE(telescope_O0)                <= 5.0e-7
signed-target MSE(telescope_O0) / MSE(full_O0)        <= 1.20
row-ratio p95 versus full_O0                          <= 2.00
all arrays finite
```

The first condition requires at least a fivefold reduction in the
support-estimation error.  The second is a conservative proxy for a
`~2e-7` final-ReLU raw candidate; it is not itself a final-layer score
claim.  Failure on either family kills this exact L4 telescope.

## Current-meter economics

Using the remote K146 count anchor `144.015 B` only as a linear
basis-layer projection, the nonlinear work fraction is:

```text
(129*4 + 17*(31-4)) / (146*31) = 975 / 4526 = 0.2154.
```

This gives about `31.0 B` counted propagation.  One collapsed response,
prefix reductions, and ordinary movement are budgeted at `<=2 B`.
Allowing `0.10 s` residual gives:

```text
projected effective compute <= 43 B.
```

That is under the requested `50 B` architecture envelope and under the
`272 B` hard cap.  This is a **projection**, not a receipt.  A passing
accuracy screen would still require a capsule-native 0.9.1 static ledger
before any physical work.

