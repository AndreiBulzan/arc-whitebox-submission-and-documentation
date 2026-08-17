# Preregistration: R92 W1 extra-B7 component gate

Date: 2026-08-10

Evidence boundary: target-free **component** measurement first.  No whole
estimator, Mini100 target, package, upload, remote action, or submission is
authorized by this experiment.

## Question

Can the exact reduced rank-7 `2x2x2` identity replace the direct physical
leaf products inside R92's live R40 W1 carrier without residual/request and
movement charges erasing its arithmetic saving?

The direct even-core product is

```text
(918 x 12) @ (12 x 12) = 253,368 charged arithmetic
```

and the direction-correct reduced rank-7 circuit is

```text
7 * ((459 x 6) @ (6 x 6)) + 4/4/7 transforms = 242,496
```

for an algebraic saving of `10,872` per logical leaf.  R92 has 1,600 R40
pairs, seven W1 products, and thirteen live R40 layers.  The five prefix
layers use a 13-column leaf; there the same identity is applied to the
12-column core while the one-column/right-row fringes remain direct.  The
fringe arithmetic is unchanged, so the algebraic saving remains `10,872`.

The count-only ceiling is therefore

```text
10,872 * 1,600 * 7 * 13 = 1,582,963,200
```

before charged bank materialization and transport residual.

## Blocking prior-art preflight

Queries covered `B7`, `rank-7`, `Strassen`, `Winograd`, `rank49`, `W1`,
`leaf recursion`, `cellwave`, `consumer matmul`, `vecmat`, `whole composite`,
and the exact `(918,12,12)` production geometry.

Nearest controlling artifacts:

- `k129_bilinear_count_breakthrough_20260729`: source-static proof of the
  `1.5829632B` algebraic ceiling and warning that an ordinary binary executor
  loses on requests.
- `k129_r91_r90_densecube_b7_20260808`: five isolated dense cubes saved
  counted arithmetic but regressed effective compute by `563.7M`, because
  918 added requests cost `5.891ms` residual.
- `k129_r90_r89_h1_b7_leaf_20260808`: a larger, amortized B7 leaf rewrite was
  effective-positive and Mini100-safe, so the negative is not universal.
- `r90_wcb_comp_20260809`: duplicate-only and PLinOpt whole-inner compilers
  failed; an alleged axis-hybrid saving was proven gauge-incompatible.

Reopen outcome: **materially new execution surface**.  This gate applies one
vectorized B7 bank to the heavily repeated live W1 leaf rather than to five
dense cubes or through the old scalar/binary executor.  It does not reopen
the invalid axis hybrid.

## C0 component spelling

Test one vectorized B7 level on both lane multiplicities for the largest
current prefix and recurrent logical batches:

- prefix representatives: leading batches `(1,19,26)` and `(2,19,26)`,
  leaf `918x13x13`;
- recurrent representatives: leading batches `(1,19,28)` and `(2,19,28)`,
  leaf `918x12x12`.

The prefix spelling uses B7 on the 12x12 core and direct rank-one fringes.
All ordinary public results must remain at most 100 MiB.

Record exact counted arithmetic, operation/request counts, residual, wall,
largest result, and numerical displacement against direct `matmul` under the
pinned FlopScope 0.10 client/server pipe.

## Gates

1. Numerical relative RMSE no larger than `1e-5`, all finite.
2. Every ordinary result at most `104,857,600` bytes.
3. Candidate counted arithmetic lower for both representative geometries.
4. Production-weighted count projection remains at least `1.0B` after all
   charged movement exposed by the component.
5. Estimated added request residual must leave a plausible positive whole
   margin.  A full implementation is forbidden if the component projects
   nonpositive effective saving even before whole-estimator orchestration.

The request projection uses the same-runtime frozen R91 receipt price of
`5.8910725 ms / 918 requests`; the subsequent whole ABBA remains controlling
if this component gate passes.

Only a C0 pass authorizes writing the bounded whole-estimator patch.  A whole
candidate would then need a quiet R92/candidate ABBA with at least `0.25B`
effective saving before any Mini100 run.  Because association changes, no
accuracy evidence may be inherited by identity.
