# Asymmetric R40 exact-gauge search — R1 verdict

Evidence label: **component** (offline exact algebra; no MLP, FlopScope,
package, upload, or submission action).

The live rank-40 tensor was reconstructed from the frozen production
schedule and verified coefficient-for-coefficient.  The frozen schedule has
`A/B/C = 108/54/132` addition/subtraction nodes.

`search_asymmetric_r40_r1.py --trials 4 --top 8` screened every elementary
unimodular change of the outer, contracted, and output coordinates, the
limited ternary `GL(3)` frontiers, and their small joint frontier.  The
identity gauge was the winner under both priorities used by the search:

1. synthesized activation-side cost (`A + C`), then
2. total synthesized cost (`A + B + C`).

Every non-identity candidate that reached exact circuit synthesis increased
one or both objectives.  Therefore this bounded exact-gauge class provides
no lawful asymmetric R40 contraction saving.  Do not promote it to a whole
candidate or spend physical-row time on it.

This closes only the searched gauge orbit; it does not prove that a genuinely
different bilinear decomposition of the same tensor cannot improve the live
schedule.
