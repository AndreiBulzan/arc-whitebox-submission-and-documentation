# Packet full-covariance antipodal-anchor broad B1/R1 verdict

Date: 2026-08-09

Verdict: **reject the fixed one-antipodal-pair final-output estimator**.

Evidence label: **broad statistical**, scoped to fixed Full16 plus
Generated16. The target-free arrays were sealed before targets opened.

## Result

The primary frozen estimator was R90 lambda=0.0075 plus the difference
between the two-anchor smoothed cloud and a simultaneously propagated literal
canonical cloud.

- pooled corrected raw-MSE ratio: `121.6462`;
- Full ratio: `111.4216`;
- Generated ratio: `130.7789`;
- improved rows: `0/16` in both families.

The direct smoothed estimate and the frozen 0.75 correction also failed by
roughly two orders of magnitude. All numerical and association checks passed:
the CUDA covariance schedule agreed with the sealed float64 schedule to
`1.975e-11` maximum absolute error, and the literal cloud exactly reproduced
the sealed packet-oracle q0 on validation row Full640.

## Why the earlier component pass did not transfer

The A1 component measured centrewise and sixteen-centre averaged packet
responses. The benchmark estimator averages 66,048 oriented K129 centres,
where large local smoothing responses cancel to leave a correction about two
orders of magnitude smaller. The fixed-pair predicted correction had RMS
`0.00527` on Full and `0.00613` on Generated, whereas the true packet-oracle
correction is around `2.76e-4` RMS on the sealed packet population.

Post-seal scalar diagnostics confirm this is not merely a missing universal
shrinkage factor: the target-aware optimal coefficient was `-0.00784` on Full,
`+0.00563` on Generated, and `-0.00010` pooled, with essentially zero pooled
capacity. The predicted signed correction therefore lacks the needed global
cancellation geometry.

## Boundary of the rejection

This kills the fixed first antipodal pair as a broad estimator. It does not
yet kill averages over a fixed, Kerdock-spread set of 2, 4, or 8 antipodal
pairs. The decisive remaining component test is to evaluate those prefix
sizes against the already-sealed global packet correction itself on all
sixteen packet-oracle networks. If that curve cannot recover the global
signed mean at score-compatible anchor count, the full anchor family closes.

No physical row, FlopScope session, package, upload, submission, or remote
action occurred.

