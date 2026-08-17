# Gate-aligned QR quadrature scout — preregistration

Date: 2026-07-28.

Evidence scope: **component**. This is an ordinary PyTorch/CUDA accuracy
scout. It is not a FlopScope implementation, measured whole, compute
projection, package, or remote result.

## Hypothesis and frozen mechanism

Keep the K146/m17 primary 129-basis arm byte-identical. Replace only the
17-basis secondary arm's fixed input rotation by the canonical QR frame of
the current MLP's first-layer gate normals:

```text
W0 = Q R
canonicalize diag(R) >= 0
secondary rotation = Q.T
```

`Q.T` is orthogonal. Therefore the signed angular cloud still has exact zero
mean and identity covariance; only fourth-and-higher angular alignment to
the first-layer ReLU gate hyperplanes changes.

## Frozen bank and separation

- Independent-process Test500 rows `0..63`, in that order.
- Capture control and candidate predictions without opening
  `data_test.npz`.
- Hash and serialize both prediction arrays.
- Only then open `alm[:, 31]` and `post_var[:, 31]` for scoring.
- Target Monte Carlo noise is estimated as `mean(post_var[31]) / 1e6` and
  subtracted equally from paired control/candidate row losses.

The Test500 schema was inspected earlier in the wider first-place foray, so
this bank is model-unseen/post-seal, not historically target-unopened.

## Frozen hard gate

Continue only if all three hold:

1. pooled noise-corrected candidate/control MSE ratio is at most `0.95`;
2. each fixed 32-row half has ratio at most `0.98`;
3. the paired 20,000-draw bootstrap p95 of
   `candidate row MSE - control row MSE` is below zero.

Otherwise kill this exact gate-aligned QR mechanism. A target-open optimum
along the candidate-minus-control direction may be reported only as oracle
capacity, never as candidate evidence.

## Boundaries

No benchmark lane, physical row, FlopScope session, package, network action,
remote action, upload, or submission is authorized by this protocol.
