# SVD-conditioned K146 secondary orientation R1

Date: 2026-07-29.

Evidence scope: **component**. This is an ordinary PyTorch/CUDA accuracy
screen. It is not a FlopScope implementation, compute receipt, measured
whole, package, or remote result.

## Frozen mechanism

Keep the complete 129-basis primary arm and every downstream K146 operation
unchanged. Replace only the 17-basis secondary input rotation by the
canonical right-singular frame of the supplied MLP's first weight matrix.
For

```text
G = W0.T @ W0 = V diag(lambda) V.T
```

sort eigenvalues in descending order, canonicalize every eigenvector so its
largest-magnitude coordinate is positive, and use `V.T` as the secondary
rotation. This is distinct from the previously killed QR frame: QR uses the
left orthogonal factor of `W0`, whereas this screen uses the eigenframe of
the output-side Gram matrix `W0.T @ W0`.

The frame is an orthogonal function of the current network's weights only.
It preserves exact signed-cloud zero mean and covariance. Only
fourth-and-higher angular structure changes.

## Frozen small screen

- Full rows: `3, 103, 203, 303`.
- Generated rows: `3, 35, 67, 99`.
- Capture control and candidate predictions from weights only.
- Require the primary-arm output to be bit-identical for every pair.
- Serialize and hash predictions before opening any target member.
- Score Full against `targets.npy[:, 31]`.
- Score Generated against `target_rows.npy[:, 31]`, subtracting the frozen
  per-row `label_noise_mse.npy` equally from control and candidate.

## Hard gate

Continue this exact mechanism only if the pure SVD candidate has pooled raw
MSE ratio at most `0.90` on Full and pooled noise-corrected MSE ratio at most
`0.90` on Generated. Otherwise stop without a larger capture, physical
implementation, or tuning.

For diagnosis only, also freeze before targets the fixed damped output

```text
control + 0.25 * (SVD - control)
```

The coefficient `0.25` is theory-motivated by the prior QR direction's
target-open oracle scale near `0.29`, but that observation came from a
different mechanism. This damped arm is therefore explicitly secondary and
cannot override failure of the pure-SVD hard gate.

## Boundaries

No FlopScope session, physical row, package, network action, remote action,
upload, or submission is authorized by this protocol. The shared benchmark
lock must cover the CUDA capture.
