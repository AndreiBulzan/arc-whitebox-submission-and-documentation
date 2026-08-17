# Dual-frame midpoint shared suffix R2 — association correction

Date: 2026-08-05

R1 captured correctly but its scorer named a sealed endpoint artifact whose
Full rows omitted row 7, so the required independent association gate could
not run.  No target was scored and no checkpoint was selected.

R2 changes only the frozen pilot rows to rows already present in the
independent broad q0/polar seal:

- Full: `200, 201, 202, 203`
- Generated: `16, 17, 18, 19`

The complete frames, checkpoints `{16,20,22,24}`, midpoint construction,
economic model, and gates are identical to R1.  Capture remains target-free;
selection happens only after the complete endpoints reproduce the independent
broad seal to `1e-10` maximum absolute error.

