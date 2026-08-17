# Harmonic alias decoder R1 — hard kill

Date: 2026-07-29

Verdict: **kill the fixed paid-basis alias decoder**.

Evidence label: **component**. The target-free phase used only the existing
complete eight-orientation signed-endpoint atlases. It sealed predictions
before a separate scorer opened exact signed-preactivation targets. No GPU,
FlopScope, physical row, package, upload, submission, or remote action ran.

## Reciprocal result

R1 fit the most general fixed linear, constant-exact weighting of the actual
146 paid basis endpoints: all 129 O0 bases plus frozen O1 S17. Coefficients
were trained only to reconstruct the target-free complete all-eight
population, with ridge selected inside the source family.

| train -> held | signed MSE ratio | row-ratio p95 | improved rows |
|---|---:|---:|---:|
| Generated64 -> Full100 | `0.999701` | `1.00717` | `46.0%` |
| Full100 -> Generated64 | `1.000100` | `1.00478` | `54.7%` |

The hard gate was ratio `<=0.80` and p95 `<=1.25` in both directions.
Both fits selected the strongest ridge (`100`) and collapsed almost exactly
to uniform weights. The effective-weight ranges were only
`0.006759..0.006929` and `0.006770..0.006935`, around uniform
`1/146 = 0.006849`.

This is stronger than a negative result for one hand-picked reweighting:
every MLP-independent affine weighting of the paid K146 signed endpoints is
represented by this constant-exact decoder. The omitted degree-6-and-higher
alias is not transferable through fixed basis-ID weights.

Runtime arithmetic would have been below `0.001B` by projection, but there
is no accuracy case for implementation.

Authoritative receipts:

- `harmonic_alias_decoder_r1_targetfree_20260729.json`
- `harmonic_alias_decoder_r1_postseal_20260729.json`
