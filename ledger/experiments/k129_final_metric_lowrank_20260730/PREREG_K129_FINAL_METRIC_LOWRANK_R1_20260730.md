# K129 final metric-low-rank R1 preregistration

Evidence scope: ordinary-CUDA, final-layer-only component diagnostic. No
FlopScope, physical row, package, upload, submission, or remote action is
authorized.

The control is the current K129/R21 final-176 map, with the already selected
endpoint `lambda=0.0075` and fixed output scale `1.000025`:

`z_ctl = (h[S176] - mean(h)[S176]) @ W[S176] + mean(h) @ W`.

The sole candidate is fixed before target access:

1. choose the 168 highest sample-energy coordinates `K`;
2. let `D` be their 32-coordinate complement and
   `X_D = h[D] - mean(h)[D]`;
3. form `C = X_D.T @ X_D`;
4. eigendecompose `C`, then compute the rank-8 SVD of
   `C^(1/2) @ W[D]`;
5. use the eight left features `X_D @ C^(-1/2) @ U8` and right factor
   `diag(S8) @ V8`;
6. concatenate those eight factors with the exact centered 168-coordinate
   head, then restore the exact full affine mean.

Numerical stability rule fixed before any target access: if the double
precision metric-SVD realization has larger centered-product Frobenius error
than the exact eight-coordinate continuation represented by `S176 \ K`, use
that coordinate continuation. This is a target-free choice within the same
rank-8 admissible class and makes the construction-dominance gate literal
even when the deep cloud is numerically rank deficient.

This is a direct covariance-metric low-rank approximation of the actual
omitted final hidden state. It is not conditional imputation. The candidate
and control both use `lambda=0.0075` and scale `1.000025`.

The fixed untouched diagnostic rows are endpoint-grid positions:

- Full positions `72,73,74,75`, indices `148,151,153,155`;
- Generated positions `56,57,58,59`, indices `111,112,115,119`.

All eight control and candidate predictions must be sealed before either
target member is opened. Before sealing, each row must numerically verify
that the candidate centered affine-product squared Frobenius error to the
exact 200-coordinate product is no larger than the control error.

The only target gate is pooled noise-corrected raw MSE:

- `candidate / control <= 0.97` on Full4; and
- `candidate / control <= 0.97` on Generated4.

Failure on either family kills this exact final-only candidate. There is no
parameter grid and no late-layer extension in this task.
