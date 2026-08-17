# K129 R69 cold-view unstack preregistration — 2026-08-03

Evidence status at registration: **projection only**. No target values are
opened by this experiment and no remote action is authorized.

## Prior-art check

The capsule already contains the persistent natural-decoder and fixed-boundary
source bindings in
`remaining_view_hoist_successor_20260729/candidate_k162_m33_remaining_view_hoist_r2_v091_20260729.py`.
Those bindings are active in R27/R56 and are not to be rebuilt or moved back to
steady execution. The untested narrow target is only their deferred first-row
construction spelling: replace repeated scalar `RemoteArray.__getitem__`
creation with hierarchical zero-count `flopscope.numpy.unstack` views.

## Frozen parent and exact hypothesis

- Parent archive: R68 r3,
  `9c4657c8ba8b939eba797aebd652f2adbc7fe81b821b8f73a34d290ed09d2fad`.
- Preserve every arithmetic call, source order, destination, floating-point
  association, and returned bytes.
- Replace exactly these initialized-row view families:
  - 648 natural-decoder cell getitems with 40 hierarchical unstack calls;
  - 720 boundary left-source getitems with 146 hierarchical unstack calls;
  - 720 boundary output-source getitems with 52 hierarchical unstack calls.
- Net target: remove 1,850 initialized-row transport requests. Steady-row
  arithmetic and transport must remain unchanged.

## Fast kill and acceptance gates

1. One exact-archive official LocalRunner row must preserve R68's initialized
   and steady prediction digest and counts (`140690324797`, `138444109252`).
2. Initialized requests must fall from 43,499 to at most 41,700; steady
   requests must remain 33,502.
3. Only if (1)-(2) pass, run the five-lane initialized/steady screen. Its
   initialized mean must be at most 51.9 seconds locally (the R17-calibrated
   boundary for a 60-second remote tail). A promotion candidate should beat
   51.5 seconds to retain nonzero calibration margin.
4. One failed exactness gate or one five-lane miss ends this spelling; do not
   tune it repeatedly.

Package, upload, submission, target scoring, and remote execution remain
unauthorized by this preregistration.
