# K129 final connected-covariance R1 verdict

Date: 2026-08-04

Evidence label: **component development diagnostic** on the already-open
public pilot16.  No FlopScope session, physical row, package, upload,
submission, or remote action occurred.

## Decision

**Kill this spelling.**  Do not implement it in R31 and do not spend a
broader capture on it.

The full K129 cloud does contain a recognizable connected residual, but it
does not supply a useful correction to the structured estimator:

```text
baseline direct K129 raw MSE            4.540889696e-7
hindsight-optimal additive raw MSE      4.540748030e-7
ratio                                    0.999968802
optimal coefficient                      0.00809367
rows improved                                  6 / 16
required ratio / rows                    <=0.75 / >=12
```

Fixed positive gains are actively harmful: ratios are `1.0278`, `1.1151`,
`1.2619`, and `1.4681` at gains `0.25`, `0.5`, `0.75`, and `1.0`.

The observable itself is not random noise:

```text
sample/high-sample pair correlation             0.4152
sample/high-sample contracted variance corr     0.3547
sample/high-sample Gaussian readout delta corr  0.9109
```

Nevertheless, even replacing the sample direction by the inaccessible exact
high-sample connected direction yields only ratio `0.9999843` when added to
the direct K129 estimator.  This closes the route more strongly than a noisy
K129 estimate alone: the pair-covariance correction is almost orthogonal to
the remaining direct-estimator error.

The standalone Gaussian anchors remain several times worse than the direct
sampler (`3.95x` for sample-total covariance and `2.94x` for the public exact
pair anchor versus this direct baseline).  Blending them was already covered
by the low-K portfolio work; the isolated additive test here confirms that
the connected component itself cannot rescue the class.

## Authority

- preregistration:
  `PREREG_K129_FINAL_CONNECTED_COVARIANCE_R1_20260804.md`
- source:
  `screen_k129_final_connected_covariance_r1_20260804.py`
- receipt:
  `k129_final_connected_covariance_r1_screen_20260804.json`

This negative applies to the final layer-30 sample covariance residual and
its Price/Mehler readout correction.  It does not kill a genuinely different
all-layer connected observable or a weight-conditioned orientation rule.
