# Dual-frame midpoint shared suffix R3 — polar association tolerance

Date: 2026-08-05

R2's target-free independent association check reproduced q0 bit-exactly but
reproduced the known near-singular Euclidean-polar arm only to maximum
absolute differences `4.90e-7` (Full) and `8.72e-7` (Generated).  The scorer
stopped before opening targets.

R3 freezes an association tolerance of `2e-6`, still over two orders of
magnitude below the estimator's coordinate error scale.  It reuses the
already sealed target-free R2 capture and changes no prediction, checkpoint,
economic model, or accuracy gate.  No target value was observed in selecting
this numerical reproducibility tolerance.

