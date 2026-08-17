# K129 within-basis Walsh-state compression R1 — verdict

Evidence label: **component**.

## Verdict

**Kill full-depth Walsh projection at ranks 32, 64, 96, and 128**, for both
fixed low-degree and target-free global-energy mode selection.

The rank-256 implementation check was close (`1.14e-6` maximum output delta),
so the qualitative result is not a transform-order bug.  Every compressed
spelling failed by orders of magnitude:

| spelling | Full2 raw ratio | Generated2 raw ratio |
|---|---:|---:|
| global-energy R128 | 71,216 | 88,763 |
| low-degree R128 | 78,279 | 94,843 |
| global-energy R64 | 162,719 | 189,546 |
| low-degree R64 | 176,387 | 187,768 |

The striking diagnostic is that R128 retained about 99% of spectral state
energy on average.  Repeatedly deleting the low-energy tail still changes
gate crossings, creates new frequencies at the next ReLU, and compounds into
a large bias over 29 projected layers.  Ordinary L2 energy is therefore not a
sufficient importance measure for deep nonlinear propagation.

This does **not** yet kill a confined spectral rewrite of a short expensive
layer window.  R2 may test that distinct question, especially the R40 middle
window, but must not retry full-depth ranks or selector tuning.

Receipts:

- `runtime/artifacts/k129_walsh_state_compression_r1_targetfree_20260806.json`
- `runtime/artifacts/k129_walsh_state_compression_r1_postseal_20260806.json`

