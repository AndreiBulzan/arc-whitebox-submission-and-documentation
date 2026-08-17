# Preregistration: WCB whole-inner PLinOpt compiler C10-R1

Date: 2026-08-09.

## Scope and prior art

This is an offline, target-free exact-algebra component and projection. It
opens no target, MLP, FlopScope session, benchmark, physical row, Mini100 row,
package, upload, submission, or remote action.

Capsule searches covered whole-inner, direct U/V/W, signed CSE, PLinOpt,
addition reducer, alternative basis, tensor product, and R40/B7. The nearest
attempts are:

- C0-R1, a bespoke direct signed-pair CSE whose search did not complete and
  produced no receipt;
- C0-R2, which killed exact duplicate reuse only;
- C4, which ran PLinOpt on separate two-level change-of-basis maps, not on
  the complete mixed inner factors;
- C7, which ran a ternary reducer on one-level R40 factors only.

C10 applies an independent verified compiler directly to the complete

```text
<6,3,3:40> tensor <2,2,2:7> = <12,6,6:280>
```

U, V, and W linear maps. Outcome: **materially new compiler surface**.

## Static ceiling

The current separable inner counts are:

```text
U=590, V=407, W=848 additions.
```

Across the live outer R40 and thirteen R90 products, one saved inner node is
worth:

```text
U: 18 * sum_layers(r*c) = 2,660,364 operations
V:  9 * 7,988            =    71,892 operations
W: 40 * sum_layers(r*c) = 5,911,920 operations.
```

Thus a W-only 1.5B gate needs about 254 saved W nodes (`848 -> 594`), while a
joint U/W result may share the burden. This is difficult but not excluded by
the naive direct maps, so the bounded compiler oracle is licensed.

## Frozen inputs and toolchain

- live R87 Tichavsky R40 schedule;
- live B7 factors;
- PLinOpt commit `656c3167a025518ea0451e44ada56fa5f8663051`;
- exact integer mixed-factor matrices, constructed only after R40, B7, and
  mixed multiplication tensors verify coefficient-for-coefficient.

For each of U, V, and W run three deterministic PLinOpt spellings:

```text
-D -G, -D, -G
```

with 64 optimizer trials and a 900-second timeout per spelling. Every emitted
SLP must pass `SLPchecker -M` against its exact source matrix. Select the
smallest verified addition count.

## Gates

- Promote the inner compiler surface if the exact live-shape projection is
  at least `1.5B` operations.
- Record a direct R90 rewrite gate at `3.6B` and implementation headroom at
  `4.5B`.
- A result below `1.5B` closes this independent direct whole-inner compiler
  spelling. It does not prove a tensor-rank lower bound or close a
  nonmultiplicative/serendipitous full `<72,18,18>` search.

