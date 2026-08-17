# K146 cold-prefix census R1

Date: 2026-07-29

This is a target-free, offline GPU component screen prompted by the public
Phase-I write-up for submission 319341.  It does not run FlopScope, an
estimator package, or a physical benchmark.

The screen replays the frozen K146 numerical graph on two Full and two
Generated weight draws.  At every production late-layer energy-selection
site (layers 24--30, selected width 192) and the final site (selected width
176), it records:

1. per-column firing rates of the already-selected activation;
2. the largest cold prefix whose summed firing rate is at most three,
   subject to a 96-column hot-block floor;
3. exact per-row cold-prefix support counts; and
4. lower-bound and implementation-aware FlopScope 0.9.1 count projections
   for the public `(0,1,2,4,8,remainder)` bucket ladder.

The selected rows are fixed before execution:

- Full: 409, 419
- Generated: 7, 11

No target, score, timing, package, upload, or remote action is permitted.
The transfer is killed without an accuracy run unless its implementation-
aware projection saves at least 8 billion counted operations on the K146
tail.  A projection is not a receipt.
