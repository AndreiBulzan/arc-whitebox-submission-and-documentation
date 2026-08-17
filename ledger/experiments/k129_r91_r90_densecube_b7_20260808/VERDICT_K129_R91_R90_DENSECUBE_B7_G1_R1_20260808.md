# Verdict: reject R91; freeze R90

Date: 2026-08-08

## Outcome

Reject the vectorized fourth-B7 rewrite for the five dense `256^3` products.
The exact algebraic plan lowers counted arithmetic, but its public-array
transport overhead is larger than the saving.  Do not run Mini100 and do not
compose this plan into R90.

No targets were opened.  No upload, remote action, or submission occurred.

## Gate

The deterministic package passed the exact initialized-plus-steady lifecycle
gate.  It hit exactly the five census-proven products and produced:

- initialized / steady FLOPs: `125,810,136,664` / `123,366,422,399`
- steady counted saving versus R90: `25,429,760`
- steady prediction SHA-256: `c1432e080ade5f80f56bdec501d5c3d0f29ac41483c8d72b7035f8b7b52eaf17`

This established that the vectorized B7 implementation was functional and
count-positive, but did not establish an effective-compute win.

## Controlling ABBA

The quiet R90/R91/R91/R90 exact-package comparison measured:

- counted FLOPs: `-25,429,760`
- residual wall time: `+5.891 ms`
- effective compute: `+563,677,490`
- transport requests: `+918`
- indexed reads: `+380`

This is **measured whole** evidence.  The effective regression is more than
twenty times the counted saving, so no plausible accuracy association benefit
could justify continuing a nominally prediction-preserving engineering patch.

The failure is narrower than “B7 does not work.”  R90's larger repaired-H1
leaf rewrite remains profitable.  This result says that five isolated dense
cubes are too small to amortize four levels of RemoteArray bank construction.

Evidence:

- `runtime/artifacts/k129_r91_r90_densecube_b7_gate_g1_r1_20260808.json`
- `runtime/artifacts/k129_r90_r91_exact_archive_abba_g1_r1_20260808.json`

