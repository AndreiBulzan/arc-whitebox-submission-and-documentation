# K129 mixed64/D6 direction-240 preregistration

Date: 2026-08-03

Evidence target: **broad statistical** for accuracy if the sealed candidate is
subsequently scored; **projection** for compute until a physical R35 exists.

## Prior-art preflight

Capsule searches covered `direction`, `thinning`, `192`, `160`, `128`,
`240`, `literal mixed64`, and the D6 capture/score lineage.  The literal
pilot tested 256/192/160/128 directions, and `STATUS.json` closes the latter
three.  No 240-direction literal-D6 capture or verdict exists.  This test is
therefore the untested near-lossless edge, not a replay of a killed variant.

## Fixed candidate

- Keep the frozen 64 `(frame, basis)` slots and ridge `1.0`.
- Keep the q0 D6 anchor unchanged.
- For each slot, retain the deterministic first 240 rows of the existing
  affine-permutation direction schedule used by the literal thinning pilot.
- Refit the 64 coefficients only to the target-free four-frame teacher using
  the existing family-balanced four-fold procedure.
- Capture all Full100 and Generated128 predictions before opening targets.

No target may select the direction count, schedule, ridge, support, or fit.

## Promotion rule

After sealing, score the one fixed candidate.  Promote only if its controlling
Generated128 adjusted projection, repriced from the R34 physical checkpoint
with the conservative old linear direction-price model, is below R34's
`1.124471608e-7`.  Strong promotion requires `<=1.10e-7`.  Otherwise kill it
without a FlopScope implementation.

