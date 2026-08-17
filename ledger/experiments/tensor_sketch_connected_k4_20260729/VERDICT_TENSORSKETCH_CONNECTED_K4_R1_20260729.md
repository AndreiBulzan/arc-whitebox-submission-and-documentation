# TensorSketch connected-K4 R1 verdict

Date: 2026-07-29

Evidence label: **component**. Cost statements are **projection**. No target,
GPU, FlopScope, physical row, package, upload, submission, or remote action
was run.

## Decision

**Kill before capture.** A degree-four CountSketch/TensorSketch can answer a
fourth-moment query, but it cannot carry that state through ReLU. The only
lawful way to update it nonparametrically is to retain the particle cloud,
whereupon the sketch is both over budget and information-redundant.

## Exact obstruction

Two symmetric scalar laws were constructed:

```text
A: |X| = 0 or sqrt(2), probabilities 1/2, 1/2
B: |X| = sqrt(1/2) or sqrt(3), probabilities .8, .2
X has an independent fair sign.
```

Both have identical raw moments through degree four:

```text
E[X], E[X^2], E[X^3], E[X^4] = 0, 1, 0, 2.
```

Therefore their complete degree-four tensors, cumulants, and every linear
CountSketch/TensorSketch of those objects are identical. After ReLU:

```text
                         law A          law B
E[ReLU(X)]             0.353553       0.456048
Cum4(ReLU(X))         -0.093750       0.0324874
```

Thus even the **uncompressed** moment state through order four does not
determine its ReLU successor. A sketch cannot. Embedding this law in one
coordinate proves the width-256 case. This meets the preregistered immediate
kill condition: the update requires distributional/support information
beyond the K4 sketch.

## Cost boundary

The exact symmetric degree-four state at width 256 has

```text
choose(259,4) = 183,181,376 entries
float32 bytes = 732,725,504
```

Merely producing one arithmetic output per state entry across 32 layers is
`5.861804032B`, already above the complete `5B` ceiling before dense tensor
transport, ReLU, centering, queries, layout, or residual. A quadratic
polynomial ReLU escape raises the needed input state to symmetric degree
eight, `choose(263,8) = 509,850,594,887,712` entries.

Retaining samples avoids the nonclosure, but the existing actual-K32 cloud
is already projected at `31.5B` before adding a sketch. It also makes the
sketch strictly dominated:

```text
<E[h^tensor4], w^tensor4> = E[(h.w)^4].
```

For a retained cloud, `h.w` is already the computed next preactivation.
Its empirical fourth moment is exact; TensorSketch estimates the same
quantity with additional hash-collision variance. It creates no new
all-distinct signal and reduces no propagation work.

## What the local evidence says

The lane was worth checking because the capacity is real:

```text
Full100 omitted 3/4-distinct K4 energy       1.687081e-3
Full100 pairwise-retained K4 energy          0.350300e-3
Full1000 exact marginal K3+K4 final MSE       2.366837e-8
```

The missing signal is not small. The negative result is specifically about
state transport: fourth-order sketches do not survive coordinatewise ReLU
without a particle/distribution representation.

This does not close nonpolynomial random-feature states whose ReLU update is
itself analytically closed, if such a state is found. It does close the
proposed higher-order CountSketch/TensorSketch recurrence and any width/rank
sweep of the same state.

## Artifacts

```text
preregistration
  PREREG_TENSORSKETCH_CONNECTED_K4_R1_20260729.md
  ef6e18fcd5258f9d7be37820fa70db95b303bef1dba6a5e5d6a138320dd72a91

static source
  audit_tensor_sketch_connected_k4_r1_20260729.py
  3bdbe51d6abd284f597b258b65964381a1edac92dba98ed35aa70cf23d3f8a59

static receipt
  tensor_sketch_connected_k4_r1_static_receipt_20260729.json
  6728eaa67f817dab65d326f6e4847ca2248e2c32419a351576559f1e2375db4b
```
