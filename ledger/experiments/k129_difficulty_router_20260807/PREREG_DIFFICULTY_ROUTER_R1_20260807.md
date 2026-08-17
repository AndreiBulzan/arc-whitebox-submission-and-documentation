# Difficulty-regressor per-row floor routing — R1 preregistration

Date: 2026-08-07. User directive: full-force execution toward ≤1.10e-7
validated on official Mini100.

Evidence by stage: **component** (label pipeline validation), **broad
statistical** (regressor gates on never-trained challenge families),
**projection** (routing simulations), exact packaged Mini100 gate before
any submission discussion. No stage builds/uploads/submits without the
standing capsule authorization rules.

## Mechanism

The challenge score is per-row: `s_m = raw_m × max(0.1, C_m/272B)`
(CHALLENGE.md; verified per-row in remote payloads). A target-blind
estimator routes each row to either the full arm (~139.6B, q≈0.513) or a
literal m-basis floor arm (~27–38B, q≈0.10–0.14) using a deterministic
scalar difficulty score computed from the weights (≤0.1B). Measured
capacity (Full100): oracle 0.92e-7 at m=32 with 39% routed; requirement
curve: held corr ≥0.6–0.7 with per-row gain clears 1.10e-7; corr ≤0.5 is
neutral (graceful degradation).

## Prior-art preflight (blocking)

Queries: routing, adaptive compute, per-row budget, difficulty, error
prediction, weight regression, cross-frame agreement. Nearest artifacts:
the killed per-neuron corrector lanes (trajectory student R2, gate-state
F0), the floor-regime P1/P2 (all-rows-at-floor; killed for lacking a
uniform cheap engine), and the eight failed selector attempts recorded in
`runtime/artifacts/k129_perrow_floor_routing_selector_requirement_r2_20260807.json`.
No prior work conditions graph size on the row, trains a scalar
difficulty target, or uses the cross-frame-agreement label. Outcome:
**materially new mechanism + materially new observable.**

Target-ceiling: oracle 0.92e-7 ≫ the 1.10e-7 requirement at corr 0.6.
PASS.

## Data

- Tranche 1: whestbench 10k-test **test500** (weights + N=1e9 reference
  final post-means; virgin family, generator matches the challenge spec).
- Tranches 2+: self-generated He(256×32) networks with antithetic-MC
  truth (N ≥ 1.6e7 pairs; label-noise offset ≤ ~2e-8 documented), plus
  target-free cross-frame agreement labels (unlimited).
- Held gates: legacy Full100 + Generated128 (never trained on), then a
  disjoint official-full sample, then exact Mini100.

Labels per net: q0 per-basis endpoints and prediction; right-frame
(canonical right-Gram) endpoints and prediction; raw_full; raw_m for
m ∈ {16,24,32,48}; per-row routing gain; debiased cross-frame agreement.

## Gates

- **G0 (pipeline)**: q0/right captures reproduce the sealed July caches
  bit-level on ≥3 Full100 rows; test500 label sanity (raw_full mean
  within family expectations).
- **G1 (signal)**: held-family corr(predicted difficulty, log raw_full)
  ≥0.6 pooled AND ≥0.5 in each of Full100 and Generated128 separately;
  same thresholds against log routing-gain.
- **G2 (routing sim)**: simulated one-shot routing on held families
  improves score ≥3.5% with kept-row ratio p95 ≤ break-even.
- **G3**: repeat G1/G2 on a never-trained official-full disjoint sample.
- **G4 (deployment)**: literal floor-arm receipt ≤27.2B effective
  including residual; router formula ≤0.1B; exact routing package through
  archive gate + five-lane exact Mini100 paired against the R87 capture,
  requiring the prespecified bootstrap probabilities before any
  submission request.

## Kill conditions

Held corr <0.45 after the full ladder (spectral/norm/alignment/closure
features → GBT/MLP → equivariant encoder) → park the routing lane,
document, revert to engineering baseline. Any evidence of target leakage
into features or labels → invalidate the stage. No tuning on Mini100;
Mini100 appears only at G4 as the exact-package gate.
