# Preregistration: nested polar midpoint R1

Date: 2026-08-05

## Construction

Let `Q0` be the standard complete K129 frame and

```text
P = polar((Q0 + Qright + Qd2) / 3).
```

The broadly confirmed two-arm statistic is the output-space midpoint of the
complete `Q0` and `P` cubatures.  R1 replaces those two propagated arms by
the orthogonal frame midpoint

```text
M = polar((Q0 + P) / 2)
```

and propagates one complete K129 design in frame `M`.  This is distinct from
the killed anchored polar rule `polar(a Q0 + b Qright + b Qd2)`: the inner
polar is completed before the outer midpoint.  It is also distinct from the
killed hidden-cloud midpoint, which averaged activations after nonlinear
propagation.

## Frozen protocol

- exactly one candidate, with both midpoint weights fixed at `1/2`;
- all 100 official Mini100 predictions are written before targets are read;
- q0 is the existing sealed control;
- no coefficient, row, frame, or support selection is allowed.

Promote to current-meter pricing only if Mini100 raw ratio to q0 is at most
`0.92`, at least `70/100` rows improve, and the paired-bootstrap 95% upper
ratio is below `1.0`.  A statistical pass is not a compute claim: the full
frame construction must then keep the measured-whole effective delta within
the remote `1.10e-7` allowance.

