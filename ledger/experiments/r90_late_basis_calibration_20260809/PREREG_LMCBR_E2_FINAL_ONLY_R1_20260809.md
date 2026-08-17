# LMCBR E2 final-only target-free calibration preregistration

Date: 2026-08-09

## Authorization and evidence

This experiment is authorized by the passing E1 necessary-condition oracle.
E1 found multiple fixed nonnegative stability cells with more than 95% raw-MSE
capacity on both official Mini100 halves.  E1 was target-aware and is not a
deployable estimator.

E2 constructs every prediction without targets, seals and hashes the complete
candidate grid, and only then opens official Mini100 targets.  Results are
**broad statistical** evidence for the frozen predictions and **projection**
only for any future compute/score claim.  No physical, package, upload,
submission, or remote action is authorized.

## Fixed final-only calibration

For each row, let `M` be the captured 129x256 radially scaled final
preactivation means, `Y` the 129x256 post-correction endpoints, `p` the
analytic preactivation proxy, and `u=1/129`.

For each coordinate scale, define

```text
X[b,j] = (M[b,j] - mean_b M[b,j]) / scale[j]
r[j]   = (p[j] - mean_b M[b,j]) / scale[j].
```

Use two target-free scale rules:

1. `identity`: `scale[j]=1`;
2. `basis_std`: sample standard deviation of `M[:,j]`, floored at 10% of
   the median positive standard deviation for that row.

For dimensionless ridge multiplier `rho`, compute the equality-centred ray

```text
G = X X.T
b = X r
lambda_abs = rho * trace(G)/128
delta = (G + lambda_abs I)^dagger b
delta = delta - mean(delta).
```

The fixed ridge grid is

```text
rho = 1e-4, 3e-4, 1e-3, 3e-3, 1e-2, 3e-2,
      1e-1, 3e-1, 1, 3, 10.
```

Along `w(eta)=u+eta*delta`, choose the largest `eta` in `[0,1]` satisfying

```text
w >= 0,
max(w) <= cap_multiplier/129,
sum(w^2) <= 1/ESS_min.
```

Use the full fixed stability grid

```text
ESS_min = 32, 64, 96
cap_multiplier = 2, 4, 8.
```

Return `Y.T @ w`.  There is no target-dependent per-row selection, shrinkage,
or fitting in the target-free builder.  Every one of the 198 fixed cells is
sealed for every row.

## Post-seal selection and transfer

After the candidate archive is sealed:

1. score all fixed cells on official Mini100 rows 0--49;
2. select exactly one global cell by lowest public-half mean raw MSE;
3. apply that same cell unchanged to rows 50--99;
4. report all cells, the frozen transfer, row wins, worst-row ratios, realised
   ESS/caps, ray amplitudes, and target-free proxy-mismatch reduction.

This public-half selection is a development choice, not evidence of
target-free hyperparameter identification.  Rows 50--99 are the controlling
frozen transfer test.

## Gates

Final-only LMCBR proceeds only if the public-selected fixed cell achieves:

- at least 15% raw reduction on rows 0--49;
- at least 15% raw reduction on rows 50--99 without retuning;
- at least 15% pooled raw reduction;
- no Mini100 half regresses; and
- at least 60 of 100 rows improve.

If the best public cell has useful signed shape but a clear amplitude miss,
one scalar shrink study may be preregistered separately.  No scalar shrink is
included in E2 R1.

A pass authorizes the output-coordinate cross-fit diagnostic and then adding
late checkpoints one at a time.  A fail closes final-only proxy calibration,
but does not erase the E1 observability result; checkpoints may be reconsidered
only if they add a genuinely new analytic total rather than another ridge or
scale spelling.

## Integrity

- Input capture and receipt must retain their target-free hashes.
- Candidate source, preregistration, input capture, output archive, scale
  names, ridge grid, and stability grid are hash-pinned.
- The builder must not import any target mapper or target-bearing dataset.
- All weights must satisfy the fixed constraints to `1e-10` and all candidate
  predictions must be finite.
