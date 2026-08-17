# Schur-harmonic angle-B independent broad transfer R3

Date: 2026-08-05

Evidence sought: target-free **component** capture followed by **broad
statistical** scoring on the already fixed Full100 and Generated128 rows.
No FlopScope row, package, upload, or submission is licensed by this test.

## Prior-work boundary

The fixed Schur `angle_b` complete frame improved official Mini100 raw MSE by
`3.181129%`, but its original Full/Generated pilot used only four rows per
family.  The later harmonic-balance variants failed and do not alter this
fixed rule.  No artifact evaluates unchanged `angle_b` over the independent
Full100/Generated128 rows associated with the sealed K129 q0 endpoint bank.

## Frozen rule and gate

For every network, construct `angle_b` exactly as in R1: take the real Schur
blocks of the q0-to-polar relative rotation, sort blocks by unsigned angle,
and use the complement of the alternating block set.  Propagate one complete
K129 frame.  The row sets are read verbatim from the hash-pinned q0 endpoint
seal; no row or coefficient is selected here.

Promote only if both family mean raw-MSE ratios are below `1.0`, the pooled
ratio is at most `0.97`, and at least half the rows improve in each family.
The fixed rule remains eligible for an engineering port only if it passes.

