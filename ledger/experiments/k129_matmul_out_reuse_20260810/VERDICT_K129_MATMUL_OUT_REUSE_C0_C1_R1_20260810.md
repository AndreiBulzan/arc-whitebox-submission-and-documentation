# Verdict: `matmul(out=)` works but is not material on R92

Date: 2026-08-10

Evidence label: **component** for the pinned FlopScope 0.10 measurements;
**projection** for complete-R92 savings and score. No whole row, Mini100 run,
package, upload, submission, or remote action was performed.

## Outcome

The new public `fnp.matmul(..., out=destination)` facility works. The two
allocation-reuse rewrites are byte-exact; the dominant R40 primitive swap has
only tiny float32 association differences. None removes enough *charged*
work from the live estimator to justify a production rewrite.

### C0: late B7

The candidate replaces seven fresh product arrays with three persistent
scratch arrays, writes three products directly into final quadrants, and
writes the fringe directly into the persistent result.

Both complete live outputs are byte-identical:

```text
geometry                         output bytes   unequal elements
(4,7,7,4128,25)                  80,908,800          0
(7,7,3,4128,25)                  60,681,600          0
```

Median component deltas, candidate minus control:

```text
geometry                     count       residual       effective
four-leaf bank             -0.809088M     -0.0608ms      -6.889088M
three-leaf bank            -0.606816M     -0.0476ms      -5.371766M
```

The count decrease is exactly the removed fringe `copyto`. Projected over
the fourteen live late products, C0 saves `0.171651956B` effective. It misses
the preregistered `0.25B` gate. Median public requests also rise from 50 to
51 because the destination form changes the release/request mix.

The initial C0 receipt failed only in the harness after both byte comparisons
had passed: v0.10 returns scalar indexing eagerly as `RemoteScalar`, whereas
the old helper expected an array handle. R2 fixes only that receipt logic;
the numerical kernel is unchanged.

### C1: repaired-H1 product banks

The five 104,620,032-byte product banks and one 20,606,976-byte tail bank per
repaired-H1 product were compared as fresh `matmul` results versus persistent
`out=` destinations. Both are byte-identical and have identical counted
matmul work.

```text
bank                         residual delta    effective delta
full 66-rank bank              -0.00589ms       -0.589250M
tail 13-rank bank              +0.00037ms       +0.037200M
```

Across four repaired-H1 products, C1 projects only `0.011636200B` effective
saving. C0+C1 together project `0.183288156B`, below the combined `0.25B`
gate.

### C2: dominant R40 `vecmat(out=)` to `matmul(out=)`

This is the genuinely new primitive combination made possible by v0.10. The
same persistent product destination is retained, while ordinary `matmul`
replaces `vecmat` and the singleton-axis right view disappears.

All four exact live leaf envelopes have equal charged contraction counts,
two fewer requests per call, finite outputs, and substantially lower
diagnostic wall. The association change is extremely small:

```text
case                       relative RMSE       outer-wall delta
prefix lane 1              8.0042e-8              -8.61 ms
prefix lane 2              8.0145e-8             -17.49 ms
recurrent lane 1           8.0186e-8              -7.25 ms
recurrent lane 2           8.0247e-8             -14.53 ms
```

The median residual saving is only about `0.0091 ms` per component call.
Across the 133 live vecmat calls, C2 projects `0.120481375B` effective saving
and therefore misses its independent `0.25B` gate. A simple call-scaled
diagnostic-wall projection is about `1.59 s`, which can improve timeout
margin but is not a score discount because FlopScope backend time is excluded
from effective compute.

## Score boundary

Holding R92's remote public raw error fixed, stacking all three component
projections subtracts `0.303769531B` from its `139.6562318655B` mean effective
compute. The adjusted score changes only conditionally from
`1.1348322218e-7` to approximately `1.1323638218e-7`, a `0.2175%`
improvement. This is a projection, not a receipt, and is too small relative
to residual variability and the memory/integration burden. A literal C0+C1
persistent implementation would reserve about `787,242,624` additional
bytes (`750.8 MiB`).

The live-source census is now closed as follows:

- dominant R40 products already use destination-backed `vecmat`;
- C2 tested their new destination-backed `matmul` alternative;
- the two large fresh allocation families are exactly C0 and C1;
- remaining active fresh matmuls are small rotation, closure, or mean
  contractions and cannot change this ceiling materially.

## Decision

Kill the unchanged production rewrite. Preserve the component kernel and
receipts as evidence that the API is functional and exact, but do not create
an R number, whole row, Mini100 run, or package. Reopen only if a future
estimator introduces a repeated large fresh matmul family or if the scorer
changes how allocator/release work is charged.

Evidence:

- `runtime/artifacts/k129_late_b7_matmul_out_component_c0_r2_20260810.json`
- `runtime/artifacts/k129_h1_product_bank_matmul_out_component_c1_r1_20260810.json`
- `runtime/artifacts/k129_r40_vecmat_to_matmul_out_component_c2_r1_20260810.json`
