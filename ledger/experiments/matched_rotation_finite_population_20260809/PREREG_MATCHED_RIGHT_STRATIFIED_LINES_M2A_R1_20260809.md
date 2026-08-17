# Matched right-Gram stratified-line correction M2A R1

Date: 2026-08-09

M2's first polar implementation was stopped before ten rows and before any
artifact or target access because materializing four float64 node clouds made
the capture CPU-bound.  M2A preserves the new sampling design while using the
complete right-Gram frame and lean float32 sufficient-statistic accumulation.
M1 established nearly identical complete-frame raw capacity for right-Gram
and polar; no target result from M2 selected this change.

The estimator samples a few signed rows or antipodal lines inside every one
of the 129 bases.  This integrates the between-basis component exactly and
leaves only within-basis finite-population variance.

Frozen inputs:

- q0 endpoints: `f67aff76f3696a9d142527fbacf996bcd35dc3c128e8270550eba3f1c05ab4b0`;
- right-Gram endpoints: `0db24b1e0cc1efefd917dff702920d49ce55a9ae7328ac0fb8c7a5e0d34f2809`;
- Full100 and Generated128 row manifests from those seals;
- no targets during capture;
- ordinary CUDA under `runtime/.benchmark_lane.lock`.

The post-seal designs, grids, economics, and gates are unchanged from M2:

- paired and independent-sign `l={1,2,4,8,12,16,24,32,48,64,96,128,192,256}`;
- signed `m=2l`, capped at 512;
- one pooled scalar `alpha in [0,1]`;
- cost `5B + 0.999B * row_equivalent_complete_bases`;
- require projected adjusted `<=8e-8` and raw ratio `<=0.70` on both broad
  families with no more than 32 complete-basis-equivalents.

The float32 node cloud is used only to estimate population covariance traces.
The complete q0/right predictions and their difference remain the sealed
float64 endpoint means.  Require internal node-mean association within
`2e-5` and sealed complete-prediction association within `2e-10`.

This authorizes no FlopScope, physical row, package, upload, submission, or
remote action.

