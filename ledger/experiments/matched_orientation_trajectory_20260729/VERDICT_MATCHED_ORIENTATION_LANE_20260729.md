# Matched-orientation trajectory lane — final verdict

Date: 2026-07-29

## Verdict

**Kill this lane.  Do not implement a runtime successor.**

The work produced a strong target-free proxy result, froze its cheapest
spelling, and then failed the preregistered exact signed-target gate.  The
fully within-MLP kernel/Nyström spelling also failed before target access.

No FlopScope session, physical row, package, upload, submission, network, or
remote action was used.

## What survived the cheap screens

The complete atlas has axes:

```text
observer_basis_postmean
  (MLP, orientation=8, checkpoint={L4,L8}, basis=129, neuron=256)
```

R2 initially transposed orientation and checkpoint.  It is explicitly
invalid and was superseded before any claim or promotion.

The axis-correct R3/R4 model used only:

- O0 and O1;
- the 17 paid O1 final per-basis signed means;
- the same-label O0 values and complete paid O0 S109 reduction; and
- one shared 40-input ridge map over all output neurons.

L4/L8 added nothing; the endpoint-only matched spelling was best:

```text
O1 S109 proxy omission MSE ratio
                         Full held    Generated held
endpoint matched            0.21161         0.21434
row-ratio p95               0.53875         0.55276
rows improved               100.0%           98.4%
```

O1-only endpoint/trajectory features were `~1.0x`, so the measured proxy
signal genuinely came from the matched O0/O1 relationship.

## Decisive target result

R5 froze the 40-dimensional endpoint model, `ridge alpha=100`, Full-train
coefficients, S17 support, S109 proxy reference, and literal `129:17` blend
before exact signed-target access.

```text
exact signed-final-preactivation MSE ratio
                         Full held    Generated held
R5 candidate                0.96979         1.01128
row-ratio p95               1.30524         1.41425
rows improved                54.0%           43.8%

half-shift final-ReLU projection
                             0.99391         1.00062 noise-corrected
```

Every preregistered signed gate failed.  The final-ReLU values are only a
projection because exact pre-ReLU replay was intentionally not purchased
after the signed kill.

The mismatch is structural, not lack of proxy fit.  The predicted and exact
S109 correction vectors correlate about `0.893` in both families, yet their
correlation with the actual signed-target residual is:

```text
                         Full held    Generated held
predicted correction        0.199           0.119
exact S109 correction       0.445           0.390
```

The exact S109 oracle would improve signed MSE to `0.8075x` Full and
`0.8491x` Generated, but the proxy-prediction error is primarily in the
small target-aligned component and erases that gain.  Improving an
orientation population proxy is therefore not equivalent to improving the
network integral.

## Fully within-MLP test

R6 used no cross-MLP coefficients.  For each MLP it fit a centered
kernel-ridge/GREG map from complete O0 endpoint or O0 L4/L8/endpoint
geometry to the 17 observed O1 endpoints, then predicted the S109 mean.

Best target-free selected result:

```text
                         Full dev     Full held    Generated held
proxy MSE ratio           1.00025       0.99872          1.00432
row-ratio p95             1.01406       1.02472          1.03527
```

That closes the requested per-MLP Nyström/Procrustes class on this
observable.

## Economics boundary

Had R5 transferred, its endpoint-only 40-input shared map would add well
under `0.001B` ordinary arithmetic, fewer than about 30 calls, and less than
`0.3 MiB` state if it reused the gamma-readout per-basis means.  R6's more
expensive dynamic kernel was projected below `0.005B`, at most 40 calls, and
about `266 KiB` state.  These are **projections**, not receipts.  Accuracy
failed before price mattered.

## Evidence

- R3 axis-correct target-free proxy: **component**
- R4 endpoint ablation: **component**
- R5 fixed signed predictions on Full100/Generated64: **broad statistical**
- R5 half-shift final-ReLU arithmetic: **projection**
- R6 within-MLP kernel: **component**

Authoritative files:

- `trajectory_residual_r3_axisfixed_targetfree_20260729.json`
- `matched_signal_r4_ablation_targetfree_20260729.json`
- `endpoint_matched_r5_capture_targetfree_20260729.json`
- `endpoint_matched_r5_predictions_targetfree_20260729.npz`
- `endpoint_matched_r5_score_20260729.json`
- `within_mlp_kernel_r6_targetfree_20260729.json`

This lane may be reopened only with an observable that predicts the
target-aligned final signed innovation directly—not another orientation
population, support, endpoint, trajectory, kernel, or shrinkage variant.
