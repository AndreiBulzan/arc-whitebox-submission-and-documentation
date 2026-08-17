# Production trajectory Green-function R1 — static audit verdict

Date: 2026-07-29

Evidence label: **projection** for timing/economics. This is a source audit,
not component, broad-statistical, measured-whole, or remote evidence.

## Verdict

**Hard stop before execution. Do not run the current capture.**

The source imports, its frozen K146 dependency hashes validate, and all
weight/compact assets needed for a CUDA capture are present. The blocker is
scientific and structural, not missing data:

1. The captured K146 states are real, but their alleged Green function is
   not the response of the deployed K146 path. From layers 24 through 30,
   production recomputes separate, state-dependent 192-coordinate
   `energy_indices` for O0 and O1. The transport function instead applies one
   common analytic-closure right matrix and one common Gaussian CDF gate
   vector to every mode. It receives neither arm's late selection maps.
2. The planned fit is a one-shot linear correction to the baseline signed
   endpoint. It never corrects and replays the candidate state. Therefore it
   does not implement the trajectory-rolled/DAgger principle that motivated
   the experiment.
3. This mechanism is substantially the already-closed
   response-projected-endpoint class. The actual rolled K146 stabilizer R2
   was the stronger test: it used the same eight checkpoints and real
   per-arm/per-basis states, yet reversed on both held families
   (`1.057951x` Full, `1.112804x` Generated). The earlier one-shot
   response-projected lane was also essentially neutral on Full and reversed
   on Generated (`0.99270x`, `1.00985x`).
4. The directory contains capture code only—no fitting/sealing stage and no
   post-seal scorer—so the preregistered experiment is not end-to-end
   runnable.
5. The script requests 140 Full plus 64 Generated rows: 204 total, not the
   144-row fast falsifier recommended by the public-method audit. It also
   does not acquire `runtime/.benchmark_lane.lock`.

The exact nine-mode spelling does add O0 Walsh bits 2–3 and four O1 DCT
modes. That is new feature content, but not a new causal mechanism. The arm
mean contrast is already in the span of two DAgger R2 features, and Walsh
bits 0–1 overlap exactly. Six added basis harmonics do not justify a
204-row acquisition through a downstream operator that is not the
production operator.

## Economics before the stop

The prior target-free K146 capture measured a diagnostic mean of
`0.203842s` per row over 164 rows. Merely replaying 204 rows therefore has a
`41.584s` baseline projection. R1 adds 106 small transported matrix stages
per row, at most `62,521,344` float64 multiply-adds, plus eight full-cloud
basis reductions. A reasonable order estimate is `50–100s` for capture,
before fitting and scoring. No such run was made.

The serialized baseline/features would be about `30.5 MB` uncompressed.
The claimed deployment increment below `2B` remains unproved because the
actual production-path response was never specified or priced.

## What would constitute a legitimate re-open

A replacement must:

- preserve O0 and O1 modes separately;
- record and use each arm's actual late energy-selection maps;
- propagate through the fixed-selection production graph with explicitly
  defined arm-specific gates;
- either roll every correction through the candidate path or describe the
  method honestly as an endpoint head;
- provide separate capture, fit-and-seal, and post-seal scoring stages;
- use the shared benchmark lock; and
- statically price the exact deployable response graph.

Only then should a reduced, already-used-row kill test be considered. The
current source should not receive even a one-row CUDA smoke because import
and asset viability are already established and a numerical smoke cannot
repair the operator mismatch.

## Execution boundary

This audit imported the source and validated frozen dependency hashes. It
opened no targets and ran zero GPU rows, FlopScope sessions, physical rows,
packages, uploads, submissions, or remote actions.

Authoritative machine-readable receipt:
`trajectory_green_r1_static_audit_20260729.json`.
