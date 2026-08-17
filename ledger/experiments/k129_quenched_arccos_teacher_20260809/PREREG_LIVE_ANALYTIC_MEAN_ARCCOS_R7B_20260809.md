# Preregistration addendum: CUDA-associated live analytic mean R7B

R7 generated its nominally seeded 512 synthetic probes on CPU, while the
sealed R1/R4 candidate features used CUDA. PyTorch does not promise identical
CPU and CUDA random streams; beta zero consequently failed the required R4
K129-feature association by `2.82`, making the R7 score invalid.

R7B reuses only R7's correctly associated live layer-31 means and regenerates
the probes on `cuda:0` under the benchmark lock with the original R4 seed and
dtype. Before sealing, beta zero must reproduce R4's `k129_full` feature to
maximum absolute error at most `1e-10`. The beta grid, ridge grid, scoring
gates, rows, and evidence boundary remain unchanged.

No target, official Mini100 row, physical FlopScope row, package, upload,
submission, or remote action is authorized by this addendum.
