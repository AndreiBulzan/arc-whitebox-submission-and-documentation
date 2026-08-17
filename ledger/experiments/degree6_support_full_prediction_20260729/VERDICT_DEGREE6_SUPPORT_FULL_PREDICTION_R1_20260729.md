# Degree-6 support full-prediction R1 — killed

Date: 2026-07-29.

Evidence label: **broad statistical** accuracy on the fixed K146 Full100 and
Generated64 development rows. The adjusted values below are
**projections** from remote submission 320262 plus the one-row measured-whole
persistent-boundary saving. This was not a FlopScope run, physical row,
package, upload, submission, or remote action.

## What was tested

The target-free degree-6 support

```text
[3,11,15,17,27,34,36,45,54,64,66,70,78,82,111,115,120]
```

was substituted for the incumbent O1 support in the actual production-aligned
K146/m17 CUDA replay. All propagation, moment repairs, compression, late
selection, readout, and the literal `129:17` arm blend were retained.

Because the preregistered endpoint scout was within three percentage points
of the final-output gate, the same target-free pass also captured the one
fixed interaction candidate: the canonical `W0.T @ W0` eigenframe at O1
phase positions `0,4,8,12`. There was no support, position, coefficient, row,
or family grid.

All `328` trajectories were finite. The plain and mixed predictions, arm
outputs, indices, and seeds were serialized and hash-sealed before the
separate scorer opened either target bank.

## Actual final-output result

Ratios and row-ratio p95 values below use ordinary observed final-layer MSE
for both families, exactly as preregistered.

| candidate | Full100 ratio | Full p95 | Generated64 ratio | Generated p95 |
|---|---:|---:|---:|---:|
| plain, literal `17/146` | `0.963488` | `1.782792` | `0.964512` | `1.388714` |
| plain, fixed alpha `0.10` | `0.955844` | `1.658414` | `0.960679` | `1.398689` |
| mixed-SVD, literal `17/146` | `0.956136` | `1.501572` | `0.974180` | `1.418470` |
| mixed-SVD, fixed alpha `0.10` | `0.948385` | `1.456029` | `0.968861` | `1.437327` |

The standard Generated64 noise-corrected ratios were respectively
`0.957186`, `0.952562`, `0.968850`, and `0.962434`. None changes the verdict.

The hard gate required pooled ratio `<=0.94`, row-ratio p95 `<=1.50`, and
finiteness in both families for a literal candidate. Neither literal
candidate passes. The fixed-alpha rows are post-hoc diagnostics and also
miss the material ratio gate.

## Port-calibrated adjusted projections

The projection starts from:

```text
remote R17 adjusted                    1.3153879677e-7
remote R17 raw                         2.0904670478e-7
persistent-boundary effective saving       1.883400548B
port-only projection                   1.3009130163e-7
```

Multiplying the port projection by each broad raw ratio gives:

| candidate | Full-calibrated | Generated-calibrated |
|---|---:|---:|
| plain, literal | `1.253414e-7` | `1.254746e-7` |
| plain, alpha `0.10` | `1.243469e-7` | `1.249759e-7` |
| mixed-SVD, literal | `1.245701e-7` | `1.269210e-7` |
| mixed-SVD, alpha `0.10` | `1.235603e-7` | `1.262281e-7` |

The mixed rows include their projected `+0.251985920B` runtime count. These
are sensitivities, not remotely validated or locally measured adjusted
scores.

The checkpoint stack needs a defensible total raw ratio near `0.9224`.
The best cross-family result here is the plain fixed-alpha diagnostic, whose
worst-family ratio is `0.960679`. The interaction is more asymmetric and its
worst-family ratio is `0.968861`. Neither is close enough to justify a
physical or implementation lane.

## Verdict

Kill both exact degree-6 full-prediction spellings and do not tune this
support, the SVD positions, or a blend coefficient on the opened banks.
The endpoint improvement did not survive the complete nonlinear
propagation/readout strongly enough. No physical, package, or remote work is
licensed.

## Artifact hashes

- preregistration:
  `fad4369f410e3fdbff030f0cd8abde72ac5de1605e7b406ab22496930b53b747`
- CUDA adapter:
  `465cc09e60fe5b4f5045e1037cabcecb0f923339dc4ade9fe6683baf1fa328a6`
- capture source:
  `2d069c5061842314129a7532d9542a08b36c8605f0f9f26697335a8de7712d48`
- target-free prediction archive:
  `75adb0c1e6e16ece1c77a74da3f40a3d7b53ba2c52a3623f7ec5f7209b88a59a`
- target-free prediction manifest:
  `680cdd410a806ae371343e412a58ca76da387dd1fd7b36e7cf0338b51959c9de`
- target-free array seal:
  `b26f2b06a8d844a6186ec11db308bc293bd8778e5836ad711890aa77768ae37c`
- scorer:
  `9b5f0d7763bca4001801dde8872cf46ce53b00368a94a673caa9db1d6cf3017b`
- score arrays:
  `25cfa6ffe550fc7a63fdddc35464d95e1b9acc9921ec849091241f8b065dae5e`
- score report:
  `cc3f7e76744726036851d1be1638f3708d899cf9415be1a99a621cecae0f3ec7`
