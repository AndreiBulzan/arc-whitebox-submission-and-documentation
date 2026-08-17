# K129 R40 `vecmat(out=)` to `matmul(out=)` preregistration

Date: 2026-08-10

Evidence target: **component** first. No target access, package, upload,
submission, or remote action is authorized.

## Reopened mechanism

The July independent audit measured ordinary `matmul` 3.1--3.4x faster than
`vecmat(out=)` on the R40 leaves at equal charged contraction count. It was
not production-ready because public `matmul` had no destination argument;
fresh results required consumer repointing and changed liveness.

FlopScope 0.10 removes that blocker. The exact live contraction can now be:

```python
fnp.matmul(left_rank, right_rank, out=product_rank)
```

using the same existing persistent product destination. It also avoids the
singleton-axis right view required by `vecmat`.

This is not expected to reduce charged contraction count. It can reduce
residual/request work and diagnostic wall, and the latter may improve
60-second safety. `matmul` and `vecmat` may associate float32 accumulation
differently, so byte identity is not assumed; any whole survivor requires a
fresh official-Mini100 accuracy capture before promotion.

## Exact component geometries and gates

Test the four live leaf envelopes already frozen by the R97 W1 component:

- prefix lane 1/2: `(1|2,19,26,918,13) @ (...,13,13)`;
- recurrent lane 1/2: `(1|2,19,28,918,12) @ (...,12,12)`.

Control creates the production singleton right view inside the timed context
and calls `vecmat(out=)`. Candidate calls `matmul(out=)` with the unsqueezed
right. Both have independent persistent destinations.

Required:

- equal charged count in every case;
- relative output RMSE at most `1e-5`, finite outputs, and cap compliance;
- candidate request count no larger than control;
- candidate diagnostic outer wall lower in every case;
- candidate effective component work lower in at least three of four cases;
- `133 * median(case effective saving)` at least `0.25B` as a conservative
  live screening projection.

Passing C2 authorizes only a capsule-native method wrapper and isolated whole
ABBA. Mini100 remains conditional on the whole result.

