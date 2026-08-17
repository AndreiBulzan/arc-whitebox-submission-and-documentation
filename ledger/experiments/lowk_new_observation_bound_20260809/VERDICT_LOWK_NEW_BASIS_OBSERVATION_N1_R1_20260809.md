# Verdict: small complete-basis moment pilots cannot recover the low-K teacher

Date: 2026-08-09

Status: **reject ordinary small generic/orthogonal-basis moment pilots**.

Evidence label: **component development diagnostic** on the already-open
public256 pair-teacher bank.  No official Mini100 row, FlopScope session,
physical row, package, upload, submission, or remote action occurred.

## Optimistic test

The simulation granted every auxiliary design the exact unavailable
high-sample final-preactivation mean and variance as its expectation.  It
then added only centered empirical complete-basis noise, bootstrapped from
the twelve isolated K12 basis units.  Each draw was also granted an
unavailable scalar fitted directly on the held 48 targets.

This is strictly easier than production: common-mode bias and scalar
selection were both removed by oracle information.

## Result

The K12 count is projected at `13.26677B`; one additional complete basis is
approximately `0.97505B`, so at most fourteen fit under the `27.2B` score
floor.

At fourteen auxiliary bases:

```text
projected total count                    26.91747 B
mean held-oracle raw MSE                 1.68362e-6
median held-oracle raw MSE               1.69160e-6
p90 held-oracle raw MSE                  1.97541e-6
frozen-train-beta held raw MSE           1.68661e-6
anchor RMS error from exact teacher      1.65375e-3
median correction-direction correlation  0.78037
required raw MSE                         <=1.0e-6
terminal preference                      <=8.0e-7
```

Even 128 auxiliary basis units, projected at `138.07B`, leave mean
held-oracle raw MSE `9.426e-7`.  The exact noise-free teacher hybrid itself
reaches `7.814e-7`, confirming that the capacity exists only after the
design-wide moment has effectively converged.

## Interpretation

The obstruction is no longer merely uncorrected finite-sample bias.  An
ordinary new sample of complete bases pays nearly one billion operations per
unit, while its moment noise decays far too slowly.  Small generic point
pilots should be worse than complete orthogonal bases unless they exploit a
new deterministic relationship.

Do not spend an official Mini100 capture on a small generic/orthogonal-basis
moment audit.  Reopen only with:

- a deterministic collective identity with no Monte-Carlo noise;
- a specialized design with independently demonstrated order-of-magnitude
  lower projected moment noise;
- or a non-moment observable whose variance/cost curve is materially better.

This is an optimistic empirical-noise falsifier, not a theorem against every
possible cubature family.

## Evidence

- precheck:
  `PRECHECK_LOWK_NEW_BASIS_OBSERVATION_N1_R1_20260809.md`
- source:
  `run_lowk_new_basis_observation_n1_r1_20260809.py`
- receipt:
  `runtime/artifacts/lowk_new_basis_observation_n1_r1_posthoc_20260809.json`
  (`9c710786d550c5c65b7ed14507b14e8c340a3f3e55022cdbd10c972dbdcf3e22`)

