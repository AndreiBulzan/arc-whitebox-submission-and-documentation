# Official Mini100 static four-frame coreset R3

Date: 2026-08-04

This is a target-free replay of the already frozen static K129 endpoint
coreset in `k129_fourframe_static_coreset_20260802`.  That rule was selected
without challenge targets and contains 72 q0-frame atoms and 57 right-frame
atoms with positive affine weights summing to one.  It was previously killed
because its reconstruction error was 2.8--3.3 times the complete-four-frame
ideal.  The new question is narrower and was not answered by that gate:
whether it still improves materially over q0 on the independent official
Mini100 bank at a nominal 129-atom support.

For each Mini100 MLP, run the two complete frames only to expose the exact
full-frame-repaired per-basis endpoints, then apply the frozen support and
weights.  Predictions must be sealed before opening Mini100 targets.

Continue implementation research only if raw MSE is at most 0.85 times q0
and at least 60% of rows improve.  This remains capacity evidence: reproducing
full-frame endpoint states from only the selected atoms has not been proved,
and the diagnostic capture itself computes both complete frames.  It is not
a package, adjusted-score, physical, upload, or submission claim.
