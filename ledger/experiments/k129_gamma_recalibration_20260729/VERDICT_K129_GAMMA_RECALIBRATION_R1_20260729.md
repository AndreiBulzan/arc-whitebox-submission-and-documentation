# K129/O0 gamma recalibration R1 — structural kill

Date: 2026-07-29.

Evidence label: **component**. No GPU run, FlopScope row, physical benchmark,
package, upload, submission, or remote action occurred.

## Bottom line

Do not fit or tune `GAMMA_COEF` for K129/O0. The O0 basis weights are exactly
uniform:

```text
seed_weights0[b] = 1 / 129
```

The frozen readout is:

```text
equal    = mean_b(endpoint[b])
weighted = sum_b(endpoint[b] / 129)
q        = equal + gamma(features) * (weighted - equal)
```

Therefore `weighted - equal == 0` in exact arithmetic, for every MLP, every
endpoint lambda, and every target family. The gamma coefficients cannot
change the O0 statistic. This remains true for the current endpoint lambda
`0.0075`; changing that lambda changes the per-basis endpoints, not the
uniform-reduction identity.

## Literal component replay

The only existing broad-shaped per-basis O0 endpoint capture has four Full
and four Generated rows. Its maximum float64 `|weighted-equal|` was
`4.4408920985e-15` in both families.

| gamma variant | Full4 pooled raw ratio | Generated4 corrected ratio |
|---|---:|---:|
| fixed `0` | `1.000000000000002` | `0.9999999999999954` |
| fixed `0.5` | `1.0000000000000455` | `0.9999999999999855` |
| fixed `1` | `1.0000000000000782` | `0.9999999999999772` |
| fixed `2` | `1.000000000000154` | `0.9999999999999590` |
| `0.5 × current gamma` | `0.999999999999998` | `1.0000000000000016` |
| clipped `2 × current gamma` | `0.9999999999999953` | `1.0000000000000089` |
| clipped `current gamma + 0.25` | `1.000000000000030` | `0.9999999999999796` |

The largest prediction change across all variants was
`8.8817841970e-15`. Every apparent change is reduction roundoff. The
required reciprocal, both-family `>=2%` raw improvement gate fails at the
target-independent exact ratio `1.0`.

There is no Test per-basis endpoint capture. No Test score is claimed. The
structural identity itself is target-independent and therefore applies to
Test as well.

## Cost and small cleanup implication

Changing fixed/affine gamma constants in the existing graph costs exactly
zero additional counted work and zero additional operation calls, but it also
has zero statistical effect.

Returning `equal_final` directly would delete a dead path whose exact dense
source-static FlopScope 0.9.1 charge is:

```text
counted work removed          340,564 = 0.000340564 B
numerical operation calls          68
basic-view calls                    1
fraction of R21 steady count   0.0002674%
```

This is a valid tiny identity-cleanup lead, not a checkpoint mechanism.
Residual and wall savings were not physically measured, and no implementation
is recommended merely for this amount.

## Artifacts

- `audit_k129_gamma_recalibration_r1_20260729.py`
- `k129_gamma_recalibration_r1_component_20260729.json`

