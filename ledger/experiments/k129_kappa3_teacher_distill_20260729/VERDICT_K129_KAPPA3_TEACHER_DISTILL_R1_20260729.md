# K129 κ3 pair-residual teacher distillation R1 — feasibility verdict

Date: 2026-07-29 (Europe/Bucharest)

Evidence label: **source-static audit** for the public-data inventory and
the algebra below; **projection** for deployment arithmetic.  No target was
opened, no FlopScope or physical row was run, and no package, upload, or
remote action occurred.

## Decision

**Do not start an offline training run from the compact public files.**  The
public κ3 summaries that are small enough to acquire (`tn`, `fn`) do not
contain the pair-residual state required by the proposed teacher.  The exact
teacher requires the 40.47 GB `rn` pair-residual file and matching 83.89 GB
weight file; neither is present in this capsule.  In addition, the broad K129
captures retain final predictions, not the per-basis O0 summaries required by
the proposed student.

This is not an accuracy failure of the κ3 mechanism.  It is a hard
information/provenance boundary: replacing the missing pair residual by
`tn`/`fn` would no longer test the advertised teacher, and replacing the O0
state by final summaries would reopen the already-killed learned endpoint
residual family with less information.

## Frozen minimal falsifier, if the exact public assets are made available

For a late post-ReLU layer `l`, unpack the symmetric pair residual
`R_l = rn[l]` and form the output-relevant variance innovation using the
realized next weight `W_{l+1}`:

```text
t[j] = (W[:, j]^T R_l W[:, j])
```

The student is a single output-permutation/gauge-equivariant, no-intercept
linear map from **already-paid** K129 O0 per-basis final-hidden reductions
and the corresponding realized final-weight column summaries to `t[j]`.  Its
only design degrees of freedom are frozen here:

- late layer: `l=30` only;
- O0 state: 32 fixed target-free Walsh contrasts of the 129 per-basis
  final-hidden signed means, transported by the realized final weight;
- weight features: per-output column norm and the already-computed signed
  preactivation/second-moment scalars;
- student: ridge, no intercept, one coefficient vector shared across output
  coordinates; ridge selected only on a public-training split;
- deployment correction: an analytic delta-method conversion of `t` to the
  final ReLU mean, clamped to the existing analytic second-moment bound.

This spelling is deliberately smaller than the previous 1,889-parameter
K146 basis decoder.  It has a conservative incremental deployment arithmetic
ceiling below `0.5B`: 32 length-129 Walsh reductions and a fixed-width
coordinatewise linear map are charged, while the per-basis O0 state and
final-weight products are already in the K129 graph.  It is not a meter
receipt.

The predeclared continuation gate is the public-scout gate: pooled raw MSE
ratio `<=0.970616` on both disjoint Full and Generated, row-ratio p95
`<=1.10`, no family reversal, and a measured increment `<=0.5B`.  One failed
sealed gate closes this exact spelling without tuning.

## Why the compact files cannot substitute for `rn`

`tn` is a repeated-index κ3 trace and `fn` a κ3 fiber norm.  Both are
neuronwise compressed statistics.  The teacher needs the quadratic form of
the *pairwise covariance residual* for each output column.  There is no
function of just a trace and a norm that determines
`W[:,j]^T R W[:,j]` for arbitrary realized `W`; different residual matrices
have the same `tn`/`fn` and different quadratic forms.  The compact κ3 files
therefore cannot produce labels for this test.

## Exact unavailable public assets

Joint-feature repository revision: `b8ddb8705805e9dec5b4a09d987e5112fd0030e3`.
Matching-moments repository revision: `1814cf17b9c22f8d39e57274cf7995081fc8823d`.

| required file | primary URL | bytes | LFS SHA-256 |
|---|---|---:|---|
| pair residual teacher `rn_train.npy` | `https://huggingface.co/datasets/keenanpepper/whestbench-relu-mlp-jointfeats-10k/resolve/b8ddb8705805e9dec5b4a09d987e5112fd0030e3/rn_train.npy` | 40,473,600,128 | `f20a0ec0e410c07f8a0d8345ec335fea1fc91cec1565819bbc6e29d6c3494ac9` |
| matching weights `weights_train.npy` | `https://huggingface.co/datasets/keenanpepper/whestbench-relu-mlp-moments-10k/resolve/1814cf17b9c22f8d39e57274cf7995081fc8823d/weights_train.npy` | 83,886,080,128 | `99d0aded0b053acd783ff129289bf5cba2d9916ff1db6ff088776ed10075d141` |
| optional target cross-check `mean_train.npy` | `https://huggingface.co/datasets/keenanpepper/whestbench-relu-mlp-jointfeats-10k/resolve/b8ddb8705805e9dec5b4a09d987e5112fd0030e3/mean_train.npy` | 327,680,128 | `c0cb9020b8117d36562b8b93fbab84efb3b4342665c877cfb4e515d810aab471` |

The teacher-plus-weight acquisition is **124.36 GB** before the new K129
production-state capture (**124.69 GB** if the public mean cross-check is
also acquired).  It is not a quick local falsifier.

An intentionally small range probe is technically conceivable (16 MLPs:
64,757,760 payload bytes of `rn`, 134,217,728 payload bytes of weights, plus
about 0.5 MB of means), but cannot establish the specified disjoint
train/development/held/Generated gate and still needs a fresh K129 O0-state
capture for those same public weights.  It would be plumbing evidence only,
not a valid accuracy falsifier, so it is not authorized by this result.

## Relation to existing negatives

The frozen design uses a genuinely new label (`W^T R W` from public pair
residuals), so it is not logically identical to the failed endpoint ridge or
basis decoder.  However, without that exact label it collapses to less
information than the following already-closed routes:

- the 156-feature learned endpoint correction reversed on disjoint Full and
  Generated;
- the 1,889-parameter K146 fixed-basis decoder was `0.9930` Full but
  `1.0037` Generated on its sealed gate;
- the 48-coefficient source-residual head reversed strongly on both sealed
  families.

Accordingly, no estimator source was created and no local score was claimed.
