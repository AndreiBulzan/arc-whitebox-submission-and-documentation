# Preregistration: direct accurate FMMa hand schedule C9-R1

Date: 2026-08-09

## Scope and correction from C8

Offline, target-free, exact source reconstruction and projection only. C8
identified that `FMMa633_mul_6_3_3.m` is the sparsified inner-basis kernel;
it is not a standalone matrix-multiplication tensor. C9 instead parses the
separate direct accurate source `FMMa_6_3_3.m`, which is documented and
called as the standalone 6x3 by 3x3 algorithm.

No target, MLP, FlopScope session, benchmark, physical row, Mini100 row,
package, upload, submission, or remote action is opened.

## Exact tests

- Parse all rational U/V/W straight-line expressions exactly with Python
  `Fraction` arithmetic.
- Require the reconstructed tensor to equal ordinary 6x3 by 3x3 matrix
  multiplication coefficient-for-coefficient.
- Compare its rank terms with the live scale-eight R40 factors under a common
  rank permutation and exact rational per-rank gauges whose product is eight.
- Record literal/global-scale equality separately.

## Counts and corrected pricing

Record binary combines, scalar multiplies/divides, explicit unary negations,
and aliases per axis. Conservatively charge every non-alias arithmetic node.
Also report a binary-only ceiling, because the production schedule can absorb
some sign and dyadic gauges into metadata/global scaling; that ceiling is not
an implementation receipt.

Reprice the depth-two R40 map with mandatory B7 transforms and rank-11,200
leaf products held fixed. U/W nodes weigh 591,192 operations and V nodes
7,988 operations across the live geometry.

- first static gate: at least 3.6B conservative saving;
- implementation-headroom gate: at least 4.5B conservative saving.

If only the binary-only ceiling passes, preserve the source as a near-gate
compiler lead and require an exact integer rescaling/schedule synthesis before
implementation. Any implementation still requires association, physical and
official Mini100 gates.
