# Preregistration: official-Mini100 K129 floor-scaling re-audit F1/R1

Date: 2026-08-10

## Question

Does a uniformly sampled low-K subset of the already sealed 129 complete
Kerdock basis endpoints retain approximately constant adjusted score as K is
reduced to the 27.2B compute floor?

## Frozen input

Use only the previously target-free sealed endpoint tensor
`runtime/artifacts/r90_bf_xcorr_lmcbr_final_mini100_r1_targetfree_20260809.npz`
and the official Mini100 targets.  The endpoint tensor was sealed before any
target access and its 129-basis mean reproduces the q0 prediction.

## Exact finite-population calculation

For endpoint vectors `y_b`, full mean `ybar`, target `theta`, population size
`N=129`, and a uniform size-K subset sampled without replacement,

```text
E_A MSE(mean_{b in A} y_b, theta)
 = MSE(ybar, theta)
 + (1-K/N)/K * [sum_b ||y_b-ybar||^2 / ((N-1)*256)].
```

Compute this row by row, then average separately over public rows 0--49,
holdout rows 50--99, and all 100 rows.

Frozen K grid:

```text
4, 8, 12, 16, 20, 24, 25, 32, 40, 50, 64, 96, 129
```

Fit the descriptive log slope `p` over K in `{12,16,20,24,25,32,40,50}`.
This is a diagnostic, not a claimed asymptotic exponent.

## Cost projection

Use the observed R92 remote mean effective compute `139.6562318655B` as the
K129 anchor and the explicitly optimistic linear projection

```text
C_K = C_129 * K / 129.
q_K = max(0.1, C_K / 272B).
```

Report projected adjusted error `E[MSE_K] * q_K`.  This is a projection, not
a physical or remote receipt.  Also report the K at which the linear model
crosses the floor.

## Decision

- `floor_match`: pooled K25 projected adjusted score is no more than 1% worse
  than pooled K129 under the same linear model.
- `floor_improve`: it is at least 1% better.
- `floor_worse`: it is more than 1% worse.

No estimator, package, upload, or submission is authorized by this audit.
