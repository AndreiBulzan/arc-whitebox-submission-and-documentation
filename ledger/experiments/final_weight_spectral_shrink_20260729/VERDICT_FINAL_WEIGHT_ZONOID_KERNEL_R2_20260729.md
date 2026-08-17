# Final-weight zonoid-kernel R2 verdict

Date: 2026-07-29  
Evidence label: **component**  
Decision: **hard kill**

R1's singular-coordinate shrink did not cover smoothing the even final
observable over the directions of the `W31` columns.  R2 tested that stronger
and mathematically distinct idea using

```text
E ReLU(w·h) = 0.5 w·E[h] + 0.5 E|w·h|.
```

It normalized the saved K146-to-Gaussian correction by `||w||` and applied
two preregistered even kernel-ridge smoothers over pairwise final-column
cosines:

- the exact centered covariance kernel of absolute Gaussian projections;
- the degree-2 spherical kernel `cos(theta)^2`.

Ridge was fixed per MLP from the target-free `q0-q1` noise estimate.  Full60
fit one interpolation scalar, Full20 development selected the kernel, and
the R2 Full20 test and Generated64 prediction arrays were sealed before the
selection stage read their targets.

Degree-2 won development with only a `-0.0088%` change and
`eta=0.00718243`.  It then reversed on both held banks:

```text
                                      candidate / K146
R2 Full20 test raw MSE                      1.00017631
Generated64 observed MSE                    1.00010081
Generated64 noise-corrected MSE             1.00012162
required on Full and corrected Generated    <=0.90
```

This directly closes the cheap absolute-projection zonoid kernel and the
degree-2 even-kernel ridge spelling.  It does **not** prove that every
possible constrained zonoid inversion is useless, but the fitted
interpolation strength collapsing below `0.01` on both kernels is strong
evidence that cross-output final-direction smoothing contains essentially
none of the required ten-percent error reduction.  Do not add a bandwidth
or harmonic-degree grid.

Authoritative artifacts:

- preregistration SHA-256:
  `49c563ddc9c839e4bc73b7d2a0cfd4096e2dc42a3a5b5e5b761ce32b5aa5f1c1`;
- sealed prediction archive SHA-256:
  `dcf9ad2907b328bf1d289a98f2fe1125638666c74f218d6f445206c00b7116f6`;
- selection receipt SHA-256:
  `b731ae65a2dc54ea977a13a284fdd6527cc2e2d171830428e288c7e37c98da9c`;
- post-seal score SHA-256:
  `eefe52533f481441fe47da3bc8dd77dbca2743966409f3cf1d725d87dbb7cced`.

No GPU, FlopScope session, physical row, package, upload, submission, or
remote action was performed.

