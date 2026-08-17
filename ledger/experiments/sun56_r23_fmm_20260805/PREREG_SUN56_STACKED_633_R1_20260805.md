# Sun-56 stacked 6x3x3 contraction R1

Date: 2026-08-05

Evidence sought: target-free static **projection** on the sealed R26/R31
product census.  No estimator, target, FlopScope session, package, upload, or
submission is involved.

## Prior-art boundary

The capsule's exact-contraction census screened 167 public ternary schemes at
catalog commit `98ba522db92b74f1f8c561a78038ff3091356d73`.  It includes the
rank-43 `6x3x3` scheme with phase additions `34/48/78`, the incumbent rank-40
R40 `108/54/132`, and older rank-23 3x3 algorithms.  It predates Yinqi Sun's
30 April 2026 exact rank-23 3x3 straight-line program with phase additions
`13/13/30` (arXiv:2604.27645), and the capsule contains no `Sun56`,
`56-addition`, or equivalent `26/13/60` stacked spelling.

## Construction

Apply Sun's exact 3x3 algorithm independently to the top and bottom 3x3
blocks of a 6x3 left operand and share its single transformed 3x3 right
operand.  This yields an exact 6x3 by 3x3 bilinear scheme:

```text
rank = 46
A additions = 2 * 13 = 26
B additions = 13
C additions = 2 * 30 = 60
```

The audit must first verify the printed Sun program and its stacked 6x3
identity on deterministic integer matrices.  It then adds this scheme to the
existing exact leaf-plan search and prices the actual sealed product groups.

Promote only if it yields a strictly positive global count saving at no more
than `+0.5s` projected backend wall, or at least `1.0B` count saving at no
more than `+2.0s`.  Otherwise kill this exact scheme for the current chassis.

