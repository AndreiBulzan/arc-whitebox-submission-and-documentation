# K129 sparse-O1 packing: static implementation and economics verdict

Date: 2026-07-29  
Evidence label: **component** for the source-static ledger; **projection**
where residual time is discussed.  
Actions deliberately not performed: estimator implementation, physical row,
GPU run, package build, upload, or remote submission.

## Bottom line

Appending one or two selected O1 bases to R21 is economically viable, but it
is not a zero-request append.

* The repaired H2 rows and layers 7--23 can use the existing row-parametric
  carriers.  In particular, the middle is one combined O0+O1 execution; it
  does **not** duplicate the full middle arm.
* The exact captured sparse-O1 trajectory cannot share the late products
  with O0.  Its energy selection and selected right operands are different.
  It therefore needs a small, separate layers-24--30 arm.
* M267 needs one extra leaf cell for either one or two O1 bases.
* Only final raw moments plus the Edgeworth3433 vector are needed.  The
  production q1 gamma readout should not be instantiated.

Against the steady R21 ledger of `127,347,663,042`, the exact proposed direct
readout deltas are:

| selected O1 bases | counted delta | projected whole count | billed operation-call delta |
|---:|---:|---:|---:|
| 1 | **1,077,206,668** | **128,424,869,710** | **1,752** |
| 2 | **2,037,319,878** | **129,384,982,920** | **1,746** |

The call discontinuity is real: the frozen middle scheduler uses six fewer
calls for two bases than for one.  Count still rises normally.

## Exact counted decomposition

The direct readout is

```text
q0 + 0.0734 * (Edgeworth3433(raw O1 moments) - q0)
```

and deliberately does not calculate q1 gamma.

| component | one basis | two bases | source-static mechanism |
|---|---:|---:|---|
| structured H1/H2 plus repair | 138,091,008 | 175,643,136 | actual lower-K source graph; includes the fixed O1 rotation path |
| layers 2--6 O1 front | 208,723,700 | 409,328,308 | actual one-chunk arm graph plus the current-owner fixed term |
| layers 7--23 combined middle | 498,633,560 | 990,988,882 | exact ShapeLedger difference at 66,560 / 67,072 rows |
| layers 24--30 separate O1 late arm | 200,491,200 | 398,860,224 | exact M248 formula after prepared-right sign-pair CSE |
| layer 31 M267 extra cell | 30,314,624 | 60,629,248 | exact row-linear M267 formula |
| aggregate O1 raw moments | 919,552 | 1,837,056 | `7N + 2,048`, `N = 131,072m` |
| Edgeworth3433 plus outer blend | 33,024 | 33,024 | 126 weighted vector units plus three 256-wide blend ops |
| **total** | **1,077,206,668** | **2,037,319,878** | sum |

The corresponding operation-call decomposition is:

| component | one basis | two bases |
|---|---:|---:|
| structured H1/H2 plus repair | 57 | 57 |
| layers 2--6, including the fixed L4 Edgeworth snap | 777 | 777 |
| combined middle delta | 4 | -2 |
| separate late arm after right CSE | 798 | 798 |
| one M267 leaf cell | 51 | 51 |
| raw moments, Edgeworth3433, outer blend | 65 | 65 |
| **total** | **1,752** | **1,746** |

The final readout count assumes a direct FlopScope spelling that reuses
`m1²`, `variance²`, and `threshold²`, uses `stats.norm.pdf/cdf`, and avoids
`power` for integer powers.  Its static count is:

```text
raw aggregate       917,504*m + 2,048
Edgeworth3433                     32,256
outer blend                            768
```

This is the proposed implementation ledger, not a claim about the more
expensive historical gamma readout.

## Why the late arm cannot be packed lawfully

The O0 and sparse-O1 late states choose columns from their own propagated
energy.  Consequently their `selected` arrays and selected right transforms
are different.  Concatenating their row operands into one matmul would
require one of:

1. using O0's selected columns for O1, which is a different estimator from
   the captured accuracy candidate;
2. using a padded/block-diagonal product, which erases the sparse economics;
3. duplicating the full late call graph inside a nominally packed wrapper.

The third option is only relabelling, not packing.  The minimal exact design
therefore keeps the 798-call sparse late arm and stops there.

## What can and cannot be packed

Layers 7--23 are already packed in the useful sense: the existing lifecycle
middle is constructed at `O0_ROWS + O1_ROWS`, and the R40/cell-wave graph
executes once.

Layers 2--6 could experimentally concatenate the O1 rows onto O0's final
short chunk:

```text
O0 short chunk       2,944
+ one-basis O1         256  -> 3,200
+ two-basis O1         512  -> 3,456
backing capacity                 6,016
```

That may remove most of the roughly 650 front call sites, but it changes the
leading row extent seen by the public products.  The reduction dimension is
unchanged, so the symbolic association is preserved; byte identity is not
provable statically because backend kernel selection can depend on `M`.
This optimization should only be tried after the sparse accuracy screen
passes.  It is not part of the exact ledger above.

## O0 association

With a separate front, late arm, and final cell, all O0 destinations and O0
late/final operation ordering remain unchanged.  The combined middle has a
larger leading row carrier, so:

* the O0 mathematical operations and reduction axes are unchanged;
* the established lower-K row-parametric graph is reused;
* byte identity still needs one target-free association gate.

Thus “prediction-preserving” is source-structural here, not yet a byte-level
receipt.  If byte identity were made mandatory, a separate sparse middle
would be required and would duplicate the middle call graph—the explicit
stop condition for this lane.

## Memory

The exact separate-owner design remains within both caps:

| owner | one basis | two bases |
|---|---:|---:|
| combined post-23 dense `(rows, 200)` | 53,248,000 B | 53,657,600 B |
| sparse late state `(512m, 200)` | 409,600 B | 819,200 B |
| sparse final `(512m, 256)` | 524,288 B | 1,048,576 B |
| one same-shape raw-moment temporary | 524,288 B | 1,048,576 B |

No existing public backing must grow.  The campaign's largest existing
single public result remains `103,809,024` B, exactly 1 MiB below the
`104,857,600` B hard cap.  The sparse owners are separate and small; global
live memory remains far below 8 GiB.

## Residual projection

The exact source design adds about 1.75k billed operation calls.  Applying
the recent approximately 4.7 microsecond/request residual calibration gives
only a **projection** of roughly `0.008--0.012 s`, or `0.8--1.2B` effective
work.  It must not be promoted to a receipt.

Two bases are therefore more request-efficient than one: almost twice the
counted propagation, but the same fixed call shell.  Accuracy—not runtime—is
the correct discriminator.

## Recommendation

Do not implement before the target-free capture decides whether direct,
matched-moment, or matched-readout control variates transfer.  If direct
one- or two-basis Edgeworth survives that screen, implement the separate
route above first.  It is small enough to matter and avoids duplicating the
full arm graph.  Consider front-short-chunk fusion only as a second,
independently gated wall/residual optimization.

