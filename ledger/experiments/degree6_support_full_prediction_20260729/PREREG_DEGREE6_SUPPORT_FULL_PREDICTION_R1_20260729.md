# Degree-6 support full-prediction R1 preregistration

Date: 2026-07-29

Evidence sought: **broad statistical** after a target-free prediction seal
and a separate scorer. This authorizes only an offline CUDA replay and the
already-used Full100/Generated64 development score. It authorizes no
FlopScope session, physical row, package, upload, submission, or remote
action.

## Frozen candidate

Use the already target-free and hash-sealed degree-6 `m=17` support:

```text
[3, 11, 15, 17, 27, 34, 36, 45, 54, 64, 66, 70, 78, 82, 111, 115, 120]
```

Replace only the orientation-1 support identities in the actual
production-aligned K146/m17 GPU replay. Keep all propagation, repairs,
compression widths, late energy selections, final readout, literal
`129:17` arm blend, and numerical dtypes/association unchanged.

Capture Full100 and Generated64 predictions with weights and seeds only,
then hash-seal the arrays before opening targets. Compare with the current
sealed K146 predictions on the identical rows.

## Hard gate

Promote only if the literal candidate independently satisfies in both named
families:

```text
pooled final raw MSE / current K146 raw MSE <= 0.94
row-MSE ratio p95                         <= 1.50
all captured and scored arrays finite
```

Generated64 uses observed target MSE for this exact ratio gate. Also report
its conventional frozen label-noise-corrected central MSE, but do not use
that correction to change candidate selection.

## Fixed post-hoc diagnostic

After the candidate is sealed, separately report the preregistered fixed
orientation-1 coefficient `alpha=0.10`:

```text
prediction = 0.90 * q0 + 0.10 * q1
```

This is distinct from the literal-support candidate and cannot rescue a
failure of the literal hard gate. No coefficient fitting or retry is
permitted.

## One frozen interaction candidate

The already-sealed endpoint scout placed the plain support at ratios
`0.94697` Full100 and `0.94998` Generated64, within three percentage points
of the final-output `0.94` gate. Therefore, before target access, capture
exactly one additional interaction candidate:

- retain the same fixed degree-6 support;
- use the canonical descending eigenframe of `W0.T @ W0` at O1 phase
  positions `0,4,8,12`;
- retain the ordinary control rotation at all other O1 phase positions;
- apply this choice at both structured-block calls, as in the already-frozen
  mixed-SVD spelling.

There is no phase-position, support, coefficient, or family grid. Score this
interaction separately under the same literal hard gate. Its projected
runtime count increment is `0.251985920B`; it does not alter the plain
candidate economics.
