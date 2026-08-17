# Large-data K146 basis decoder R1 — verdict

Date: 2026-07-29.

Evidence label: **component development diagnostic** for accuracy and
**projection** for incremental operation count.

## Verdict

**Hard stop.  Do not integrate, physically benchmark, broaden, or tune this
exact basis-decoder spelling.**

R1 captured actual K146 final per-basis state on 96 Full and 32 Generated
MLPs.  It preserved 32 target-free support-imbalanced Walsh characters of
the O0 phase bank, all 17 matched O1-minus-O0-support responses, their signed
pre-ReLU transports through the realized final weight matrix, and a small
arm/weight summary block.  A single shared 1,889-parameter equivariant MLP
was trained on 64 Full MLPs, selected on 16 Full development MLPs, refit on
those 80 MLPs, and sealed before either held family was opened.

The best fixed development checkpoint was already worse than the unchanged
K146 baseline:

```text
selected epoch                    20
Full development pooled ratio     1.047561
later checkpoints                 1.088652 .. 1.279966
```

The separately scored sealed predictions confirmed the kill:

| held bank | pooled corrected ratio | observed row-ratio p95 | rows improved |
|---|---:|---:|---:|
| Full `80..95` | `0.99301799` | `1.39055439` | `43.75%` |
| Generated `0..31`, noise-corrected | `1.00365356` | `1.30837507` | `56.25%` |

The preregistered requirements were pooled ratio `<=0.88` and p95
`<=1.25` independently in both families.  Neither family met the p95 gate,
and the small Full aggregate gain reversed on Generated.

## What this establishes

This is materially stronger than the earlier six-feature DAgger pilot and
the 52-feature aggregate endpoint ridge in one narrow direction: much more
Full supervision does not make the actual fixed phase/basis alias pattern
predict the target-aligned K146 residual under a low-cap shared decoder.
The missing signed innovation is not recoverable here by exposing selected
Walsh coefficients, matched support deltas, and the final realized-weight
response to a small nonlinear head.

It does not prove that every possible complete-cloud or connected-chaos
observable is useless.  Reopening requires a new mathematical state, not
more masks, hidden units, epochs, shrinkage, or split tuning.

## Integrity and economics

- Independent baseline parity against the frozen CUDA adapter was exact:
  RMS `0.0`, maximum absolute difference `0.0`.
- All 96 Full and 32 Generated target-free captures plus training completed
  under `runtime/.benchmark_lane.lock` in `48.583s`.
- Ordinary-CUDA peak allocation was `733,390,336` bytes.
- Full held and Generated predictions were serialized and hashed before
  their targets were opened.
- The lower-K confirmation set was untouched.
- Projected incremental count was `0.25B`, below the `2B` ceiling; no
  physical FlopScope ledger was taken because accuracy failed.
- No physical row, package, upload, submission, network, or remote action
  occurred.

## Authoritative artifacts

- `INVENTORY_AND_PREREG_LARGE_DATA_BASIS_DECODER_R1_20260729.md`
- `run_large_data_basis_decoder_r1_20260729.py`
- `large_data_basis_decoder_r1_seal_20260729.npz`
- `large_data_basis_decoder_r1_seal_20260729.json`
- `score_large_data_basis_decoder_r1_postseal_20260729.py`
- `large_data_basis_decoder_r1_postseal_score_20260729.json`
