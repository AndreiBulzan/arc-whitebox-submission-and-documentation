# Verdict: partition-balanced packet cubature R1--R3

Date: 2026-08-07

## Headline

The one-frame **capacity mechanism passed decisively**, while the first
network-independent low-rank NNGP surrogate **failed decisively**.

This changes the controlling question.  There is no longer evidence that a
packet expectation intrinsically requires several propagated frames.  A
practical one-candidate-per-Kerdock-line solver can reproduce the finite
packet pool almost exactly when supplied with actual output features.  What
is missing is a cheap, lawful feature map which predicts the output-space
cancellation choices from weights or early trajectory state.

No deployable estimator, compute projection, package, upload, or submission
is authorized by these receipts.

## R1: finite pool and actual-output capacity pass

R1 used the existing `epsilon=0.20` Gaussian-packet construction, with
`m=4` and `m=8` nested candidates for each of 33,024 unoriented K129 lines.
Candidate selection used unavailable actual final-layer output deviations;
one selected candidate and its antipode were retained per line, hence exactly
66,048 propagated rows in the selected rule.

On reused Full8 plus Generated8:

| result | value |
|---|---:|
| q0 raw MSE | `3.462164828e-7` |
| ideal 64-replicate packet raw MSE | `1.607330452e-7` |
| m=4 finite-pool raw MSE | `1.679139486e-7` |
| m=4 ideal-gain retention | `96.1285%` |
| m=4 selected pool-gain retention | `99.9159%` |
| m=8 finite-pool raw MSE | `1.679756954e-7` |
| m=8 ideal-gain retention | `96.0953%` |
| m=8 selected pool-gain retention | `99.9012%` |
| m=8 selected-to-pool output MSE, mean | `7.4225e-11` |
| m=8 selected/random reconstruction ratio, median | `4.346e-4` |
| rows improved versus q0 | `16/16` |

Every preregistered finite-pool and output-selection gate passed.  This is
**broad statistical exploratory on a reused bank**.  The target-free
reconstruction result is **component** evidence.  Actual-output selection is
not available to a contest estimator.

The practical result is stronger than the existential colorful-balancing
bound: ordinary coordinate descent found a one-frame rule whose output mean
was essentially the finite-pool mean.  One-frame capacity is therefore no
longer the bottleneck.

## R2: pure input-NNGP/Nyström surrogate killed

R2 froze four universal selectors before opening any supplied network:

```text
P4:  m=4, p=128, r=64   primary
P8:  m=8, p=64,  r=64   primary
D8a: m=8, p=128, r=64   diagnostic
D8b: m=8, p=128, r=128  diagnostic
```

Features were block-centred within each line and constructed from difference
landmarks under the exact antipodally evenized depth-32 He-ReLU NNGP kernel.
The lookup implementation agreed with the literal 32-fold recurrence to
maximum absolute error `8.2451e-11`; all difference Grams were positive.

Each selector almost perfectly balanced its represented feature space:

| variant | represented selected/random ratio |
|---|---:|
| P4 | `3.035e-4` |
| P8 | `5.949e-7` |
| D8a | `1.649e-7` |
| D8b | `1.148e-5` |

But the frozen supports did not balance actual supplied-network outputs.
Their target-free actual-output reconstruction ratios were:

| variant | actual selected/random ratio |
|---|---:|
| P4 | `1.2291` |
| P8 | `1.0418` |
| D8a | `1.1486` |
| D8b | `0.9064` |

The preregistered limit was `0.30`.  Post-seal pooled ideal-gain retention was
`-9.15%`, `+8.03%`, `-10.93%`, and `-7.72%`, respectively.  No variant met a
single complete promotion gate; the best primary P8 improved only `10/16`
rows and had raw MSE `3.313269090e-7` versus q0 `3.462164828e-7`.

This is a clean **broad statistical exploratory** kill of the frozen
rank-64/128 pure input-NNGP/Nyström spelling.  The failure occurred before
target association in the stronger target-free output-reconstruction gate.

