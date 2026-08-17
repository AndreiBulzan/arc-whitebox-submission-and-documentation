# Preregistration: live analytic layer-31 mean arc teacher R7

Date: 2026-08-09

## Question

Can the analytic closure already executed by the live R90 ancestry supply the
only statistic isolated by R5: the 256-dimensional layer-31 activation mean?

This is a **component** capacity experiment on the same frozen two Full and
two Generated rows used by R2--R6. It performs no official Mini100 prediction,
physical FlopScope session, package, upload, submission, or remote action.

## Frozen construction

Replay the exact float32 `analytic_closure_current` recurrence through the
first 31 supplied layers, including its frozen screening schedule. Retain the
full 256-vector post-ReLU mean immediately before the final layer-31 screen.
Require its selected coordinates and final proxy reconstruction to agree with
the live closure owner.

Let `m_K` and `C_K` be the already sealed K129 layer-31 mean and full
covariance. For the fixed grid

```text
beta = -1, -0.5, -0.25, 0, 0.25, 0.5, 0.75, 1, 1.5, 2, 3
```

form

```text
m_beta = m_K + beta * (m_live - m_K).
```

Use `m_beta` with `C_K` in the same 512-probe Gaussian-ReLU feature map and
the same constrained 129-basis ridge readout as R4. All candidate arrays and
predictions are sealed before benchmark expectation targets are opened.

`beta=1` is the primary target-free construction. The rest are post-seal
capacity diagnostics; no row-wise target selection is authorized.

## Gates

- Promote the live mean directly if `beta=1` gives pooled raw ratio at most
  0.80 and neither family regresses.
- Preserve the mechanism for a frozen scalar-calibration experiment if one
  global beta gives ratio at most 0.70 with both family ratios below 0.85.
- Kill this mean source if every beta has ratio above 0.80 or the useful beta
  changes sign across families.

Passing licenses broader target-free capture only. It is not a production or
Mini100 result.
