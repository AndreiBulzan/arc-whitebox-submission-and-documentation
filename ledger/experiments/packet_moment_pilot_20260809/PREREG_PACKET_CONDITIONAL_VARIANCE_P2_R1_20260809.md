# Packet conditional-variance pilot P2 R1 preregistration

Date: 2026-08-09

Evidence class: target-free component oracle. No benchmark target,
FlopScope/physical run, package, upload, or submission.

## Mechanism

V1 showed that the exact average conditional marginal preactivation variance
per neuron/layer preserves 97.9% of the centre-averaged packet correction. P1
failed because it recovered this small variance as an unstable difference of a
total moment and an approximate conditional-mean square.

P2 estimates the V1 statistic directly. For every line in `k` complete bases,
draw two independent Gaussian packet treatments. Propagate both antipodal
pairs through the actual supplied network. For each oriented centre and layer,
two independent preactivations `z1,z2` give

`E[(z1-z2)^2 / 2 | centre] = Var(z | centre)`.

Average this positive quantity across the selected oriented centres to obtain
one 256-vector `v_l`. Then propagate conditional means for all 66,048 K129
centres using `E[ReLU(N(m_l,v_l))]`. Pilot output vectors are never averaged
into the estimator.

## Frozen experiment

- Sealed Full8/Generated8 64-replicate packet R2 population.
- Nested uniform Kerdock-basis supports
  `k = 2,4,8,12,16,24,32,64,129`.
- Eight independent support/noise replicates, source-frozen seeds.
- Four pilot rows per selected line: two independent treatments for `+u` and
  their valid antipodal treatments for `-u` (`4*k*256` rows).
- All 32 actual layers, float32 products, TF32 disabled, exact univariate
  Gaussian ReLU means, selected outputs 1,2,4,8,16,24,32.
- No target-based support, seed, scale, shrinkage, or layer choice.

## Gates

Use replicate-noise-corrected packet correction metrics. Primary capacity pass:
some `k <= 16` must have

- median final pooled fidelity >= 0.70 and cosine >= 0.85;
- median Full and Generated fidelity >= 0.60 each;
- at least 6/8 replicates with pooled fidelity >= 0.50;
- median layer-8 pooled fidelity >= 0.60;
- all predictions and variance estimates finite.

Fallback pass applies the same requirements at `k <= 32`. `k=64,129` are
diagnostic capacity ceilings. Passing licenses post-seal target scoring and a
meter-aware integration design. Failure at k=129 kills a two-treatment sample
variance pilot for this recurrence, but not deterministic conditional-variance
cubature, more than two treatments, or a variance control variate.
