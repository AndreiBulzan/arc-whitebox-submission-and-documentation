# Gauge-closure Stage-A continuous-optimizer R3 verdict

Date: 2026-07-29

Evidence label: **component**.

## Verdict

**R3 is closed: development gate failed, and this experiment family is
closed with no further retry.**

The exact R1 trajectory replayed bit-for-bit through step 20:

```text
expected model-state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
actual model-state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
```

The original live AdamW state, model, RNG stream, batch stream, and loss
were then continued unchanged through total step 200.  The best development
checkpoint remained step 20:

```text
late-16 innovation ratio          0.6039600626
late-16 signed-premean ratio      0.5962199995
```

Both miss the preregistered hard `<=0.40` gate.  Every later evaluation had
a worse selection objective.  The optimizer-state-reset hypothesis raised
by R2 is therefore rejected: retaining the exact optimizer trajectory does
not unlock the gauge-closure model.

No development prediction archive was sealed because the gate failed.  No
Full-held or Generated target value was opened.

## Frozen identities

```text
preregistration
PREREG_STAGE_A_CONTINUOUS_OPTIMIZER_R3_20260729.md
SHA-256  38c324c5b00a17b92d480f9895397348d2224e36cb692a130b24e31527839c54

source
train_stage_a_continuous_optimizer_r3_20260729.py
SHA-256  22bccef852283760e9cca8acfa06799ae7ee53d1f7151abeee339ebe7e92a696

report
stage_a_continuous_optimizer_r3_run1_20260729/report.json
SHA-256  e16c2f6afa6e75a65252365e7857ab747eb288594edec2f3919be29b52f69c47

selected step-20 checkpoint
stage_a_continuous_optimizer_r3_run1_20260729/checkpoint.pt
SHA-256  ec81cf3d3b18a8d46379b3167fc1d12fcac3c2ed874c417ebf0164e67e5d5c2d
model-state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
```

## Continuous-trajectory results

All values are ratios of pooled error sums over Full development indices
`700..799`.  Selection minimizes the maximum of the two late-16 ratios.

| step | late-16 innovation | late-16 signed premean | selection max |
|---:|---:|---:|---:|
| 20 | **0.603960** | **0.596220** | **0.603960** |
| 40 | 0.708819 | 0.653191 | 0.708819 |
| 60 | 0.890607 | 0.852485 | 0.890607 |
| 80 | 1.044509 | 2.190487 | 2.190487 |
| 100 | 1.340368 | 5.358798 | 5.358798 |
| 120 | 1.070620 | 1.924077 | 1.924077 |
| 140 | 1.106663 | 1.552528 | 1.552528 |
| 160 | 1.195855 | 2.569823 | 2.569823 |
| 180 | 1.103712 | 1.957006 | 1.957006 |
| 200 | 1.155758 | 2.365633 | 2.365633 |

The selected step-20 innovation octets were:

| depth | innovation ratio |
|---|---:|
| 0--7 | 0.482491 |
| 8--15 | 0.458686 |
| 16--23 | 0.557886 |
| 24--31 | 0.662029 |

The final octet also misses its preregistered `<=0.60` gate.  All
evaluations were finite and had zero physical-bound violations; failure is
statistical, not numerical.

## Execution boundary

```text
optimizer steps                         200
train-MLP exposures                   1,600
wall                              148.575583 s
torch CUDA peak allocated       469,363,200 bytes
step-20 exact replay                       true
all development evaluations finite       true
bound violations                              0
development gate pass                      false
Full-held values opened                    false
Generated values opened                    false
sealed prediction archive                   none
FlopScope / physical rows                      0
remote actions                                 0
```

R3 establishes that the promising R1 step-20 result was already the best
point on the exact fixed optimization trajectory.  Neither a fresh
depth-localized continuation (R2) nor continuous original-loss optimization
(R3) reaches the required teacher-quality threshold.  Under the frozen
protocol, this closes the gauge-equivariant innovation-closure training
family.
