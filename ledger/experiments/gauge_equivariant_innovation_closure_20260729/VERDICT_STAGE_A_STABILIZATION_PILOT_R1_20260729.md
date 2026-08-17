# Stage-A stabilization pilot R1 verdict

Date: 2026-07-29

Evidence label: **component**.

## Verdict

**Stop.  The frozen stabilization pilot failed its licensing gate narrowly,
so the 600-step Stage-A run was not started.**

The selected checkpoint was the final step 20, the trajectory was
non-worsening from step 10, every complete Full-development evaluation was
finite with zero bound violations, and late-16 signed premean passed.
Late-16 innovation was:

```text
0.6039600626
```

This misses the frozen `<=0.60` pilot threshold by `0.0039600626`
(`0.6600%` relative).  The near miss is not rounded into a pass.

## Frozen identity

```text
addendum
ADDENDUM_STAGE_A_STABILIZATION_R1_20260729.md
SHA-256  71744cfe54bccc1bfc7ff3e7bcace61e8f99c37748c201cc8269ea840779d164

trainer
train_stage_a_gauge_closure_r1_20260729.py
SHA-256  910b9c569d628ff5d816f0f573e76ec711d270e22807bd5daa66a3b67f0ca358

report
stage_a_stabilization_pilot_r1_20260729/report.json
SHA-256  5968165a76e7d99e1eb3c74046549b31dd0fefc3675ec2f24a234aa1839f23ea

selected checkpoint
stage_a_stabilization_pilot_r1_20260729/checkpoint.pt
SHA-256  c8dc4f2627ba9100e660638278449e8f8c1f21970a8d620088972e68d1a65a7c
state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c
```

The architecture/loss/seed/splits were unchanged.  The sole stabilization
was:

```text
batch                     8
pilot steps              20
learning rate          1e-4
gradient clip           1.0
weight decay            1e-5
seed               2026072921
```

Every evaluation used ratio of pooled error sums over all Full development
MLPs `700..799`, in batches of eight.

## Full-development trajectory

| step | late-16 innovation | late-16 signed premean | late-16 post mean |
|---:|---:|---:|---:|
| 1 | 0.906251653 | 0.872439118 | 0.873054831 |
| 5 | 0.716898981 | 0.662579312 | 0.668016473 |
| 10 | 0.636236203 | 0.601566859 | 0.612085155 |
| 20 | **0.603960063** | **0.596219999** | **0.611123360** |

The frozen selection objective was the maximum of the first two columns.
It improved at every evaluation:

```text
0.906252 -> 0.716899 -> 0.636236 -> 0.603960
```

Thus step 20 was the selected checkpoint and the tail-stability condition
passed.  The innovation threshold alone failed.

## Selected checkpoint details

| depth octet | innovation ratio | signed-premean ratio | post-mean ratio |
|---|---:|---:|---:|
| 0--7 | 0.482491309 | 0.844958809 | 0.780729584 |
| 8--15 | 0.458685673 | 0.491128016 | 0.486425759 |
| 16--23 | 0.557886337 | 0.555810696 | 0.570076083 |
| 24--31 | 0.662028885 | 0.694049247 | 0.706130926 |
| all | 0.541875529 | 0.601739040 | 0.603034371 |
| late 16 | 0.603960063 | 0.596219999 | 0.611123360 |

The final octet remains the hardest region.  It also exceeds the eventual
Stage-A late-octet innovation gate of `0.60`.

## Runtime and safety

```text
wall                               44.197762484 s
torch CUDA peak allocated         469,363,200 bytes
train MLP exposures                        160
all evaluations finite                    true
bound violations                              0
Full held values opened                    false
Generated values opened                    false
FlopScope / physical rows                      0
remote actions                                 0
```

The pilot demonstrates real local state-sufficiency signal and stable
optimization under the one frozen repair.  It does not license extrapolating
another 580 steps, changing the pilot threshold, or trying another optimizer
setting.  A new run would require a separately authorized and preregistered
mechanism; R1 is closed at this gate.

