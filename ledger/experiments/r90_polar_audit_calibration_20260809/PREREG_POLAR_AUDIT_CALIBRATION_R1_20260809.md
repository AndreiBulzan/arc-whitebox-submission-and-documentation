# Preregistration: polar-audit calibration R1

Date: 2026-08-09

## Motivation and scope

The E1 output-coordinate cross-fit proved that a stable shared weighting of
the 129 standard Kerdock basis endpoints has large held-output capacity
(81.82% pooled raw reduction under a 2/129 cap).  Analytic checkpoint totals
failed to reveal that direction.  This experiment supplies a genuinely
independent target-free observation: a small number of complete bases from
the already frozen `polar_q0_right_d2` frame.

This is not the killed packet-source audit.  Each polar basis endpoint is a
direct 256-vector cubature estimate of the desired final expectation; it does
not require cancellation against a large radial packet source.  The complete
polar-frame 50/50 blend has already shown about 47% raw reduction on Full,
Generated, and official Mini populations, but a complete second frame is too
expensive.  The question is whether a few polar bases can identify a useful
standard-basis correction.

All endpoint captures and candidate construction are target-free.  Targets
may be opened only after the candidate archive is hash-sealed.  No FlopScope
physical row, package, upload, submission, or remote action is authorized.

## Broad target-free endpoint capture

Use the frozen standard endpoint corpus rows (Full100 rows 7,17,...,997 and
Generated128 rows 0,...,127).  For the same rows, construct exactly the
selected `polar_q0_right_d2` orthogonal frame and capture all 129 repaired
per-basis endpoints.  Require:

- mean of endpoints equals the captured polar prediction to 1e-10;
- overlapping rows agree with the previously sealed broad single-polar
  prediction to 5e-5.  The historical broad source constructs the same polar
  frame through an independently spelled two-layer Jacobian; a target-free
  preflight measured maximum float32 frame-entry delta 1.49e-8, so this is a
  compatibility check rather than a bit-identity claim.  A subsequent
  target-free one-row end-to-end diagnostic measured 1.04e-6 maximum modern
  versus historical prediction delta and 7.57e-7 numerical replay delta even
  for the historical constructor itself;
- standard endpoint means retain their existing association;
- targets are never mapped.

Official Mini100 uses the already sealed standard and selected-polar
per-basis endpoint archives; no recapture is needed.

## Frozen audit and calibration grid

Use one fixed nested permutation of polar basis labels, seed 2026080902.
Audit counts are `k = 2, 4, 6, 8, 12, 16, 24`.

For each k, form the mean of the first k selected polar endpoints.  Seal two
estimator families:

1. `direct`: `q0 + rho * (audit_mean - q0)`.
2. `projected_crossfit`: fit a shared standard-basis ridge ray to the audit
   mean on even output coordinates and apply it only to odd coordinates;
   fit on odd and apply only to even.  This prevents same-output audit noise
   from being fitted and scored on itself.  The ray is centered at uniform
   weights and clipped to nonnegative weights, ESS >= 32, and max weight
   2/129.  Its output correction is subsequently multiplied by rho.

For the projected family use identity and floored basis-standard-deviation
scales and ridge multipliers
`1e-4, 3e-4, 1e-3, 3e-3, 1e-2, 3e-2, 1e-1, 3e-1, 1, 3, 10`.

Shrink factors are
`rho = 0.025, 0.05, 0.1, 0.2, 0.35, 0.5, 0.75, 1.0`.

The complete prediction grid for Full100, Generated128, and Mini100 must be
sealed before any targets are opened.

## Selection and gates

After sealing, choose one global cell by minimizing the worse projected
adjusted-score ratio on Full100 and Generated128.  The projection prices one
extra polar basis at 1.0B effective operations and uses the 139.365B remote
R87 anchor; both the raw and projected-adjusted ratios must be reported.
Apply the selected cell unchanged to official Mini100.

- Strong promote: projected adjusted ratio <= 0.70 on both broad families
  and official Mini100 (enough to approach the 8e-8 terminal target from the
  1.1433e-7 anchor), with no material tail instability.
- Waypoint promote: ratio <= 0.87 on all three populations (roughly 1e-7).
- Weak lead: same-sign gain on all three and ratio <= 0.95 pooled; investigate
  support design or a second independent audit.
- Kill: sign reversal on a population or no score-positive cell after the
  complete preregistered grid.

Any physical implementation requires a separately hash-pinned sparse-polar
runner and benchmark-lane receipt.  This component test cannot establish its
actual compute cost.
