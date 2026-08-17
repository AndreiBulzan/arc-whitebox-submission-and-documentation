# R93/R94 exact-archive ABBA physical gate

Date: 2026-08-09

Run fixed order R93, R94, R94, R93.  Each position executes one initialized
and one steady prediction on the same frozen rows through the exact packaged
worker, under one benchmark lock and one CPU partition.

Frozen public raw ratio: `0.9998578743326509`, taken from the already-sealed
R94 post-seal receipt.  No targets are opened here.

Pass requires zero counted-FLOP delta, exact association with each archive's
Mini100 capture, and every row below 240B effective compute.  Residual and
wall differences are diagnostic only: changing an already executed scalar
must not be promoted or rejected from four noisy timing legs.  The controlling
score statement remains the count-only projection.

This receipt is `measured whole`; it is not broad or remote evidence.  No
upload, submission, or remote action is authorized.
