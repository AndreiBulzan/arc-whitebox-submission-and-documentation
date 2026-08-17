# Cubic repaired-H1 cross-fitted regression R2

Date: 2026-08-01

The rank-16 target-free capture is already sealed. Before reopening its
targets, this score stage fixes a cross-fitted control-variate estimator:

1. standardize the first `p` pre-Jacobian cubic controls on one parity of
   the 129 frames;
2. ridge-regress the 256-dimensional frame endpoint on those controls;
3. apply that regression to the exact-minus-observed control discrepancy on
   the opposite parity;
4. swap train/apply parities and average the two estimates.

The fixed grid is `p in {4, 8, 12, 16}` and dimensionless ridge
`alpha in {0, 0.01, 0.1, 1}` where the penalty is `alpha * n_train` after
standardization. No challenge target enters the estimator. Promotion needs
pooled MSE ratio at most 0.88, at least 10/16 rows improved, and maximum row
ratio at most 1.75. Failure kills this cross-fitted spelling.
