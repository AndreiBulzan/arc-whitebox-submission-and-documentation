# K129 R71 rank-axis view-family batching preregistration — 2026-08-03

Evidence status: **projection only**. Target-free; no remote action authorized.

## Prior-art check

- R11 previously replaced one D-carrier family by transpose/unstack and
  projected 170 fewer setup calls, but that transformed vendor source is not
  present in the current R69 archive.
- R69's exact census still attributes 1,114 initialized-only getitems to
  `candidate_rank_axis_cdww_square200_v091.py`.
- Generic view memoization was measured net-negative and is not repeated.

R71 exposes only the observed, finite rank-axis basic-index families. On the
first member of a family it performs any shared basic slice once, then
hierarchically unstacks the small integer axes and serves the original
constructor's subsequent indexing in Python. The original constructor and
all arithmetic remain literal.

## Frozen parent and invariants

- Parent: R69 r1 archive
  `557421c4867618e52a4d51440acb7906b20c163c2e938fa569f1caad1978a5f9`.
- No arithmetic, association, call order, destination, or output change.
- The hook is active only inside `RankAxisCDWWSquare200.__init__`, is restored
  in `finally`, and all hierarchy parents are retained by the owner.

## Fast ceiling

1. Exact archive gate must preserve R69's two digests and exact counts.
2. Steady requests must remain 33,502. Initialized requests must improve on
   41,649 by at least 500 or the candidate is killed.
3. If it passes, run one five-lane screen. Promotion requires initialized
   mean <=51.5 s locally and calibrated remote tail <60 s.

No target scoring, upload, submission, or remote execution is authorized.
