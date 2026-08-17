# K129 late-B7 `matmul(out=)` reuse component preregistration

Date: 2026-08-10

Evidence target: **component** only. No target arrays, Mini100 labels, package,
upload, submission, or remote action is authorized by this experiment.

## Mechanism and reopened prior art

The live R92 graph inherits R31's late B7 owner. Four equal leaves are
evaluated as one `(4, 7, 7, 4128, 24) @ (4, 7, 7, 24, 25)` graph, and the
three remaining leaves as one `(7, 7, 3, 4128, 24) @ (7, 7, 3, 24, 25)`
graph. The existing kernel creates seven large product arrays, one result
array, and one fringe product per graph.

The capsule was searched for `late`, `B7`, `matmul out`, `persistent
product`, `product_bank`, `in-place`, `consumer`, and `destination`. The
nearest work is:

- R31, which batches the four leaves but still creates product/result arrays;
- R32, which removes the two operand stacks but still creates product/result
  arrays and lost physically because seven extra view requests cost more than
  its 0.272883B counted saving;
- the July consumer-repointing audits, which explicitly treated public
  `matmul` as lacking an `out=` destination;
- the dominant R40 leaf, which already uses `vecmat(..., out=...)` and is not
  in scope.

FlopScope 0.10 now exposes public `fnp.matmul(a, b, out=destination)`. That is
a new execution surface relative to the prior negative audits and is the sole
reason this lane is reopened.

## Exact liveness rewrite

Keep the existing Winograd forward and reverse arithmetic. Reuse three
persistent core-shaped scratch arrays and one persistent result:

1. `p1 -> scratch0`; `p2 -> c11`; `c11 = p1 + p2`.
2. `p6 -> scratch1`; overwrite `scratch0` with `u2 = p1 + p6`.
3. `p7 -> scratch1`; overwrite it with `u3 = u2 + p7`.
4. `p5 -> scratch2`; overwrite `scratch0` with `u4 = u2 + p5`.
5. `p3 -> c12`; `c12 = u4 + p3`.
6. `p4 -> c21`; `c21 = u3 - p4`; `c22 = u3 + p5`.
7. Write the 25th-column fringe directly into the result view with
   `matmul(out=...)`, eliminating the explicit `copyto`.

This preserves each scalar add/subtract association and each matmul operand.
The component must nevertheless prove byte equality against the frozen
kernel on both live geometries.

## Gates

The benchmark uses the pinned FlopScope 0.10 pipe transport and the shared
benchmark lock. Persistent destinations are allocated in a separate setup
budget. Four alternating steady rounds are run for each live geometry.

Required:

- byte-identical complete outputs for both geometries;
- identical matmul/add/subtract counts, except exactly one removed `copyto`
  per graph;
- every public result below 100 MiB;
- no increase in public-operation request count;
- median effective-component saving positive in both geometries;
- projected saving over fourteen live late products at least 0.25B effective.

The 0.25B promotion floor is deliberately modest because remote R92's entire
residual channel is only about 16.25B and this component touches one tail.
Even a pass remains a projection until an isolated whole ABBA on a
capsule-native R92 successor.

