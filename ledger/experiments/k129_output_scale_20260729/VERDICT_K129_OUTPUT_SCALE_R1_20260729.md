# K129 fixed output scale — R1 verdict

Date: 2026-07-29

## Verdict

**PROMOTE AS A SMALL COMPOSABLE LEAD.**

The coefficient was frozen at `a = 1.000025` before this audit. Applied only
to the final 256-vector of K129/O0, it improves the pooled central MSE on all
three available process families:

| bank | evidence | endpoint | candidate / baseline | gain |
|---|---|---:|---:|---:|
| Full100 | broad statistical | lambda `0.0075` | `0.9914966082` | `0.8503%` |
| Generated64, noise-corrected | broad statistical | lambda `0.0075` | `0.9981235044` | `0.1876%` |
| independent-process Test64, noise-corrected | component | lambda `0` | `0.9894471487` | `1.0553%` |

The Full100+Generated64 row-pooled ratio is `0.9941231815`. The descriptive
all-three pooled ratio is `0.9928421187`; it is not a new evidence class.

This is not enough to cross `1.2e-7` alone. Using the least-favourable of the
three central ratios and the conservative projected request cost moves R21's
current remote-calibrated projection from `1.22715e-7--1.23213e-7` to about
`1.22485e-7--1.22982e-7`. Using the two-development-bank pooled ratio gives
about `1.21994e-7--1.22489e-7`. Both are projections.

## Robustness details

Fixed ordinal-half ratios are:

| bank | first half | second half |
|---|---:|---:|
| Full100 | `0.9940973` | `0.9889576` |
| Generated64 | `1.0037418` | `0.9923745` |
| Test64 | `1.0045323` | `0.9777519` |

The two reversals are only `+0.374%` and `+0.453%`; neither reaches half a
percent. They prevent a claim of uniform row-wise improvement, but are not a
material magnitude reversal for this composable screen.

Paired 100,000-draw bootstrap ratio p95 values are `1.00167` Full100,
`1.01222` Generated64, and `1.00407` Test64. The central effect is therefore
small and not individually conclusive on these bank sizes. Row-ratio p95
values are `1.07125`, `1.07168`, and `1.08697`, respectively.

The strongest sign check is independent of the frozen coefficient:
target-open descriptive least-squares oracle scales are all above one:

```text
Full100       1.0000545773
Generated64   1.0000232095
Test64        1.0000640180
```

These oracles are diagnostics only and did not alter the candidate.

## Test64 association boundary

Test64 stores the actual K146 O0 `q0` at lambda zero. This is directly
relevant because K129 is that literal O0-only statistic and lambda changes
the endpoint correction rather than the trajectory. It is not an exact R21
confirmation: no K129/O0 lambda-`0.0075` Test64 prediction freeze exists.
Therefore Test64 remains **component** evidence and a process-separated sign
guard, not broad-statistical R21 evidence.

## Economics

The intended implementation is one in-place float32 operation:

```python
fnp.multiply(final_256, 1.000025, out=final_256)
```

Under the pinned FlopScope 0.9.1 tariff this projects to:

- counted FLOPs: `+256`;
- transport requests: `+1`;
- residual: `+2.85--3.44 us`, using pinned lane-held direct-call prices;
- effective compute: `+0.000285256--0.000344256 B`, about
  `0.000194--0.000234%` of R21's remote-calibrated effective compute.

These are projections, not a physical receipt.

## Artifacts

- Audit source:
  `audit_k129_output_scale_r1_20260729.py`
  (`f2c43a3d1efca648892ca4b1e4643880feef996846c480c0c11858fa71f88372`)
- Numerical receipt:
  `k129_output_scale_r1_20260729.json`
  (`27ffd9daab6c3b623360c8334e33b94c7454b8f6d8fd6fd93c8e36f98b2a2880`)

No GPU, FlopScope session, physical row, package, network, upload, or remote
action was performed.
