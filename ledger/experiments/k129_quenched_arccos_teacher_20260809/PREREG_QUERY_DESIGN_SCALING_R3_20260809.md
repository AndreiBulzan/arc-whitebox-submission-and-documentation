# Preregistration: arc-cosine target-query design and scaling R3

Date: 2026-08-09

## Prior-art and reopen boundary

Capsule searches covered `rotated Kerdock`, `Haar frame`, `arc-cosine`,
`target kernel mean`, and `synthetic final`.  The nearest controlled negatives
are the 4,096-row R2 Haar-frame result, the unrelated great-circle quadrature
failure, and the expert's earlier warning that the quenched last-layer teacher
lacked a target-side kernel mean.  R2 supplies the new observable that reopens
only that missing bridge: a 4,194,304-row teacher reduced corrected MSE by
92.63% on both captured families and every row.  The representation is no
longer conjectural; target-feature query error is the isolated bottleneck.

## Fixed experiment

Retain the exact same four rows, 512 Gaussian synthetic final directions,
candidate features, and actual candidate endpoints as R1/R2.  Capture two
independent target-query replicates for each of:

1. complete independent Haar orthogonal input frames, cumulatively at
   8, 16, 32, 64, and 128 frames (4,096--65,536 rows);
2. one globally Haar-rotated complete K129/MUB design per replicate, using a
   fixed target-blind basis permutation and cumulative prefixes of
   1, 2, 4, 8, 16, 32, 64, and 129 bases (512--66,048 rows).

Every direction is paired with its antipode and multiplied by the exact
Gaussian mean radius after propagation through the actual first 31 layers.
The rotated-Kerdock arm is materially different from independent Haar bases:
cross-basis inner products retain the MUB association structure and therefore
annihilate a stronger common set of low spherical harmonics.

R3 also applies a fixed two-replicate empirical-Bayes shrink in weight space.
For replicate weight deviations `d1,d2` from uniform, define

`T=max(dot(d1,d2),0)`, `V=norm(d1-d2)^2/4`, and `alpha=T/(T+V)`.

Return `uniform + alpha*(d1+d2)/2`.  This rule is target-free and fixed before
score opening.  Raw averaged weights remain the control.

## Target ceiling

At 31 full 256x256 layers, one target query costs approximately 4.06M dense
arithmetic operations before the synthetic readout.  Thus 4,096, 8,192, and
16,384 query rows are approximately 16.6B, 33.2B, and 66.4B for propagation.
Only designs showing at least 20% raw improvement by 8,192 rows or a credible
path to at least 50% by 16,384 rows can support the requested adjusted-score
frontier once candidate-feature and lifecycle costs are included.  Counts
above that are sample-complexity diagnostics, not presumed production points.

## Sealing and gates

The capture and fit open weights only.  Predictions are sealed before the
existing component targets are opened.

Continue a design only if a target-free fixed rule reaches at least 20% pooled
raw reduction with neither family reversing, or if its target-open diagnostic
at 16,384 rows retains at least half of the dense-teacher gain.  A 65k-row
failure below 35% kills that query family.  No physical row, Mini100 row,
FlopScope session, package, upload, submission, or remote action is authorized.
