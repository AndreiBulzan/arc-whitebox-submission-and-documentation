# Preregistration: Kerdock-gauge-aligned polar frame R1

Date: 2026-08-04

R1's useful unregularized polar mean was unstable; R2's q0 shrinkage removed
both the singularity and the accuracy gain. This test uses a different exact
mechanism. A complete Kerdock frame is unchanged as a node population when
its coordinate rows are transformed by a Walsh character and an XOR
translation: these operations merely permute/sign the antipodal nodes inside
its complete bases. We may therefore choose that gauge to align each complete
frame with q0 *without changing its individual cubature estimate*, then take
the polar mean in the well-conditioned representatives.

Prior-art queries covered frame signs, phase gauges, Walsh/Hadamard alignment,
signed permutations, eigenvector signs, and Kerdock automorphisms. Existing
code only canonicalizes individual eigenvector signs; no attempt optimizes an
exact complete-design gauge before combining frames. This is **novel in the
capsule** and directly invalidates the assumption that the near-null polar
subspace must be regularized by changing frame weights.

Stage A is target-free on Full0 and Generated0:

1. align right and d2 frames over all `256 x 256` Walsh-character/XOR gauges;
2. verify original and aligned complete-frame predictions agree to RMSE
   `<=1e-10` and maximum absolute difference `<=1e-8`;
3. require the aligned equal `q0+right+d2` polar minimum singular value to be
   at least `0.05` on both rows.

Only if all gates pass may the fixed alignment algorithm be captured on
Mini100. Mini100 promotion gates remain raw ratio `<=0.92`, at least 60/100
rows improved, and paired-bootstrap upper 95% ratio `<1.0`. No target may
select a gauge, coefficient, or frame.

