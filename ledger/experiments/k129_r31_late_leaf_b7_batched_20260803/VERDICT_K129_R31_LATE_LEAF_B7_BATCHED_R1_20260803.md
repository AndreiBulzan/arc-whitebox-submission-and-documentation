# Verdict: four-leaf late B7 batching promotes

Date: 2026-08-03

Evidence label: **measured whole** for one initialized and one steady exact
archive row.  Gate timing is diagnostic.  Accuracy remains inherited only by
bit identity; no broad or remote result was produced.

R31 batches the four same-shaped late B7 leaves in each of fourteen products.
It increases counted work slightly but removes 1,876 steady transport
requests.  The fetched prediction is bit-identical to R30.

```text
                              R30                 R31
steady count                 123.965724034B      124.239224002B
steady residual                0.146884703s        0.136870367s
steady effective             138.654194334B      137.926260702B
steady requests               32,917               31,041
prediction SHA-256            0ab299a9...e165      identical
```

Measured-whole diagnostic effective saving: `0.727933632B` (`0.525%`).
Counted saving versus R27 after the complete R28+R30+R31 stack is
`2.166727680B`.  The corresponding same-gate diagnostic effective saving
versus R27 is `2.098293080B`.

Exact archive:

```text
runtime/artifacts/k129_r31_h1_dps48_lateleaf_batched4_scale_local_candidate_r2_20260803.tar.gz
SHA-256 520febd128d61a9015cb0c96a9c69a9a4da9ed2848fc5de06cc4f11bda641f3c
```

Decision: retain R31 as the current exact-engineering micro frontier.  It is
not remotely submitted and no upload is authorized.

