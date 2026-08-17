# K129 mixed/D6 geometry-aligned support R19

Date: 2026-08-03

Evidence is **component** before broad scoring and **projection** for compute
until a physical successor exists.

## Prior-art preflight

Capsule searches covered `support 40`, `support 44`, `support 47`, `mixed48`,
`literal48`, and `6016`.  Earlier work tested a different matched48 feature,
the exact R8 lineage tested only `48,52,56,60,64`, and direction thinning
tested `128,160,192,240,256` rows per basis.  No exact R8 nested support below
48 or two-by-6016 geometry verdict exists.

## Fixed screen

- Reuse the exact R8 broad target-free features, q0-core64 anchor, teacher,
  family-balanced four-fold split, OMP ordering, and ridge `1.0`.
- Seal support sizes exactly `40,42,44,46,47,48` before opening targets.
- Price them by the measured R34-to-R35 per-basis slope.  This deliberately
  ignores the residual/request benefit of dropping the third chunk and is
  conservative for size 47 and below.
- After sealing, report every size on Full100 and Generated128.  Promote only
  a size that improves the central adjusted projection over physical R35 in
  both families; ties favor the larger support.

No package, upload, or submission is authorized.
