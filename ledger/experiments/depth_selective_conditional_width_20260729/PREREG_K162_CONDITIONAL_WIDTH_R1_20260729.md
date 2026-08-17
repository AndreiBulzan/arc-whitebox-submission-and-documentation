# K162 depth-selective conditional-width R1 preregistration

Date: 2026-07-29

Evidence sought: **component** accuracy and **projection** current-meter
economics only. This authorizes one target-free CUDA capture and a separate
post-seal score on already-used development rows. It authorizes no physical
FlopScope run, package, network action, upload, or remote submission.

## Question

Can the exact K162/m33 numerical chassis spend less trajectory width in the
early/middle network without throwing away the omitted fluctuations?

The incumbent retains 232 analytic-screen coordinates through layer 11 and
then retains 216 coordinates through layer 20. Its omitted-coordinate fold
transmits only the analytic mean. The fixed R1 candidate instead retains 200
coordinates from layers 8 through 20 and uses the analytic Gaussian
post-ReLU covariance to reconstruct the omitted fluctuation linearly.

For retained coordinates `K` and omitted coordinates `D` at a source layer,

```text
B = pinv(C_KK) C_KD
h_D ~= mu_D + (h_K - mu_K) B
```

The next affine map is evaluated without materializing `h_D`:

```text
W_K' = W_K + B W_D
fold' = (mu_D - mu_K B) W_D / sphere_er
```

This is the target-free minimum-MSE linear conditional reconstruction under
the same analytic covariance closure already used to choose the incumbent
screens. It is not a learned correction and has no challenge-target
coefficient.

## Frozen identities

Chassis:

- orientation supports: literal frozen `129 + 33 = 162`;
- H2 repair, layer-4 snap, late 192-wide energy selection, final 176-wide
  selection, omitted-final-mean restoration, gamma readout, and literal
  `129:33` outer blend remain unchanged;
- analytic baseline screens remain the incumbent width-232 screens;
- retained width-200 coordinates use the incumbent energy (`post_second`)
  ordering, not a new sensitivity or target score;
- reconstruction begins only after the layer-8 ReLU and is folded into
  layers 9 through 21.

Only two predictions are captured per row:

1. `control_s12_w216_mean`: exact current mean-fold control;
2. `conditional_s08_w200`: the single candidate above.

The Moore-Penrose inverse uses a fixed numerical cutoff
`rcond = 256 * eps(float32)`. This cutoff is declared before prediction and
exists only to discard modes below float32 numerical resolution.

## Rows and target separation

Use the already-opened development rows:

```text
Full       211, 223, 227, 239
Generated   13,  20,  24,  27
```

The capture may open only weights and seeds. It must atomically seal source,
preregistration, row identities, predictions, and array hashes before a
separate scorer opens targets. Generated scoring reports both observed and
noise-corrected pooled loss.

The Full0 control must associate with the frozen K162 current prediction at
relative RMSE at most `2e-6` and maximum absolute error at most `3.2e-5`.

## Frozen economics

Use R19 steady anchors:

```text
count       159,552,285,962
effective   181,151,096,762
```

The exact row-proportional replacement projection (before conditional-map
construction) is:

```text
candidate base count      140,455,363,706
base counted saving        19,096,922,256
```

Charge a conservative fixed `0.250B` count surcharge for all thirteen
conditional covariance solves, `B W_D` compositions, fold adjustments, and
associated reductions. The controlling projection is therefore:

```text
candidate count           140,705,363,706
candidate effective       162,304,174,506
effective reduction                10.404%
```

These are projections, not physical receipts.

## Fixed decision gate

The candidate advances this component screen only if all are true:

1. finite `(256,)` predictions on all eight rows;
2. pooled raw-MSE ratio to control `<=1.05` on Full;
3. noise-corrected pooled raw-MSE ratio to control `<=1.05` on Generated;
4. maximum observed row-loss ratio `<=1.50` in each family;
5. at least two of four rows improve or tie in each family; and
6. the conservative projected effective reduction remains `>=10%`.

At the current R19 remote-calibrated projection band `1.18e-7..1.22e-7`,
the gate-edge (`1.05x` raw) candidate projects to approximately
`1.11e-7..1.15e-7`. This arithmetic is a projection only.

Failure decisively kills this exact conditional-width schedule. No ridge,
width, start-layer, selection rule, inverse cutoff, or row scan is
authorized in R1.
