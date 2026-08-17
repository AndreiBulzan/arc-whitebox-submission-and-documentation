# Preregistration: one-frame packet replicate replay R8B

The R8 four-row component showed a 21.0% raw reduction from the frozen
`fixed0` packet cloud before any arc correction. The sealed true-packet R2
archive already contains 64 independent complete one-pair-per-line packet
frames on disjoint Full8 plus Generated8 rows.

This post-seal retrospective fixes replicate index zero as the primary rule;
it may not select the best replicate. All 64 replicate ratios are reported
only to measure the mechanism's frame variance and probability of improvement.
Generated label-noise MSE is subtracted in the broad corrected summary, with
the original uncorrected result retained for association.

Promotion requires fixed replicate zero to improve both families and pooled
corrected ratio at most `0.85`, plus at least 75% of all independent frames
improving pooled corrected MSE. Otherwise the R8 fixed0 result is treated as a
four-row lucky frame and the uncorrected one-frame packet estimator is killed.

This uses already-opened historical targets and is labeled **broad
statistical retrospective**. It authorizes no physical row, Mini100 run,
package, upload, submission, or remote action.
