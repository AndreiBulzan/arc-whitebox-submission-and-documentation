# Official Mini100 deep-pullback tail R2

Date: 2026-08-04

This amendment was fixed after the R1 score and an independent source audit.
R1's K153 proxy used the historical `J J^T` capture, while the packaged K153
source uses the `J^T J` frame. R1 K153 is therefore not package evidence.

R2 captures three already-frozen numerical identities, without using Mini100
targets:

- corrected K153/m24: the exact K153 support and coefficients, with every `D`
  slot supplied by `jacobian_d2_right` (`J^T J`) as in the package;
- K173/m44: the frozen model `44` in the hash-pinned literal prefix curve;
- K197/m68: the frozen hash-pinned R25 selection.

The q0 returned by all three runs must agree within `1e-10`. The q0 Full7
prediction must associate to its exact physical archive within `5e-4` maximum
absolute error. K153/K173/K197 Mini100 results are component-associated CUDA
proxies until a winning candidate receives a separate exact-package Full7
association check.

Fixed promotion thresholds use the historical measured/projection compute
ratios versus remote R31:

| candidate | raw ratio to beat R31 | raw ratio to project `1e-7` |
|---|---:|---:|
| K153 | `0.857` | `0.740` |
| K173 | `0.761` | `0.657` |
| K197 | `0.686` | `0.593` |

A candidate also requires a bootstrap raw-ratio upper endpoint below `1.0`.
These are transfer/capacity screens, not adjusted-score receipts, and no
upload or submission is authorized.

