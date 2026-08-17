# K129 output-column Stein/Hermite calibration R1 — fast kill

Date: 2026-07-30.

Evidence label: **component** for the fixed Full4/Generated4 accuracy
falsifier and **projection** for cost. No FlopScope session, physical row,
estimator edit, package, upload, submission, or remote action occurred.

## Decision

**Kill this exact degree-two output-column calibration.**

The target-free capture associated exactly with R21, but the fixed
correction worsened both process-separated families:

| family | baseline raw MSE | candidate raw MSE | ratio |
|---|---:|---:|---:|
| Full4 | `3.0312853e-7` | `3.0449859e-7` | `1.004520` |
| Generated4, noise-corrected | `2.8977211e-7` | `2.9268753e-7` | `1.010061` |

The preregistered promotion gate was `<=0.970` on each family. Correction
versus target-residual cosines were negative on both (`-0.0590` Full and
`-0.1537` Generated), so this is not a near miss.

## Exact mathematical result

For `w ~ N(0, tau^2 I)` independent of hidden activation `H` and
`t(w)=E_H ReLU(w'H)`,

```text
E_w[t(w)]
  = tau / sqrt(2*pi) E||H||

E_w[w_i t(w)]
  = tau^2 / 2 E[H_i]

E_w[(w_i w_j - tau^2 delta_ij)t(w)]
  = tau^3 / sqrt(2*pi) E[H_i H_j / ||H||].
```

Thus, with `R=E[HH'/||H||]`, the exact second Wiener-chaos component is

```text
g2_R(w)
  = (w'Rw - tau^2 trace(R)) / (2*tau*sqrt(2*pi)).
```

These identities are valid and survive the negative experiment. What fails
is the proposed inference-time estimate of the unknown true `R`, not the
Stein derivation.

## What was available and what was tested

The actual K129/R21 replay contains enough state for a cheap acquisition:

- the complete layer-30 O0 hidden cloud;
- the selected final-176 coordinates and omitted-sample-mean restoration;
- the actual aligned final weight matrix;
- the already-paid analytic closure layer-30 mean/covariance;
- the exact R21 `lambda=0.0075` output.

The capture reinterpreted final-176 plus mean restoration as an effective
200-coordinate cloud and obtained its degree-two query without another
large final matmul. It shrank that query toward

```text
R_G = (Sigma_G + mu_G mu_G') /
      sqrt(trace(Sigma_G + mu_G mu_G'))
```

using the sole target-free across-basis Ledoit-Wolf scalar fixed in the
preregistration. The resulting shrinkage was automatically tiny,
`0.00150--0.00290`, and correction RMS was
`0.95e-5--2.27e-5`. Even at that conservative scale, its sign was wrong.

The exact target-free baseline association was bit-exact on both families.
All predictions, coefficients, and diagnostics were sealed before targets
were opened.

## Duplication boundary

- Degree zero is the already-killed output-ensemble moment anchor.
- Degree one is already represented by R21's analytic-proxy signed-mean
  correction and its broadly selected `lambda=0.0075`.
- Fitted equivariant endpoint/weight residuals already reversed on held
  Full and Generated.
- Input-sphere Stein controls are a different, already-killed lane.

R1 tested the only distinct lowest-order output-column Hermite channel. Do
not retry it with a scalar grid, fitted coefficient, alternate clipping, or
more rows. Reopening would require a genuinely better target-free estimator
of the true layer-30 radial matrix `R`, not a rescaling of this closure
discrepancy.

## Economics and artifacts

Incremental counted work was projected below `0.1B`, using the existing
final preactivation and analytic closure state. Accuracy killed the lane
before any physical accounting.

Authoritative artifacts:

- `PREREG_K129_STEIN_OUTPUT_CALIBRATION_R1_20260730.md`
- `capture_k129_stein_output_calibration_r1_targetfree_20260730.py`
- `k129_stein_output_calibration_r1_targetfree_20260730.npz`
- `k129_stein_output_calibration_r1_targetfree_20260730.json`
- `score_k129_stein_output_calibration_r1_postseal_20260730.py`
- `k129_stein_output_calibration_r1_postseal_score_20260730.json`
