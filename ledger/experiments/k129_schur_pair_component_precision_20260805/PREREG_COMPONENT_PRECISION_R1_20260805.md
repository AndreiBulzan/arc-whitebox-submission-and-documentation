# K129 Schur-pair component-precision pilot R1

Date: 2026-08-05

## Question

R82 converted the entire R83 frame constructor to float32 and reversed on all
three pilot families.  That result does not identify which of the four
numerically distinct constructor islands caused the reversal.  Test each
island independently while every other operation remains float64:

1. right-Gram construction plus its canonical eigensolver;
2. depth-2 Jacobian path plus its canonical eigensolver;
3. polar-factor Gram/eigh/reconstruction;
4. final symmetric-relative eigensolver, selector, and hybrid reconstruction.

The propagated K129 statistic and output scale are otherwise unchanged.

## Prior-art preflight

Queries covered `float32`, `mixed precision`, `eigh`, `eigen`, `polar`,
`Schur`, `right Gram`, `Jacobian d2`, and the production constructor call
sites.  Nearest work:

- `k129_schur_pair_float32_r82_20260805/`: whole-constructor float32;
- `k161_rightgram_d2right32_breakthrough_20260731/`: extra float32 propagated
  trajectories, not precision reduction inside the R83 constructor;
- R83: the all-float64 physical constructor.

Outcome: **materially new isolation of an existing negative**.  No capsule
artifact tests one constructor island at a time.

## Ceiling

R83 adds about 1.958B counted FLOPs over R78.  Its four float64 256x256
eigensolvers alone bill 1.208B.  Converting every safe constructor island can
therefore save no more than roughly 1.0B, about 0.7% of R84 effective compute.
An individual island is only a 0.1--0.3% candidate.  This pilot is justified
only as a short screen; it cannot by itself close the sub-1e-7 objective.

## Sealed pilot

- Full: the first 12 rows in the already sealed Schur-pair Full100 capture.
- Generated: the first 12 rows in the corresponding Generated128 capture.
- Official Mini100: rows 0..11.
- Controls: q0, the already sealed exact gamma=0.50 statistical prediction,
  and a physical-style all-float64 NumPy reimplementation of R83.
- Candidates: exactly the four one-island float32 variants listed above.

The capture is target-free.  A variant may advance to a broad all-population
test only if, on every family, its mean raw MSE is at most 1.005x the physical
float64 control; its pooled raw ratio is at most 1.001; and its pooled output
RMSE from the physical control is at most 3e-5.  These are screening gates,
not deployment evidence.  No coefficient or candidate is selected using the
targets.

Every promoted spelling still requires Full100, Generated128, all official
Mini100, exact FlopScope count, and archive association before banking.

