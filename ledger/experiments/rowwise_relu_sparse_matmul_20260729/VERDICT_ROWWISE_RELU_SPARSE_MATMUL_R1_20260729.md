# Rowwise ReLU sparse matmul R1 — static verdict

Date: 2026-07-29  
Status: **killed by the released 0.9.1 cost formulas**  
Evidence: **component** numerical check plus **projection** economics  
Estimator/physical/package/remote actions: **none**

## Outcome

Do not implement or benchmark this rewrite on the current K146 graph.

The decisive cost is the row-specific gather of weight rows.  For a dense
`(B,N) @ (N,M)` contraction, FlopScope 0.9.1 charges

```text
dense = B M (2N - 1).
```

Give the sparse method an impossible oracle which supplies every active
index and value for free.  With `P` active inputs per row it must still pay:

```text
row-specific W gather = 4 B P M
binary contraction     = B M (2P - 1)
oracle sparse total    = B M (6P - 1).
```

Therefore it beats dense only when `P < N/3`.  This conclusion already
ignores support discovery, active-value gathering, padding, bucketing,
sorting/scattering, scalar fetches, extra calls, and residual.

Including just one `nonzero` scan and the mandatory active-value gather gives

```text
B N + 4 B P + 4 B P M + B M (2P - 1),
```

whose strict break-even point at `N=M=256` is `P < 84.945`, or `33.18%`
activity.  ReLU states are near `50%` active, on the wrong side by a large
margin.

## Target-free numerical capture

The analysis opened only Full1000 `weights.npy` row zero.  It propagated 512
fixed float32 Gaussian rows through the actual 32-layer, width-256 Full0
weight draw and captured exact post-ReLU states.  This is a representative
target-network activation capture, not a claim that these rows are the
current structured K146 trajectory.  The economic inequality itself does
not depend on that distinction.

| post-ReLU layer | zero fraction | mean active | support patterns |
|---:|---:|---:|---:|
| 1  | 49.84% | 128.422 | 512 / 512 |
| 8  | 52.51% | 121.576 | 512 / 512 |
| 16 | 50.08% | 127.783 | 512 / 512 |
| 24 | 51.06% | 125.287 | 511 / 512 |
| 30 | 53.33% | 119.469 | 508 / 512 |

Even the oracle sparse ratio was `1.401x--1.506x` dense over those five
states.  The still-optimistic official lower bound was `1.406x--1.512x`;
8-wide bucketing was `1.443x--1.555x`; fixed-`Pmax` padding was
`1.497x--1.791x`.

At exactly half activity, every production-relevant width pair gives the
same controlling result:

| `(N,M)` | dense, B=6016 | oracle sparse | increase | 99 MiB splits |
|---:|---:|---:|---:|---:|
| `(256,256)` | 0.787B | 1.181B | +0.394B | 8 |
| `(232,232)` | 0.646B | 0.970B | +0.324B | 7 |
| `(216,216)` | 0.560B | 0.841B | +0.281B | 6 |
| `(200,200)` | 0.480B | 0.721B | +0.241B | 5 |
| `(176,256)` | 0.541B | 0.812B | +0.271B | 6 |

The current K146 receipt contains `112.816B` of matmul work.  Applying the
observed ratio to all of it is only a deliberately broad projection, but it
shows the scale: approximately **+52B**, not the required `-15.014B`.  The
stronger statement is per contraction: for every row with activity above
one third, this spelling has the wrong sign before residual.

## Result cap and wall

For one current 6,016-row, width-256 chunk at half activity, the gathered
`(B,P,M)` weight tensor is `788,529,152` bytes.  The campaign cap is
`103,809,024` bytes, forcing at least eight subcalls.  Other relevant widths
force five to seven.

The kill assumes **zero new residual**.  In reality each former contraction
becomes selector work, one or more gathers, and one or more contractions;
data-dependent exact-count bucketing also needs many groups.  Materializing
row-replicated weights is adverse for backend wall as well.  A component
benchmark cannot reverse the already-positive analytical count delta, so
none was run.

## Column-oriented alternative is not a participant surface

Transposing the sparse loop is the one algebraic way to avoid duplicating
weight rows: for each input column, form outer products only for the active
trajectory rows and scatter-add them into the output.  Native in-process
NumPy can spell that with `np.add.at`.

The released participant client cannot.  `fnp.add` is a plain remote proxy
without the ufunc `.at` method, raw NumPy is unavailable, and
`RemoteArray.__setitem__` explicitly rejects indexed in-place updates.
Materializing one sparse contribution matrix per input column and combining
it through legal whole-array operations restores at least the dense
multiply/add count before selector and movement costs.  Thus this transpose
does not rescue the proposal on the actual grader surface.

## Numerical association

The sparse formulas are algebraically exact because only exact zeros are
removed.  They are not float32 byte-identical to dense BLAS because the
accumulation association changes:

- padded/bucketed binary einsum: relative RMSE
  `1.47e-7--1.57e-7`, maximum absolute `3.34e-6--5.72e-6`;
- row-ragged matmul: relative RMSE `1.90e-7--2.03e-7`, maximum absolute
  `2.86e-6--7.63e-6`.

Association would therefore require a new whole prediction freeze if the
economics were favorable.  It is not.

## Reopen condition

Reopen only for an execution surface that invalidates the mandatory
row-specific weight-gather charge, or for a proved production region below
one-third activity with enough dense contraction work to matter.  Neither
condition exists on the current legal FlopScope NumPy surface.

## Artifacts

- analysis source SHA-256:
  `d35c5e9e37a0b4e47902138f97d8a21aa36cce68581950f2006d4f4e38268674`
- component/projection receipt SHA-256:
  `a72d714da3fe5030a8a81594f77f64edd1f90ba3aaaf4ad21b24d94b4d3ecd88`
- Full0 weight-row SHA-256:
  `ab288d09e5cfb354b3d644f3c754c14c4f0ee3e9c75d33ec5342d982abfd666a`

No target member, score, estimator, FlopScope session, physical row,
package, upload, or remote action was used.
