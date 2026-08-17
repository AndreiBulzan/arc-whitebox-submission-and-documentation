# K129 repaired-H1 `matmul(out=)` product-bank preregistration

Date: 2026-08-10

Evidence target: **component**. This is a second, independent component after
the exact late-B7 C0 result. No targets, package, upload, submission, or
remote action are authorized.

## Prior result and mechanism

C0 proved byte-identical destination reuse in both late-B7 geometries, but
projected only 0.171652B effective saving and therefore failed its 0.25B
promotion gate. The remaining live fresh-matmul site with a materially larger
allocation is R90's repaired-H1 third-B7 leaf.

Each repaired-H1 product creates six product banks: five outputs of
`(66,48,1032,8) @ (66,48,8,8)`, each 104,620,032 bytes, and one output with
leading rank 13, 20,606,976 bytes. The five large results sit only 237,568
bytes below the public 100-MiB cap. Four repaired-H1 products execute per
prediction.

FlopScope 0.10 now permits these exact products to write to six persistent
destinations. Matmul operands, arithmetic, association, rank, and count stay
unchanged. This test isolates only fresh result allocation/release versus
`out=` reuse.

## Gates

- byte-identical outputs for the full and tail bank geometries;
- equal matmul count and call count;
- every result at or below the 100-MiB cap;
- median effective-component saving positive for both geometries;
- C1 alone projects at least 0.10B effective saving over
  `4 * (5 full + 1 tail)` calls;
- combined C0+C1 projection reaches at least 0.25B.

Passing components still do not authorize an estimator rewrite. Promotion
requires a separate capsule-native R92 successor and isolated whole ABBA.

