# Borel–Padé readout R1 verdict

Date: 2026-07-29

Verdict: **killed target-free**. Do not open targets, broaden, integrate,
benchmark physically, package, upload, or submit this spelling.

Evidence label: **component**. This was an offline K146 final-preactivation
capture, not a measured whole, broad statistical result, or remote result.

## Result

The frozen standard Borel–Padé definitions fail their predeclared analytic
stability gate on every one of the eight networks:

| order | positive-axis pole coordinates | singular | near quadrature node | survives |
|---|---:|---:|---:|---|
| `[2/2]` | 633 / 4096 arm-coordinates | 0 | 0 | no |
| `[3/2]` | 825 / 4096 arm-coordinates | 0 | 0 | no |

Every row/arm combination has at least one positive-real denominator pole.
After combining arms, only 1606/2048 `[2/2]` output coordinates and
1436/2048 `[3/2]` output coordinates remain finite/defined. A 256-vector
readout therefore does not exist under either predeclared order.

Per protocol, this closes the lane before target access. There is no raw-MSE
ratio to report and no family-transfer claim.

## Exact scope

```text
Full rows       0, 1, 2, 3
Generated rows  2, 4, 5, 6
targets opened  0
physical rows   0
remote actions  0
```

The capture reproduced the frozen current K146 predictions with maximum
absolute difference `8.71734e-7` and relative RMSE `1.01984e-7`, comfortably
inside the adapter's existing association tolerance. Raw moments through
order eight and all Edgeworth coefficients were finite.

The first two generated series coefficients were separately checked before
capture against the closed-form `kappa3`, `kappa4`, and `kappa3^2`
Edgeworth terms; maximum discrepancies were `4.34e-19` and `5.20e-18` on a
synthetic target-free distribution. The pole result is not a sign or
Hermite-index error in those controlling terms.

## Economics

Evidence label: **projection**.

For the existing `74,752 x 256` two-arm cloud:

```text
seven recursive power multiplies        133,955,584
eight armwise reductions                153,088,000
radial scaling and width-only algebra    <13,000,000
projected incremental total                 <0.30B
hard ceiling                                  2.00B
```

The idea was cheap enough; stability, not work, killed it.

## Artifacts

```text
preregistration
PREREG_BOREL_PADE_READOUT_R1_20260729.md
SHA-256 a425c383d234b15ea140c26b9ea428423a5662fdf92b49f11d698771c9fcf694

capture source
capture_borel_pade_readout_r1_targetfree_20260729.py
SHA-256 50ec4aac1084ce871bda38bd47a6e310faca9614bffc7fec711fdbb48417c1a5

target-free arrays
borel_pade_readout_r1_targetfree_20260729.npz
SHA-256 879f39b3006d8510cd84fad712d5af4f74df5311daad66a34a44deaa3a364a9c

target-free receipt
borel_pade_readout_r1_targetfree_20260729.json
SHA-256 2882e764bfad8b17ceb24829851e2c15a4950612ed91d83927346266e220f491
```

## Reopen rule

Do not reopen by changing quadrature order, clipping around poles, taking a
principal value, or fitting a contour/pole prescription on these rows.
Those would be new methods with new analytic and transfer risks. Reopening
requires a genuinely different summability mechanism whose positive-axis
regularity is established target-free before any score is read.

