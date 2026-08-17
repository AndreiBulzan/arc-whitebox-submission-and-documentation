# Verdict: kill the in-place quadrant-bank variant

Date: 2026-08-03

Evidence label: **measured whole** diagnostic; no broad or remote evidence.

The corrected persistent-view version is exact and prediction-bit-identical,
and its steady count hits the preregistered `123,966,341,434`.  It removes
R31's stacks but adds seven net requests because exposing the spatial
quadrants requires a 2x2 view bank and three unstack calls.

```text
                              R31                 R32
steady count                 124.239224002B      123.966341434B
steady residual                0.136870367s        0.143486960s
steady effective             137.926260702B      138.315037434B
steady requests               31,041               31,048
```

The `0.272882568B` counted saving is overrun by `0.6616593B` of additional
residual charge in this diagnostic, for a net `0.388776732B` loss.  Decision:
kill unchanged and keep R31.  The failed first archive used the raw spatial
workspace as if it had a quadrant-leading axis; it was rejected locally
before a prediction and has no evidentiary value.

