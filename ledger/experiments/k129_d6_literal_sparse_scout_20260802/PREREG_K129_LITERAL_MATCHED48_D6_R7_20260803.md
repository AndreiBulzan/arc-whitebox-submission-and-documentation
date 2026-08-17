# K129 literal matched-48 depth-6 scout R7

Evidence is **component** until the frozen literal prediction is scored.
All compute and adjusted-score statements here are **projections**.

## Prior-art preflight

Queries covered `direct sparse d6`, `matched`, `size 48`, `literal sparse`,
`independent repair`, `paid anchor`, `qmean`, `diagonal continuation`, and
the production H1/H2/L4 call sites.

The R6 target-free census already contains a complete-frame `matched`,
`size=48`, `ridge=1` proxy candidate.  It was not selected, never rerun as
an independently repaired 48-slot arm, and never received a literal broad
score.  R13's paid-anchor class is not reopened: this candidate retains the
existing matched q0 per-basis diagonal proxy rather than substituting the
scored q0 endpoint.  The R1--R6 literal64 result newly establishes that
independent sparse repair preserves the depth-6 correction closely.

Outcome: **materially new observable**.  This is the first literal matched48
arm and closes both the sparse-repair and q0-reference execution costs.

## Frozen candidate

- Support: the `size=48` OMP path using only the broad target-free complete
  four-frame teacher and full depth-6 proxies.
- Feature: alternate-frame depth-6 diagonal proxy minus the matched q0-basis
  diagonal proxy.
- Ridge: exactly `1.0`.
- Directions: exactly `256`.
- Literal graph: all 48 mixed frame/basis slots undergo one independent
  compact repair, H2 repair, exact layers 2--5 including L4 snap, and then
  diagonal continuation.  Only the distinct matched q0 basis proxies are
  continued alongside them.
- No candidate size, feature kind, ridge, coefficient, or direction count
  may change after this file.

## Target ceiling

Literal64 projects to `1.13284e-7` on the weaker Generated128 family.  Moving
from 64 to 48 exact prefix slots removes about `3.46B` effective operations.
Matched q0 continuation uses at most 48 rather than all 129 q0 basis states,
removing roughly another `0.5B--0.7B`.  The audited exact contraction remains
worth `1.1949B` if subsequently integrated.  At unchanged raw MSE the
combined projection is approximately `1.09--1.10e-7`.

Promote only if the literal candidate improves both Full100 and Generated128
and its weaker-family central adjusted projection is at most `1.11e-7`
before the separate exact contraction.  No package, physical row, upload, or
submission is authorized here.

