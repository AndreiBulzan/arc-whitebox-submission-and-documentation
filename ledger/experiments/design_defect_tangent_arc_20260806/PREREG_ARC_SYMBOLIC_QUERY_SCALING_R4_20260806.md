# ARC symbolic query-scaling audit R4

Date: 2026-08-06

Evidence scope: **component** only.  This is a source/tractability audit of
ARC's public symbolic cumulant-propagation implementation.  It is not an
estimator, benchmark run, score projection, package, upload, submission, or
remote action.

## Question

Does the released ARC code already contain a practically reusable
query-directed path for the signed rank-6/8/10 design-defect transport needed
after the Poisson-band oracle?

The public symbolic backend is the closest candidate: it builds a tensor
network for a requested final mean and can search contractions across layers.
The audit measures whether that backend remains tractable as cumulant order
and depth increase.  It does **not** test a future custom adjoint recurrence.

## Frozen source

Use the unmodified public repository
`alignment-research-center/mlp_cumulant_propagation`, pin its current commit,
and hash the relevant source files.  No source in that checkout may be edited.

Run `abstract_kprop` with:

- `output_all=False`;
- `mean_only=True`, `var_only=False`;
- `simplify=True`, `split=True`, `prune=True`;
- one fresh subprocess per `(k_max, depth)`;
- a 60-second hard timeout and a 32 GiB address-space ceiling per subprocess.

The originally drafted 4 GiB virtual-address ceiling was raised before any
row was accepted: importing the environment's PyTorch CUDA libraries alone
failed to map under 4 GiB.  Actual peak RSS remains the memory measurement.

The grid is deliberately stair-stepped:

```text
k_max 1: depth 2, 4, 8, 16, 32
k_max 2: depth 2, 4, 6, 8, 12
k_max 3: depth 2, 3, 4, 5, 6
k_max 4: depth 2, 3, 4
k_max 5: depth 2, 3
k_max 6: depth 2, 3
```

Record completion, wall time, peak RSS, final-mean diagram count, total final
diagram count, and maximum tensor/index counts in any final-mean diagram.

## Interpretation gate

The released backend counts as a **turnkey bridge** only if `k_max=6` reaches
depth 3 and its observed growth, together with the lower-order depth curves,
supports depth 32 without more than `1e6` final-mean diagrams or 15B estimated
width-256 arithmetic.  Otherwise the source audit rejects turnkey reuse but
does not reject a genuinely new query-directed tangent recurrence.

This distinction is controlling: failure of a deprecated full-diagram engine
cannot prove that the desired mathematics is impossible.
