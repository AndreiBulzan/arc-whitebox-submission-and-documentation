# K129 sparse-D6 R6 official Mini100 transfer gate

Date: 2026-08-05

Evidence before execution: **component** for the target-free capture and
**projection** for deployment economics.  The official Mini100 is now a
blocking regression bank, not a virgin holdout.  No FlopScope row, package,
upload, or submission is authorized here.

## Prior-art boundary

The rule is not newly selected.  It is the frozen 64-atom `qmean`, ridge-1
depth-6 sidecar in
`k129_fourframe_h2_mlmc_20260802/direct_sparse_d6_scout_r6_targetfree_20260802.npz`.
That rule was selected without challenge targets to reconstruct the complete
four-frame endpoint, then scored post-seal on Full100 and Generated128.  It
gave raw ratios `0.804922` and `0.858124`, respectively.  It was killed only
because its `1.129e-7` projection used the older `1.20044e-7` remote anchor.
It has never been scored on official Mini100.

Direction thinning, equal-row multiframe splitting, later R34/R36 supports,
and literal sparse-polar grids are different frozen rules and cannot answer
this transfer question.  No support, coefficient, ridge, frame, basis,
endpoint scale, or readout parameter may be changed in this gate.

## Fixed prediction

For each official Mini100 network:

1. compute the ordinary complete q0 K129 endpoint;
2. compute the q0 depth-6 per-basis diagonal-continuation proxy and take its
   literal mean over all 129 bases;
3. compute only the frozen 64 alternate `(frame, basis)` depth-6 proxies; and
4. add the frozen weighted difference between those alternate proxies and the
   q0 proxy mean to the q0 endpoint.

Before opening Mini100 targets, associate the spelling on Full row 7 to the
already sealed R6 Full prediction with maximum absolute displacement at most
`5e-4`, seal all 100 predictions, their hash, and the source hash.

## Blocking gates

Promote toward a current-meter implementation only if all of the following
hold on the complete official Mini100:

- finite predictions;
- raw-MSE ratio to q0 `<= 0.87`;
- at least `60/100` rows improve;
- paired bootstrap 95% upper ratio `< 1.0`; and
- using a deliberately conservative deployment effective-compute ratio of
  `1.10`, the R78-anchored central projection is `<= 1.10e-7`.

The conservative score projection is

`1.14946e-7 * raw_ratio * 1.10`.

Failure closes this exact R6 support without coefficient retuning on
Mini100.  Passing authorizes only a capsule-native implementation and exact
physical pricing, not an upload.
