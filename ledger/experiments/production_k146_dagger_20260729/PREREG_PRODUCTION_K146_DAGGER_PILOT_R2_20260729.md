# Production K146 rolled-state stabilizer — unit-corrected pilot R2

Date: 2026-07-29

Evidence sought: **component**. No FlopScope run, physical benchmark,
package, upload, submission, or remote action is authorized.

R2 is the exact R1 experiment with one preregistered dimensional correction:
the K146 particle cloud is in unit-sphere angular units, whereas
`official_alm` is in radial activation units. Every checkpoint regression
therefore uses

```text
official_alm[layer] / chi_mean(256)
```

as its target. See `R1_INVALIDATION_UNIT_AUDIT_20260729.md`. R1 selected eta
zero before any held target was opened, so this amendment uses no held
feedback.

Everything else is frozen from R1:

- checkpoints `4,8,12,16,20,24,28,30`;
- the same six arm/basis features;
- positive coordinate scale clipped to `[0.5,1.5]`;
- ridge `1e-4`;
- eta grid `0.25,0.50,0.75,1.00`, trained on each eta's own rollout;
- Full train `0..11`, development `12..15`, held `16..19`;
- Generated held `0..3`;
- separate held-target scoring after prediction seal.

Continue only if Full-held pooled raw ratio and Generated noise-corrected
pooled ratio are both `<=0.88`, both observed row-ratio p95 values are
`<=1.25`, predictions are finite, and projected incremental count is
`<=2B`. Otherwise kill the exact spelling without tuning.
