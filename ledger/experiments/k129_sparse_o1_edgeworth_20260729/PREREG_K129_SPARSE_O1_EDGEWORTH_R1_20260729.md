# K129 sparse-O1 Edgeworth R1 preregistration

Date: 2026-07-29

Evidence scope: offline CUDA **component** accuracy acquisition and
post-seal development scoring only. No FlopScope physical row, estimator
edit, package, upload, or remote submission is authorized.

## Question

Can two target-free-selected orientation-1 bases (`70,45`) estimate the
full orientation-1 final raw moments accurately enough to improve the
K129/O0-only estimator by at least `3.5%` raw MSE?

The fixed correction is

```text
candidate = q0(lambda=.0075) + .0734 * (estimated_O1_Edgeworth3433 - q0)
```

The coefficient is frozen before this acquisition from the existing
full-17 O1 Edgeworth diagnostic. It is not retuned on the sparse capture.

## Estimators frozen before targets

For supports `[70]`, `[45]`, and `[70,45]`:

1. direct sparse-support raw moments;
2. matched-pair moment control:

   `raw(O0 full) + mean_b(raw(O1 b) - raw(O0 b))`;

3. matched-pair readout control:

   `E3433(O0 full) + mean_b(E3433(O1 b) - E3433(O0 b))`.

The matched O0 rows are already present in K129. Only the sparse O1
trajectories would be incremental in a deployable successor.

## Bounded ladder

First capture and score the first eight already-named Full100 and
Generated64 rows. Expand to all `100+64` only if one two-basis spelling
shows a credible family-wise signal, provisionally:

```text
Full8 raw ratio       <= .975
Generated8 raw ratio  <= .975
```

The broad promotion target is at least `3.5%` raw gain on both families.
At the projected two-basis count increment of about `1.95B`, that is large
enough to be relevant to the `1.2e-7` checkpoint. Tiny-slice results are
only component diagnostics.

