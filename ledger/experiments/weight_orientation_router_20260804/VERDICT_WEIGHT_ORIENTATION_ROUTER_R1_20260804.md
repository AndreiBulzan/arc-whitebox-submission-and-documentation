# Verdict — weight-only orientation router R1

Date: 2026-08-04

Evidence: **component**.  All compute and adjusted values are
**projections**; no physical row or remote action was performed.

## Result

Kill this exact route.  Across every nested support `m = 33, 49, 65, 81,
97, 109`, the frozen 2,496 supplied-weight features failed the reciprocal
Full100/Generated64 transfer gate.

- At `m=109`, the best cross-family selector was `1.051x` the best fixed
  pair on Generated64 and `1.005x` on Full100.  Oracle-gap recovery was
  negative in both directions.
- The only apparent gains at any support were 2--4%, recovered at most 7%
  of the pair oracle gap, and did not persist across supports/families.
- More fundamentally, even the label-using signed-preactivation oracle has
  projected adjusted values above `1.36e-7` at every tested support.  Thus
  this two-orientation spelling lacks a sub-`9e-8` economic ceiling before
  selector error or final-ReLU approximation is considered.

This result complements, rather than repeats, the earlier pilot selector and
weight-feature support router.  It closes routing the frozen orientation pair
directly from the cached supplied-weight summaries.  Do not spend propagation
or physical-row time on it.

Exact result: `weight_orientation_router_r1_screen_20260804.json`.
