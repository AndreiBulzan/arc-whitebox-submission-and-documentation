# Fusion-frame K120 r2 implementation-correction amendment

Date: 2026-07-28

R1's target-free geometry passed, but its scorer exposed a dimensional bug
before the mechanism received a valid accuracy test: the H1 affine match put
the radial Gaussian mean and second moment directly into a unit-sphere
cloud.  It therefore applied the chi radius inside H1 and again at readout,
producing final means around 6--12 instead of around 0.4--0.8.  R1's
candidate/control target ratios are not evidence about the fusion frame.

R2 changes only the unit conversion:

```text
unit-sphere H1 mean   = Gaussian ReLU mean / E[R]
unit-sphere H1 second = Gaussian ReLU second / E[R^2]
```

The seed, 119 axes, eight coordinate sweeps, masks, conventional K120
control, propagation, and every gate in
`PREREG_FUSION_FRAME_K120_R1_20260728.md` remain fixed.  To prevent the
opened-but-invalid r1 rows from influencing the corrected result, r2 uses
disjoint rows:

```text
Full       3, 103
Generated  3, 67
```

R2 writes a fresh artifact identity and refuses overwrites.  Failure kills
the fusion-frame K120 construction.

