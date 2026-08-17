# Stage-A depth-localized continuation R2 verdict

Date: 2026-07-29

Evidence label: **component**.

## Verdict

**R2 is closed: development gate failed.**

The exact 200-step continuation completed.  Its best selected checkpoint was
the first evaluation at step 20:

```text
late-16 innovation ratio          0.7851616026
late-16 signed-premean ratio      0.7273872210
```

Both miss the hard `<=0.40` development gate.  Both are also substantially
worse than the frozen R1 initializer (`0.6039600626` and `0.5962199995`).
Every later R2 evaluation had a worse selection objective.

No further continuation or optimizer repair is licensed.  No development
prediction archive was sealed because the gate failed.

## Frozen identities

```text
preregistration
PREREG_STAGE_A_DEPTH_LOCALIZED_R2_20260729.md
SHA-256  6d0e4580502a2fdcc4c007c2a245ad2b11600a01a247525e8e8162fa47314c05

source
train_stage_a_depth_localized_r2_20260729.py
SHA-256  bc47b1c1f32801da170dc397723353187e60669c1a861aea16267b09214b49ac

report
stage_a_depth_localized_r2_run1_20260729/report.json
SHA-256  363be5aee441c9b0c9b8c3a16bb165bfe88dc3162360ccb7a25d6fafb0fcf324

selected step-20 checkpoint
stage_a_depth_localized_r2_run1_20260729/checkpoint.pt
SHA-256  58e3a8cd3e5c82ffb677034c578afcb0b508681645b8cc1b821bcef153eee013
model-state SHA-256
5e70b049f6cb3e0f38037c6771155dfa06c714f7af4155a7f32944798eb549ce
```

Initialization was the exact R1 step-20 checkpoint:

```text
file SHA-256
c8dc4f2627ba9100e660638278449e8f8c1f21970a8d620088972e68d1a65a7c
model-state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
```

## Exact mechanism

Architecture, state, messages, covariance anchor, features, bounds, split,
seed, batch eight, AdamW `1e-4`, weight decay `1e-5`, and gradient clip
`1.0` were unchanged.  R2 added only:

```text
late_weight(d) = 1 + 3 * (d/31)^2
```

to innovation, post-mean, log-second, and target-depth-aligned signed
premean losses.  K3/K4 terms were unchanged.  The checkpoint contained no
AdamW buffers, so the fresh zero-moment optimizer state was declared in the
preregistration.  The deterministic batch stream continued after R1's first
20 draws.

## Full-development results

All values are ratios of pooled error sums over Full development indices
`700..799`.

| step | late-16 innovation | late-16 signed premean | selection max |
|---:|---:|---:|---:|
| 20 | **0.785162** | **0.727387** | **0.785162** |
| 40 | 0.869505 | 0.829483 | 0.869505 |
| 60 | 0.904809 | 1.054972 | 1.054972 |
| 80 | 1.187176 | 4.477376 | 4.477376 |
| 100 | 1.265276 | 4.365062 | 4.365062 |
| 120 | 1.200386 | 3.897555 | 3.897555 |
| 140 | 1.218726 | 3.556101 | 3.556101 |
| 160 | 1.006321 | 1.308679 | 1.308679 |
| 180 | 1.286562 | 3.198129 | 3.198129 |
| 200 | 1.104290 | 2.399511 | 2.399511 |

Selected step-20 octets:

| depth | innovation | signed premean | post mean |
|---|---:|---:|---:|
| 0--7 | 0.578686 | 0.540060 | 0.553010 |
| 8--15 | 0.723928 | 0.660371 | 0.673832 |
| 16--23 | 0.769359 | 0.715576 | 0.717751 |
| 24--31 | 0.805078 | 0.755981 | 0.759638 |

The deterministic depth emphasis did not preferentially repair the final
octet.  Under the preregistered fresh optimizer state it immediately moved
away from the stronger R1 checkpoint and never recovered.

## Execution boundary

```text
optimizer steps                         200
train-MLP exposures                   1,600
wall                              145.503941 s
torch CUDA peak allocated       469,363,200 bytes
all development evaluations finite       true
bound violations                              0
Full held values opened                    false
Generated values opened                    false
sealed prediction archive                   none
FlopScope / physical rows                      0
remote actions                                 0
```

R2 provides a clean negative result for this exact depth-weighted
continuation.  It does not alter the separate R1 finding that the underlying
gauge-equivariant state carried useful signal before continuation.