## Interpretation

The low-rank feature objective was not merely a little miscalibrated.  It
was nearly orthogonal to the output-relevant cancellation outside its own
landmark span: improvements of six to seven orders of magnitude inside the
represented space became no improvement over independent sampling in actual
output space.  Increasing from 64 to 128 modes did not approach the required
regime.

This does **not** kill:

- the packet target, which retains about 54% measured raw headroom;
- one-frame partition balancing, whose unavailable-information capacity is
  now directly demonstrated;
- a genuinely weight-conditioned selector;
- a genuinely deeper/full-network feature map;
- teacher-verified centre clustering or output-relevant prevariance
  compression.

It does kill treating a small, universal input-NNGP landmark projection as
the missing bridge.  Do not expand this exact spelling by routine rank
tuning.

## R3: first-layer-conditioned tail kernel also killed

R3 added the exact supplied-network first-layer states for every `m=4`
candidate and formed the depth-31 NNGP covariance of the resulting
antipodal pair objects.  This was materially different from R2 and from the
old binary H1 gate-co-occupancy support experiment.  Four preregistered
Nyström variants used `p=128..256` and `r=64..256`.

The scalar tail-kernel lookup agreed with the literal 31-fold recurrence to
`1.5974e-10`, q0 association was exact, and every selector again balanced
its represented space strongly.  It still failed the stronger target-free
output-reconstruction gate:

| variant | represented selected/random | actual selected/random |
|---|---:|---:|
| C4P p128/r64 | `5.423e-7` | `0.9900` |
| C4D1 p128/r128 | `5.857e-6` | `1.1418` |
| C4D2 p256/r128 | `5.084e-6` | `1.0868` |
| C4D3 p256/r256 | `5.950e-4` | `1.5339` |

The frozen limit was `<=0.30`.  Mean agreement with the network-specific
actual-output oracle choices stayed at chance (`about 0.25`) for all four
variants.  Per preregistration, targets were not opened: this is a
target-free **component** kill stronger than a post-target reversal.

Therefore both the universal input-NNGP and exact-first-layer-conditioned
low-rank tail-kernel bridges are closed.  A further selector must use a
genuinely deeper/full-network observable, not a larger routine Nyström sweep.

The independent `K=1` collective full-rank gate-Gram covariance route was
also tested target-free and killed: pooled correction fidelity fell from
`0.972178` at layer 1 to `0.226120` at layer 2 and `-65.9023` at layer 32.
That closes one shared covariance, not all `K>1` prototype systems.  A
prototype route now requires teacher evidence that `K<=16` retains the
output-relevant centre structure before any production-shaped sweep.

## Evidence and seals

R1:

- preregistration: `02311ac9213f9eda085c97343cc5daa59ae45b5ffc41584905e48bd240ad8ff7`
- target-free capture: `24a0ab5bdff0e3addf1d75eb7106d83d1635e02c249bb11a35d2d77c2d13a966`
- post-seal score: `2cf559daae54e79710f20b835217fb47d4a3c8c96c94126edb34a181b1ece201`

R2:

- preregistration: `0a5c771394217999a2e4796a4da7ee35c2a764b331312fe7fa538365c7831d64`
- universal selector: `c2cdec00378d37c29c3cb8b392122b89460f995b0405e2a5f368376bac73c449`
- network capture: `d1d55d6b4cc5aab9c69331cf4f0d37866a20b1c01fc18af6aa539b594d0aa082`
- post-seal score: `691d14fdcc49997a651ca6184c9025ab0cbdd4bff53ec73a2f6b5a8ac487553d`

R3:

- preregistration: `506a9339b70c9ef877753e8a265657775e6119f317c39fc8d29125e6e687e9a6`
- target-free capture: `9408d3a8bbd25c8df8390b43b899e2993a2fd74c3095ed6ecb002f5e3aa3ac63`

Across both rounds: zero FlopScope sessions, physical rows, packages,
uploads, submissions, or remote actions.
