# Stable-shadow closure R1 — preregistration

Date: 2026-08-06

Evidence class: offline **component** research. This experiment performs no
FlopScope physical run, packaging, upload, submission, or remote action.

## Prior-art boundary

The capsule search was completed before implementation. The following exact
families remain closed and are not being renamed or retested here:

- TAP/Onsager and ordinary one-site or query-conditioned cavity corrections;
- Gaussian respawn, active-subspace mixtures, Price/Hermite order sweeps, and
  Edgeworth marginal readouts;
- free recurrent learned moment closures whose predicted mean/covariance is
  fed into the next layer;
- global harmonic decoders, geodesic/great-circle rules, naive mixed-frame
  allocation, and signed quartic/Schur frame selectors.

The new distinction is structural: a deterministic Price-order-4 Gaussian
closure remains immutable for all 32 layers. A bounded, permutation-
equivariant neural state is transported beside it using normalized visible
weights. Its prediction is an additive readout only; it never changes the
anchor mean, variance, covariance, or any later feature. Consequently a bad
learned correction can be set to zero without destabilizing the anchor.

## Hypothesis and economics

The analytic closure supplies stable low-order state. A small shadow message
system may learn the connected finite-width error that this state omits.
Projected deployment work is capped at 5.0B charged operations, including the
full covariance anchor. This is below the 27.2B score-multiplier floor.

At that floor, final raw MSE <= 1.0e-6 implies adjusted score <= 1.0e-7.
This is therefore the only promotion threshold; merely improving the roughly
7e-5 analytic anchor is insufficient.

## Data firewall

- Training: Full indices 0..699.
- Development/model selection: Full indices 700..799.
- Locked test: Full indices 800..999 and Generated128.
- The target-free anchor cache for 0..799 is sealed before any training run.
- Full800..999 and Generated targets remain unopened until one fixed model is
  selected from the development trajectory.
- No Mini100 or leaderboard values participate in selection.

## Fixed first spelling

- Price covariance truncation order: 4, exact marginal variances.
- Shadow width: 32 channels.
- Shared pointwise cell width: 128.
- Messages per layer: signed normalized-weight transport and squared-weight
  transport, plus a global pooled state and lawful analytic scalar features.
- Correction bound: 0.10 times analytic preactivation RMS via `tanh`.
- Layer zero correction is fixed to zero because its Gaussian marginal is
  exact.
- Training seed: 2026080617; AdamW, learning rate 3e-4, weight decay 1e-5.
- Development checkpoints: 50, 100, 200, 400, and 800 updates.

## Kill/promote rules

Fast kill: after 200 updates, stop unless the 100-row development raw MSE is
<= 5.0e-6. Continue to 800 only if that gate passes.

Lock the checkpoint with lowest 100-row development raw MSE. Then and only
then open the two test families.

Promote only if all of the following hold unchanged:

1. Full800..999 final raw MSE <= 1.0e-6;
2. Generated128 noise-corrected final raw MSE <= 1.0e-6;
3. finite predictions and zero bound violations;
4. static deployment upper bound <= 5.0B charged operations.

Failure closes this architecture spelling. It does not license width, seed,
loss, or coefficient sweeps on the locked families.

