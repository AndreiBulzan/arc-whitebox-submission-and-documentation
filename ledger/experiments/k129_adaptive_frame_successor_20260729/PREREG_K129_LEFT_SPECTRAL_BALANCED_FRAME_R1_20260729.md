# K129 left-spectral balanced-frame R1

Date: 2026-07-29.

Evidence target: a fast ordinary-CUDA accuracy falsifier. Any operation
economics in this document are a **projection**. This protocol authorizes no
FlopScope physical row, benchmark-lane work, package, upload, remote action,
or submission.

## One fixed candidate

Retain the exact K129/O0-only R21 statistic, its cleanup graph, endpoint
lambda `0.0075`, all `129` O0 bases, every downstream operation, and its
readout. Change only 32 of the 128 transformed O0 input bases.

For the supplied MLP's first weight matrix, use

```text
W0 = U diag(sigma) V.T
G  = W0 @ W0.T
H  = the normalized 256-dimensional Walsh matrix
F  = H @ U.T
```

Obtain `U` from `eigh(G)` in the released ascending order. Canonicalize every
column of `U` so its largest-absolute-value coordinate is positive. There is
no eigenvalue sorting, threshold, coefficient, or retry.

The exact phase-array positions replaced by `F` are:

```text
P = (
     1,  5,  9, 13, 17, 21, 25, 29,
    33, 37, 41, 45, 49, 53, 57, 61,
    65, 69, 73, 77, 81, 85, 89, 93,
    97,101,105,109,113,117,121,125,
)
```

These are zero-based positions in the immutable 128-row `phases` array.
Position zero is deliberately excluded: it is the sole stored phase whose
Walsh spectrum is not bent. Every position in `P` has Walsh-spectrum
magnitude exactly `16` in every coordinate.

At both calls to the O0 structured-block constructor:

1. form the incumbent `rotation0 @ weight` and all incumbent phase work;
2. form `F @ weight` once;
3. overwrite only phase rows `P` with the corresponding phase times
   `F @ weight`; and
4. run the existing common native FWHT once.

The standalone identity basis and the other 96 transformed phase bases stay
incumbent. The same frame and the same phase positions must be used in the
initial and repaired-difference calls so the signed pair reconstruction
remains coherent.

## Exact invariant and mechanism

For a substituted phase `p`, its input basis is

```text
B_p = H D_p F = H D_p H U.T,
```

where `D_p` is the diagonal sign matrix for phase `p`. `B_p` is orthogonal,
so its paired signed cloud has exactly zero first moment and exactly the same
second moment as every incumbent basis.

For every `p` in `P`, bentness makes every entry of `H D_p H` have absolute
value `1/16`. Since `U.T W0 = diag(sigma) V.T`,

```text
||(B_p W0)[i, :]||_2^2
  = sum_j sigma[j]^2 / 256
  = ||W0||_F^2 / 256
```

for every row `i`. Thus each substituted basis has exactly equal first-layer
row energy, while preserving its exact signed mean and covariance. The
candidate removes a real fourth-and-higher-order radial defect rather than
matching a fitted target or routing between stored predictions.

This is distinct from all nearby killed classes:

- the killed right-eigenframe used `V.T` from `W0.T @ W0`; `V.T @ W0` has no
  equal-row-energy identity;
- the killed QR frame made `Q.T @ W0` triangular and concentrated, rather
  than flat, leverage;
- degree-6 work changed the O1 support and did not test this O0 frame;
- equal-quota all-eight herding divided finite support among orientations;
  this candidate keeps the complete K129 O0 support and changes no weights;
- active-moment routing selected among fixed orientations from a local
  proxy; this candidate has no selector and enforces an algebraic invariant.

The severe failure of the fixed right-eigenframe therefore does not
invalidate this mechanism.

## Count ceiling

Using the released FlopScope 0.9.1 formulas:

```text
W0 @ W0.T                                      33,488,896
eigh at 9 * 256^3                             150,994,944
one 256x256 native FWHT plus normalization        589,824
two extra frame-by-weight matmuls              66,977,792
32 selected phase multiplies at both calls      4,194,304
-----------------------------------------------------------
arithmetic subtotal                           256,245,760
```

Canonical-sign, indexing, and small movement work must keep the exact
measured count increment below `0.270B`; `0.500B` is the hard rejection
ceiling. Request residual and wall are not included in this projection.

At the R21 remote-mapped price near `146.99B`, a `0.270B` increment raises
the compute multiplier by about `0.184%`. A raw ratio around `0.972--0.976`
would put the existing `1.227--1.232e-7` R21 projection at or below the
`1.2e-7` checkpoint.

## Fast falsification

Run exactly one target-free 12-Full/12-Generated capture after the currently
running right-frame job has released the GPU and shared lane. Freeze and hash
candidate predictions before opening targets. Do not sweep the frame, phase
offset, phase count, coefficient, or row set.

Against the exact R21 prediction on identical rows, continue only if,
separately on Full and Generated:

1. pooled final-layer raw-MSE ratio is at most `0.970`;
2. row-ratio p95 is at most `1.50`;
3. at least `55%` of rows improve; and
4. every prediction is finite.

Failure kills this exact candidate immediately. Passing licenses one
Full100/Generated64 **broad statistical** capture, followed only then by an
exact count implementation and physical validation. It does not by itself
license packaging or remote submission.

