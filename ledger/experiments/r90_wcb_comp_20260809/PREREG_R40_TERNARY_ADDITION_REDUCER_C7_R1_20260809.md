# Preregistration: live R40 ternary addition reduction C7-R1

Date: 2026-08-09

## Scope

This is an offline, target-free, exact factor-circuit search. Evidence is a
**projection**. It opens no target, MLP, FlopScope session, benchmark,
physical row, Mini100 row, package, upload, submission, or remote action.

## Inputs and tool

- Live 40x18 U, 40x9 V and 18x40 W factor matrices reconstructed from the
  hash-pinned production R40 schedule.
- `dronperminov/ternary_addition_reducer` at commit
  `e59693512f095a4704521f1185c592445af9e058`.
- A one-line private audit patch changes only the input tensor validation
  target from one to eight. The live accurate factors have their uniform
  A/4 and W/2 denominators stripped and therefore represent eight times the
  multiplication tensor; production compensates this globally. The reducer
  itself only rewrites each fixed linear factor and is unmodified.

The generated input, patched source and executable hashes must be recorded.
Every returned reduced factor must expand exactly, coefficient for
coefficient, to the original live U/V/W matrices. Tensor equivalence alone
is insufficient.

## Frozen search

- reducers: 64;
- threads: 8;
- seed: 2026080907;
- maximum no-improvement iterations: 10;
- default strategy mixture and partial-initialization rate.

If this first search is near a gate, independent frozen seeds may be
preregistered later; they are not silently folded into this receipt.

## Corrected B7-fixed pricing

Let one-level counts be `u`, `v`, and `w`. Price the best exact depth-two
spelling independently:

- U: `min(4292, 58u)`;
- V: `min(2597, 2142, 49v, 1629+9v)`;
- W: `min(8236, 6594, 58w, 4056+18w)`.

The rank-7 B7 transforms and all leaf products remain fixed. U/W nodes weigh
591,192 operations and V nodes 7,988 operations over live R90.

Pass the first static gate at >=3.6B saving and the implementation-headroom
gate at >=4.5B. No implementation is authorized by a projection alone.

