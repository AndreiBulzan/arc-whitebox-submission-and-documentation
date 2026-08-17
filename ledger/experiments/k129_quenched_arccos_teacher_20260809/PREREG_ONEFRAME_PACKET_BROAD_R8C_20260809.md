# Preregistration: exact one-pair-per-line packet broad replay R8C

R8's fixed candidate zero improved the four-row component by 21.0%. R8B
cannot validate that result because every historical packet replicate averaged
four points, i.e. two candidate pairs per line.

R8C captures the exact missing object on the established packet Full8 plus
Generated8 bank: each of the eight nested packet candidates is paired only
with its own antipode, producing exactly 66,048 propagated rows per candidate
frame. Candidate index zero is frozen as primary before scoring. The other
seven indices diagnose frame variance and cannot be selected post hoc.

The average of the eight captured frame predictions must reproduce the
already sealed `m=8` finite packet-pool prediction to maximum absolute error
at most `2e-6`; canonical q0 must pass the same association gate.

Promotion gates:

- fixed candidate zero corrected raw ratio versus q0 `<=0.85`;
- both family ratios below one;
- at least 12/16 rows improve;
- at least 6/8 candidate frames improve pooled corrected MSE;
- median frame ratio `<=0.90`.

A pass licenses production graph/cost work because one frame has the same
row count as canonical K129. It remains **broad statistical retrospective**
on a reused bank, not a physical, Mini100, packaged, remote, or submitted
result.
