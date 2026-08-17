# K129 mixed64/D6 unique-frame R1 verdict

Date: 2026-08-03

Decision: **promote the exact component into the whole sidecar**.

Evidence label: **component**.  This is not a whole-estimator receipt, broad
score, package, or remote result.

## Physical result

The preregistered production-shape A/B passed under the released FlopScope
0.9.1 client/server over the byte-faithful pipe:

```text
two literal 64-frame calls       4,320,133,120 counted
two unique 3-frame calls           234,487,808 counted
exact saving                     4,085,645,312 counted
expected saving                  4,085,645,312 counted
output 0                         byte-identical
output 1                         byte-identical
largest output                      16,777,216 bytes
```

Receipt:
`runtime/artifacts/k129_mixed64_d6_unique_frame_component_r1_20260803.json`

Receipt SHA-256:
`01ce513cac4bb912fa788ffa1195da58deaa5149756700e2444ccf0f1e1b5cd3`

Source SHA-256:
`1f4cfc5b8de70ec71d88f5e4c7da6c14eb93dddd6584006785cdf861e6e9138a`

## Score implication, still projection

Applying this exact counted saving to the R31-adjusted mixed64 projection
moves projected effective compute from `157.361997104B` to
`153.276351792B`.  At unchanged sealed raw MSE, the central adjusted
projections become:

```text
Full100       1.025117e-7
Generated128  1.088909e-7
```

These are **projections**, because the full physical mixed64/D6 sidecar has
not yet been implemented.  The rewrite leaves approximately `1.561254334B`
of effective-compute headroom beneath the preregistered conservative
Generated-family `1.10e-7` threshold.  The next task is therefore narrowly
defined: implement the remaining sealed statistic without spending that
headroom.
