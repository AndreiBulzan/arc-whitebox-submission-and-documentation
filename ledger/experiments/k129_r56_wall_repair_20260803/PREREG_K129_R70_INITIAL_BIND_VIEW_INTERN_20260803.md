# K129 R70 initialized-bind view interning preregistration — 2026-08-03

Evidence status: **projection only**. Target-free; no remote action authorized.

## Prior-art exclusion

The earlier handle-keyed memoization taxed every one of roughly 11k steady
view call sites and was measured net-negative. R68 already performs exact
descriptor de-duplication *within* each bind. R70 tests neither spelling.
The R69 census isolates 1,169 additional `R23._materialise_view` calls that
exist only during deferred worker initialization. R70 temporarily interns
identical `(live bank object, affine positions)` views *across initialization
binds*, then removes the lookup wrapper before steady prediction.

## Invariants

- Parent archive: R69 r1,
  `557421c4867618e52a4d51440acb7906b20c163c2e938fa569f1caad1978a5f9`.
- Arithmetic, association, plan call order, destinations, output bytes, and
  steady execution are unchanged.
- Cache entries retain both the live bank object and its view, preventing
  object-id reuse and stale-parent ownership.

## One-shot gates and ceiling

1. Exact official two-prediction gate: R69 digests and FLOP counts must match.
2. Steady requests must remain 33,502. Initialized requests must improve on
   R69's 41,649 by at least 500; otherwise kill this candidate.
3. If gate (2) passes, one five-lane screen only. Promotion requires local
   initialized mean <=51.5 s and calibrated remote tail <60 s.

No target scoring, upload, submission, or remote execution is authorized.
