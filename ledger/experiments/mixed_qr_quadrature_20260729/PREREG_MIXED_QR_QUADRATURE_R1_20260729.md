# Degree-6 support × mixed-QR quadrature R1

Date: 2026-07-29.

This is a fast target-free falsifier, not a physical, package, submission, or
remote candidate.

## Fixed mechanism

Keep the already frozen target-free degree-6 orientation-1 support exactly:

```text
[3,11,15,17,27,34,36,45,54,64,66,70,78,82,111,115,120]
```

Keep total `K=146`, every production-aligned propagation step, repair,
compression, late selection, readout, and the literal `129:17` arm blend.

For each MLP, compute the reduced QR factorization of the first weight matrix
in float64:

```text
W0 = Q R
```

Canonicalize signs column-by-column so `diag(R) >= 0`, then cast
`frame = Q.T` to float32. At exactly orientation-1 phase positions
`(0,4,8,12)`, replace the ordinary input frame by this QR frame, so
`frame @ W0` is triangular up to the declared float32 replay tolerance.
All other phase positions retain the ordinary production frame.

There is one support, one frame construction, and one phase set. There is no
support, position, coefficient, row, or family grid.

## Frozen scout rows

- Full: indices `(0,1,2,3)`;
- Generated: indices `(2,4,5,6)`.

The capture may open only `weights.npy` and `seeds.npy`. It must serialize and
hash-seal all candidate predictions, arm outputs, indices, and seeds before a
separate scorer opens any target-bearing member.

## Fixed reports and gate

Primary candidate: literal `129/146 : 17/146` blend.

Secondary diagnostic fixed in advance: `0.90*q0 + 0.10*q1`. This diagnostic
cannot by itself promote the primary spelling.

Against the incumbent K146 prediction on the identical rows, report for both
families:

- pooled actual-final MSE ratio;
- row-ratio median, p95, and maximum;
- improved-row fraction and finiteness;
- the remote-320262/persistent-port calibrated adjusted projection, including
  the same conservative `+0.251985920B` frame-construction/runtime increment
  used for the corresponding four-phase mixed-SVD falsifier.

Broaden only if the **literal** candidate simultaneously has, in each family:

1. pooled actual-final ratio `<=0.94`;
2. row-ratio p95 `<=1.50`;
3. row-ratio maximum `<=1.50`;
4. all predictions and losses finite; and
5. worst-family port-calibrated adjusted projection `<=1.2e-7`.

Otherwise kill this exact mixed-QR spelling immediately. Do not run
FlopScope, a physical row, packaging, networking, upload, submission, or
remote evaluation.

