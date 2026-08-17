# Final-hidden Rao--Blackwell R1 verdict

Date: 2026-07-29  
Evidence label: **component**  
Decision: **hard kill**

The target-free low-K capture was sealed before the separate scorer opened
the two Full and two Generated final targets.  No candidate passed.

The final hidden covariance really is concentrated:

```text
mean energy in top  2   Full 0.675   Generated 0.657
mean energy in top  8   Full 0.861   Generated 0.848
mean energy in top 16   Full 0.923   Generated 0.921
mean energy in top 32   Full 0.965   Generated 0.965
```

That concentration does not make the orthogonal residual conditionally
Gaussian.  Every fixed rank/mode worsened pooled error on both families.
The least harmful K32 spelling, rank-32 constant covariance, measured:

```text
                         Full2       Generated2
candidate / baseline    1.00396        1.02005
maximum row ratio       1.09265        1.03502
required                <=0.50         <=0.50
```

Rank 2--8 was materially worse.  Scaling residual covariance by each
sample's orthogonal energy did not change the conclusion.  The non-Gaussian
signal is present inside the nominally low-energy residual directions; PCA
energy is therefore not an importance measure for this integral.

This closes final-hidden PCA conditional Gaussianization as the requested
low-compute breakthrough.  Do not broaden it or tune the rank.  It does not
close observable-specific connected-chaos transport, which must use
downstream response rather than covariance energy.

Authoritative artifacts:

- capture SHA-256:
  `f8b5fe55e2e8d1ae39ca15457152a3ca6225226f5c9c4a7816b8444121d14c5d`;
- post-seal score:
  `final_hidden_rao_blackwell_r1_postseal_20260729.json`.

No FlopScope session, physical row, package, upload, submission, or remote
action was performed.

