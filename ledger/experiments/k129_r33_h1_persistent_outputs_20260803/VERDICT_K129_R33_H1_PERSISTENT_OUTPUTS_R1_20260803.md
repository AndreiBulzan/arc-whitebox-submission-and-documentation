# K129 R33 repaired-H1 persistent outputs: verdict

Status: **killed; effective-compute regression**.

Evidence label: **measured whole diagnostic** for one initialized and one
steady exact-archive prediction.  There is no broad-statistical or remote
evidence.

Exact archive:

`runtime/artifacts/k129_r33_h1_persistent_outputs_scale_local_candidate_r2_20260803.tar.gz`

SHA-256:

`5d8b90ad0c20e414352be56fcc797dac5724debd6c94a17b24fdce2050854b0c`

Passing receipt:

`runtime/artifacts/k129_r33_h1_persistent_outputs_scale_official_runner_two_prediction_r5_20260803.json`

SHA-256:

`09897bd0ef8308a6695537fd6f36219626bb141318ac4fa735e080b8aa0f2ea1`

## Result

```text
                         R31 parent           R33
steady count             124.239224002B       124.205210818B
steady residual              0.136870367s          0.145233869s
steady effective         137.926260702B       138.728597718B
steady requests               31,041               31,076
prediction hash          0ab299a9...e165      0ab299a9...e165
```

The direct output banks save exactly `34,013,184` counted FLOPs, but add
`8.363502 ms` of steady residual and 35 transport requests.  At the challenge
residual price, that is `0.8363502B`; net effective compute therefore worsens
by `0.802337016B`.

The first diagnostic attempt also established that the active K129 graph has
one repaired-H1 call per prediction, not two.  Thus there is no duplicate
weight-side DPS48 transform to cache in this graph.

Both R33 predictions are byte-identical to R31.  A prior failed gate used a
mistyped expected hash (`0ab299a9e5...`) from a summary; the authoritative R31
receipt and the passing R33 receipt both contain
`0ab299a9b5a1c9086a701baad61b7a0839186d1fdf42dff26218331066c0e165`.

## Decision

Do not promote or repeat R33 unchanged.  Retain R31 as the exact-engineering
micro frontier.  Persistent `out=` storage is not automatically cheaper under
the released client/server handle lifecycle; require a whole-row residual win,
not just a counted-FLOP reduction.
