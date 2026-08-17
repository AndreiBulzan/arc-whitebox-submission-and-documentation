# Frozen gauge-cell free-rollout R1 verdict

Date: 2026-07-29

Evidence label: **component**.

## Verdict

**Killed at the Full-development gate.**

The selected step-20 checkpoint was rolled for all 32 layers on Full indices
`700..799`.  The graph used the exact analytical Gaussian first transition
and thereafter used only its own predicted mean, second moment, recurrent
state, probe state, and Price-covariance scale.  No target mean, second
moment, or covariance entered a transition.

```text
finite                                      true
bound violations                               0
final post-mean MSE                 1.291857981e-2
final signed-premean MSE             2.569754947e-2
hard continuation ceiling                       1e-6
post-mean excess over ceiling               12,919x
signed-premean excess over ceiling          25,698x
```

The failure is broad rather than a small number of unstable rows:

| metric | minimum | median | p95 | maximum |
|---|---:|---:|---:|---:|
| final post-mean row MSE | 2.248e-3 | 1.127e-2 | 2.743e-2 | 6.611e-2 |
| final signed-premean row MSE | 5.290e-3 | 2.200e-2 | 5.534e-2 | 1.119e-1 |

The checkpoint therefore learned a useful one-step teacher-forced correction
but not a self-consistent state transition.  Its errors compound during free
rollout by orders of magnitude.  This fixed-checkpoint spelling is closed;
no Full-held or Generated value was opened, and no FlopScope, physical,
package, upload, or remote action was performed.

## Frozen artifacts

```text
checkpoint SHA-256
ec81cf3d3b18a8d46379b3167fc1d12fcac3c2ed874c417ebf0164e67e5d5c2d

model-state SHA-256
fdc088e50f694f31fac3920138a2269bef310a18d822dd34b4da5eb2fd5e412c

screen source
run_gauge_free_rollout_dev_r1_20260729.py
SHA-256  0dd9378a389ec0d6c806b80c3a6928e0b3ab5aaa9152b8d379ba7cb5306505ba

receipt
gauge_free_rollout_dev_r1_20260729.json

development prediction archive
gauge_free_rollout_dev_r1_predictions_20260729.npz
SHA-256  d4b2f408009c4e50d1d3e0f16188d97d1c4f40571f651e529b48b266a498e468
```
