# Preregistration: local antipodal packet-GP direct post-seal score R1

Date: 2026-08-08

Evidence scope: `component` on the four already-fixed development rows.

The target-free local bridge was sealed before this score and passed its
strong gate.  The primary estimator is frozen as follows:

1. use the `basis_energy` amplitude variant from
   `local_antipodal_packet_gp_l1_r1_targetfree_20260808.npz`;
2. average its 129 predicted complete-basis packet responses;
3. multiply by the exact Gaussian radial normalization
   `E[R] / packet_radius_mean`;
4. apply no fitted scale, offset, shrinkage, row selection, output selection,
   or target-dependent calibration.

The target-free `theory`, `global_energy`, and `zero_variance` variants are
reported as diagnostics only.  The exact 32-replicate packet mean is an
oracle ceiling, not a candidate.

The baseline is the exact canonical K129 response reconstructed from the
same sealed complete-basis capture.  Open only the expectation targets for
Full640--641 and Generated88--89 after verifying all upstream hashes.

Primary acceptance requires:

- at least 35% pooled raw-MSE reduction from canonical K129;
- strictly positive raw-MSE reduction on both Full and Generated;
- at least three of four individual rows improved.

A primary pass authorizes target-free compute compression and disjoint-row
validation.  It does not authorize Mini100, a physical row, a package,
upload, or remote submission.  A diagnostic-only variant pass is a lead that
must be frozen in a new preregistration before disjoint validation; it is not
promoted to the primary result retroactively.

No arithmetic projection is a benchmark receipt.  The current four-row
score remains `component` even if the margin is large.
