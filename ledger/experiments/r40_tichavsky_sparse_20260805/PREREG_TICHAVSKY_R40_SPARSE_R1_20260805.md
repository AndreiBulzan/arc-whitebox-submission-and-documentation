# Tichavsky sparse rank-40 screen

Date: 2026-08-05

## Fixed question

The live K129 graph uses an exact rank-40 algorithm for the
`<6,3,3>` product, with production linear-circuit sizes
`108 / 54 / 132` for the 18-input, 9-input, and 18-output factors.

Tichavsky's published decomposition B of the equivalent
`<3,3,6:40>` tensor has factor nonzero counts `144 / 192 / 312`, versus
`192 / 384 / 384` for the original Smirnov representation.  Before any
estimator implementation, transpose that exact published tensor into the
live `<6,3,3>` orientation, verify every tensor coefficient with integer
arithmetic, and synthesize signed common-subexpression circuits for all three
factors.

## Promotion gate

Promote only if the candidate's weighted whole-row add/sub projection saves
at least `2.8B` operations relative to the live production schedules.  A
smaller result is recorded and killed without a FlopScope row or estimator
edit.

This is target-free algebraic component work.  It opens no MLP, target,
FlopScope session, package, upload, or remote surface.
