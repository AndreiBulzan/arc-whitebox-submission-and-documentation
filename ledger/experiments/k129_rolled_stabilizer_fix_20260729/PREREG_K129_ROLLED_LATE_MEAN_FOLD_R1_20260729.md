# K129 rolled late omitted-mean stabilizer R1

Date: 2026-07-29.

Evidence sought: an ordinary-CUDA **component development diagnostic** for
accuracy and a **projection** for incremental operation count. This
preregistration authorizes no FlopScope run, physical benchmark, package,
network action, upload, submission, or remote action.

## Exact defect being repaired

The failed production-trajectory Green R1 transported every checkpoint mode
through one analytic-closure right/gate chain. That is not the deployed
operator. K129/O0 recomputes `energy_indices(current_state, 192)` independently
at every layer `24..30`; a correction can change the next keep set.

There is a smaller exact message at this boundary. Each late layer has 200
active inputs, keeps 192, and omits exactly eight. R1 computes the current
O0 sample mean of those eight omitted coordinates and transports it through
the corresponding eight rows of that layer's actual right matrix:

```text
S_l       = energy_indices(h_l, 192)
O_l       = {0,...,199} \ S_l
f_l       = mean_rows(h_l[:, O_l]) @ W_l[O_l, :]
z_l       = product(h_l[:, S_l], W_l[S_l, :]) + beta * f_l
h_{l+1}   = ReLU(z_l)
```

The next `S_{l+1}` is recomputed from this rolled `h_{l+1}`. No analytic
right or Gaussian gate transports the correction. At layer 31, production
already restores the omitted sample mean after its state-dependent
`keep=176` selection; R1 leaves that exact restoration unchanged and merely
reaches it through the rolled state.

The eight omitted coordinate means are the only new transported message.
The construction is shared across layers and networks, equivariant under
hidden/output permutations, positive-gauge covariant, and target-independent
at inference.

## Fixed kill experiment

Target-free CUDA capture rows:

```text
Full train       0..7
Full development 8..11
Generated guard  0..3
```

Before opening any target, capture and seal all predictions and all actual
late/final keep maps for the fixed global grid:

```text
beta = 0.00, 0.25, 0.50, 0.75, 1.00
```

`beta=0` must reproduce the frozen K129/O0 endpoint trajectory. A separate
post-seal scorer opens Full-train final targets, selects the beta with lowest
pooled train MSE (ties to smaller beta), and then evaluates the already-sealed
Full-development and Generated predictions. No development or Generated
target selects beta or changes the method.

The endpoint readout is the banked K129 `lambda=0.0075` statistic, never
`0.075`.

## Hard gate

Continue only if all hold:

```text
selected beta > 0
Full-development candidate / baseline raw MSE          <= 0.92
Generated candidate / baseline noise-corrected MSE     <= 0.92
row-ratio p95 on each family                           <= 1.25
at least half the rows improve in each family
all predictions and fold diagnostics finite
projected incremental count                            <= 2B
```

Failure kills this exact rolled omitted-sample-mean spelling. It does not
license a beta, layer, row, analytic/sample blend, or feature retune.

## Static deployment economics

For `N=66,048`, width 200, and seven late layers, one extra column mean and
one broadcast add cost approximately

```text
7 * 2 * N * 200 = 184,934,400 ordinary element operations.
```

The seven `8 x 200` vector-matrix transports and scalar scales add less than
`0.03M`. R1 declares `0.25B` as a conservative incremental-count projection,
well below the `2B` ceiling. This is not a FlopScope ledger.

