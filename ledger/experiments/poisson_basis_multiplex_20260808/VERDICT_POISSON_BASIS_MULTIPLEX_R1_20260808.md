# Poisson basis-multiplex R1 verdict

Date: 2026-08-08

## Verdict

Kill this sparse/multiplexed implementation of the signed Poisson-band
correction.  The 832-frame teacher remains a real accuracy oracle, but its
signal is not preserved by assigning one independently perturbed radius to
each Kerdock line inside a basis.

This is target-free **component** evidence on fixed Full20 plus Generated20
rows.  No physical row, FlopScope session, Mini100 access, upload, submission,
or remote action occurred.

## Result

The frozen 13-radius teacher weights were multiplexed across all 256 lines of
each basis, with independent tangent directions and a matched zero-radius
control.  Sixteen independent randomizations were captured.  The primary
33-basis held result was:

```text
correlation with complete Poisson teacher     0.191733
teacher MSE ratio                             0.962150
median single-randomization ratio             0.961613
fraction of randomizations at ratio <= 0.80   0 / 16
```

Even all 129 bases retained too much conditional noise:

```text
correlation                                    0.408216
teacher MSE ratio                              0.834464
median single-randomization ratio              0.858799
fraction at ratio <= 0.80                      5 / 16
```

The failure is not lack of teacher headroom: the complete Poisson-band oracle
previously reduced raw MSE by 38.84% on disjoint families.  It is a failure
of sparse conditional sampling to reproduce the complete orthogonal/Latin
cancellations.  Do not turn the all-radii teacher into a claimed low-cost
estimator by ordinary line subsampling, radius multiplexing, or independent
tangent sketches.

## Evidence

- `runtime/artifacts/poisson_basis_multiplex_r1_targetfree_20260808.json`
- `runtime/artifacts/poisson_basis_multiplex_r1_analysis_targetfree_20260808.json`
