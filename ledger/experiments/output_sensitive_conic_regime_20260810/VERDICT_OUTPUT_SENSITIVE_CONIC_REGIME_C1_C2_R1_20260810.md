# Output-sensitive conic-regime C1/C2 verdict

Status: **kill the tested small affine-atlas spelling**.

Evidence label: **component** teacher-capacity replay on 16 official Mini100
weights (public rows 0--7 and disjoint holdout rows 50--57).  No FlopScope
session, physical row, package, upload, submission, or remote action occurred.

## What was tested

The complete standard O0 Kerdock population supplies 66,048 exact input
directions per network.  At post-ReLU layers 24 and 28, the exact remaining
tail supplied:

- homogeneous final-output signatures;
- frozen sketches of all downstream gate masks.

Deterministic two-restart partitions with `K=8,16,32,64,128` produced
weighted checkpoint means.  Three compact tail readings were tested:

1. ordinary exact ReLU propagation of each regime mean;
2. the fixed complete gate map of the regime medoid;
3. the coordinatewise majority fixed gate map.

The capture retained no regime's average final output.  A method succeeds
only if one compact checkpoint mean plus one tail map reproduces the many
member outputs.

## Result

No cell passed any representation gate.

- The best score-compatible (`K<=64`) C1 cell was layer 28, `K=64`:
  pooled candidate-versus-complete-K129 MSE `1.9498544e-4`, versus the
  preregistered `2.5e-8` ceiling—a miss by `7,799x`.
- The strongest diagnostic C1 cell, layer 28 `K=128`, remained at
  `1.5903549e-4`.
- The best fixed-map score-compatible cell was layer 28, `K=64`, majority
  gates: `2.0322080e-4`, a miss by `8,129x`.
- Medoid maps were worse.  Majority maps were comparable to, but did not
  improve on, ordinary centroid ReLU propagation.
- Direct target MSE ratios were hundreds to thousands of times the complete
  K129 baseline.  Public-selected safe scalar blends chose coefficients only
  around `1e-3`; the transferable layer-28 effects were about a tenth of a
  percent, not a useful tail replacement.

The complete K129 baseline itself was healthy on this fixed Mini16 screen:
pooled raw MSE `2.3595355e-7` (`1.4749633e-7` public and `3.2441076e-7`
holdout).  The failure is therefore the compact tail representation, not a
broken input design.

Across `K=8..128`, the layer-28 centroid representation error decays only
approximately as `K^-0.31`.  Extrapolating that short ladder to the gate
would require an absurd number of regimes; this extrapolation is diagnostic,
not a lower bound.

## Interpretation

The useful late object is not a small collection of affine activation
regions.  Even after partitioning with exact downstream information, the
within-regime correlation between checkpoint state and boundary/gate map is
orders of magnitude larger than the estimator's entire error budget.

This closes:

- 8--64 output/gate-signature regimes at layers 24 and 28;
- one centroid plus ordinary ReLU tail per regime;
- one medoid or majority fixed affine map per regime.

It does **not** prove impossible:

- analytic integration of boundary flux inside each regime;
- a representation carrying state--gate cross-moments rather than only a
  mean and one map;
- an exact global identity that bypasses region partitioning;
- a radically different partition with a demonstrated error bound.

The user's functional-compilation intuition was worth testing, but the
minimal sufficient regime statistic is materially richer than “probability,
mean state, one local linear rule.”  Do not scale this spelling to Mini100 or
production.

## Evidence

- `runtime/artifacts/output_sensitive_conic_regime_c1_r1_targetfree_20260810.npz`
- `runtime/artifacts/output_sensitive_conic_regime_c1_r1_postseal_20260810.json`
- `runtime/artifacts/output_sensitive_conic_affine_c2_r1_targetfree_20260810.npz`
- `runtime/artifacts/output_sensitive_conic_affine_c2_r1_postseal_20260810.json`

