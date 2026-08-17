# Late per-basis tail telescope T1 R1 preregistration

Date: 2026-08-08

## Question

Can the exact canonical K129 population be propagated only to a late
checkpoint, replaced there by one cheap moment closure **per Kerdock basis**,
and corrected with a small uniformly sampled set of exact basis tails?

This is not the previously rejected complete-population Gaussian tail: the
129 basis regimes are never pooled.  It is also not the rejected L4 prefix
telescope: the closure begins late and is nonlinear at every retained tail
layer.

## Access boundary and evidence

The capture is target-free.  It may read only network weights and the compact
canonical K129 design.  It must not read challenge expectation targets,
cached target errors, Mini100, a physical-row receipt, or any remote result.

The fixed development rows are:

- Full: `640, 641`
- Generated: `88, 89`

The result is **component** evidence.  All compute and score arithmetic is a
**projection**, not a physical receipt.

## Fixed arms

- checkpoints after layers `16, 20, 24, 28, 30, 31`;
- tail support sizes `q = 1, 2, 4, 8, 12, 16, 24, 32, 48, 64` of 129 bases;
- `mean_path`: propagate each basis empirical mean through the actual
  remaining weights and ReLUs;
- `diagonal_gaussian`: initialize each basis with its exact empirical
  coordinate means and variances, then use independent-coordinate Gaussian
  variance propagation and the exact univariate Gaussian-ReLU moments.

All 512 antipodal points of a sampled basis are propagated exactly through
the tail.  The same support is used for every output coordinate and every
tail layer.

## Estimator and exact variance

For exact basis response `y_b`, closure response `c_b`, residual
`r_b = y_b - c_b`, and a size-`q` uniform sample without replacement `S`,

```text
y_hat = mean_b(c_b) + mean_{b in S}(r_b).
```

It is exactly unbiased for the complete K129 mean.  Its conditional added
MSE per output coordinate is computed without Monte Carlo:

```text
V_q = (1 - q/129) / q
      * sum_b ||r_b - mean(r)||^2 / (256 * 128).
```

All reported variances are multiplied by the square of the exact expected
radius used by the canonical homogeneous estimator.

## Projection convention

The reference operating point is the sealed R89 projection:

- raw MSE `2.2274112794207212e-7` (unchanged from R87);
- effective compute `138.2229704775e9`;
- adjusted score `1.1339746073e-7`.

For a checkpoint with `h = 32 - checkpoint` tail layers, use the deliberately
simple optimistic proxy

```text
C_layer = 123487889023 / 32
C_new = C_R89 - h * C_layer * (1 - q/129) + h * C_closure
```

with `C_closure = 20e6` for `mean_path` and `40e6` for
`diagonal_gaussian`.  This proxy is used only to rank scientific capacity;
any survivor requires a separately hash-pinned production graph and physical
metering.

Because the official score is a mean of per-row adjusted losses, not the
product of aggregate means, the projected incremental score is

```text
S_new = S_R89
        - E_raw * (C_R89 - C_new) / 272e9
        + mean(V_q) * C_new / 272e9.
```

This preserves the observed R87 covariance between row error and row compute
inside `S_R89`; only the new constant-cost proxy and randomization variance
are approximated.

## Frozen gates

Primary continuation gate: at least one identical arm must project to
`<= 1.10e-7` on each family separately and `<= 1.08e-7` pooled.  This margin
is intentional because the cost proxy is optimistic.

Secondary continuation gate: if the primary gate misses, retain the mechanism
only when one identical arm improves the R89 projected score by at least 1%
on each family and has no row whose projected score exceeds R89 by more than
2%.

Otherwise reject only these fixed late per-basis mean/diagonal closures.  Do
not generalize a failure to richer per-basis nonlinear closures without a
separate oracle.

## Reproducibility

The sealed artifact must pin source, preregistration, compact-design, and
network-archive hashes; record CUDA/TF32 settings; refuse overwrite; and
coordinate through `runtime/.benchmark_lane.lock`.
