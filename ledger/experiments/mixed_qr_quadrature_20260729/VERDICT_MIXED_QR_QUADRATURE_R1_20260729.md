# Degree-6 support × mixed-QR quadrature R1 — killed

Date: 2026-07-29.

Evidence label: **component** (four fixed Full rows plus four fixed Generated
rows, actual production-aligned K146 final predictions). Adjusted values are
**projections**. This was not FlopScope, a physical row, a package, an
upload, a submission, or a remote action.

## Result

The target-free degree-6 support was retained. At O1 phase positions
`(0,4,8,12)`, the input frame was replaced by the canonical QR frame
`Q.T`, with `W0=Q R` and `diag(R)>=0`. All eight trajectories were finite;
the float32 replay frames remained orthogonal to at most `2.04e-8`, and the
relative subdiagonal residual of `frame @ W0` was at most `6.91e-9`.

| candidate | Full pooled ratio | Full max | Generated pooled ratio | Generated max |
|---|---:|---:|---:|---:|
| literal `17/146` | `1.045840` | `1.503692` | `1.134528` | `1.206546` |
| fixed alpha `0.10` diagnostic | `1.026344` | `1.458254` | `1.108551` | `1.165054` |

The literal spelling worsened actual final MSE by `4.58%` on Full and
`13.45%` on Generated. The fixed diagnostic also worsened both families.
Its port-plus-QR adjusted projections are correspondingly poor:

```text
literal Full       1.36257e-7
literal Generated  1.47812e-7
alpha=.10 Full     1.33717e-7
alpha=.10 Generated 1.44428e-7
```

The preregistered broadening gate required pooled ratio `<=0.94`, p95 and
maximum `<=1.50`, and a cross-family projection `<=1.2e-7`. It fails
materially before implementation-cost uncertainty matters.

## Decision

Kill this exact mixed-QR spelling immediately. Do not broaden, physically
meter, package, or tune its phase positions. QR triangularization is not a
useful effective-dimension frame for this production K146 statistic on the
tested cross-family scout.

## Hashes

- preregistration:
  `0c515d427aa9039bb5b81fe3f78e8be9176f4a04d05db884f371d716f4d53fb3`
- adapter:
  `26166e4775016d218b002ebb6671f5c3f36f5e48fa74b2b188af6f4ff8d3851b`
- capture source:
  `7bba26ff15a9f26c11e6c63e62855efa4eaee91159c00b2827701813fb9df84a`
- target-free predictions:
  `84284f597d106a4ae4bcdaa9e3d2f3ab4389f635fb34a535f5a2b3351e211f01`
- target-free manifest:
  `25d9c2909bed44823e09b825431aa67a3c68267a1997fcf098465ef696880acd`
- array seal:
  `533a766d91af5222807cd8b7a3dddcbaaddb07f6a81685b5d0b734974f3db3fb`
- scorer:
  `b3a43bb9d327c7cd55c752a27e95d8b76e3d3a5051a22aed61e1c43486786ff1`
- score arrays:
  `4a4b7edb58abeb3c69c2d43948952b668d032a56e5b224c0b2347dcab0b7eb40`
- score report:
  `b21936103f20f4b71b66938197f3a6ad596517f17dae4487cae7e198be1886f4`

