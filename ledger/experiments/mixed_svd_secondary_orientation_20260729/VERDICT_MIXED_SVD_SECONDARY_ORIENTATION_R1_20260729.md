# Mixed SVD secondary orientation R1 — killed

Date: 2026-07-29.

Evidence label: **broad statistical** for the fixed Full100 and Generated64
accuracy comparison. Operation economics are a **projection**. This was not
a FlopScope physical row, measured whole, package, upload, remote action, or
submission.

## Fixed candidate

The exact K146/m17 identity was retained. Within the 17-basis O1 arm, phase
positions `0,4,8,12` used the canonical descending eigenframe of
`W0.T @ W0`; the other 13 positions used the incumbent rotation. There was
no position, support, coefficient, or family grid.

The candidate-only CUDA capture reused the exact frozen incumbent Full100
and Generated64 indices and baseline prediction archive. All 164 candidate
predictions were finite and sealed before target access.

## Broad result

| family | pooled raw ratio | row-ratio p95 | improved rows | gate |
|---|---:|---:|---:|---|
| Full100 observed | `0.975588` | `1.344236` | `51/100` | fail |
| Generated64 noise-corrected | `0.943514` | `1.307408` | `36/64` | pass |

The required thresholds were pooled ratio `<=0.95`, row-ratio p95
`<=1.50`, and improved fraction `>=55%`, separately on both families.

The candidate improved the Full100 mean by `2.44%`, but missed both the
material pooled-improvement and majority-row gates. Generated64 improved by
`5.65%` and passed all three requirements. The family asymmetry is too large
to promote.

## Same-K economics

Total propagated support remains exactly K146. Using the released FlopScope
0.9.1 formulas, a direct lawful implementation adds approximately:

```text
W0.T @ W0                              33,488,896
eigh at 9 n^3                         150,994,944
two extra frame-by-weight matmuls      66,977,792
selected-phase elementwise upper bound   524,288
total before small sort/index ops     251,985,920
```

That projected count increment is `0.1750%` of the R17 steady counted work.
It does not change the statistical verdict.

## Verdict

Kill this exact four-phase mixed orientation. Do not spend a physical,
packaging, or remote lane and do not tune phase positions or a blend
coefficient from these opened targets.

## Artifact hashes

- preregistration:
  `8adff47843d9fdbde82046686f761fd9c01d8285a7cf636f7a7bc2822299af44`
- capture source:
  `5dbbe0a525070b06fe2ef564fceeb0ff8e5cb3de9c58b5e3e5d211b0276a03cd`
- target-free prediction archive:
  `4c39628335afc2aeaa1a8a382ac4ef6556d57e7991c0474e06fb5dc7fc9d3c86`
- target-free manifest:
  `906a1b381a3646736611a7db4b59a9d12852c018e4fbc22dba8930bc07c2a778`
- scorer:
  `aaeee5446a30379652f2ae9139ace1f83df6c8f76b1dc049c9ec4cf07cb47476`
- score arrays:
  `ca7b59840fea9c388fbbd0cf3fada8e5a1522e3c6268c30fd98187186a3dc8ff`
- score report:
  `64b148dddf2f21a061b30a41a8ade2fca1e7cbac947dca22aec7e6c0b312f345`

