# Mixed48 multidepth R3 preregistration

Date: 2026-08-04

Evidence scope: **component**.  This is a target-free Full16/Generated16
screen against the sealed four-complete-frame teacher.  It opens no challenge
target and performs no FlopScope, packaging, upload, or submission action.

## Question

R29 takes 48 selected atoms spread across the right, d2, and d2right frames,
propagates them literally to depth 6, and then replaces each atom by a cheap
diagonal-Gaussian continuation.  Complete frames are much more accurate but
too expensive.  Test the literal interpolation between those endpoints:

```text
same 48 atoms -> literal through depth d -> diagonal continuation to output
```

for `d in {6, 8, 10, 12, 16, 20, 24}`.

This exact current-support multidepth path is absent from the capsule.  The
older multidepth scout used independently selected 4--32-atom per-frame
coresets and did not test R29's jointly fitted mixed48 support or coefficients.

## Frozen decisions

- Support, frames, bases, and fixed-coefficient spelling are exactly R29.
- The q0/core64 anchor is unchanged.
- Primary test: reuse R29 coefficients unchanged at every depth.
- Secondary capacity diagnostic: train coefficients on Full16 and score only
  Generated16, then reverse.
- Relative ridges for that diagnostic: `1e-4, 1e-3, 1e-2, 1e-1, 1, 10`.
- Incremental counted price is projected conservatively as `3.234B` per
  extra literal layer; this is not a receipt.

Promote a depth only if its fixed-coefficient teacher-reconstruction error
improves both families and its reconstruction-error times projected-compute
ratio is below `0.98` in both families.  Cross-family refitting may motivate
a separately preregistered successor but cannot rescue a failed fixed path.

