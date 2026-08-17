# R97 R92 K-wide straight-line codegen verdict

**Date:** 2026-08-10  
**Decision:** kill R97; retain R92 unchanged  
**Evidence:** measured whole (two fixed isolated ABBA blocks), plus projection

R97 replaced the live loop over 6,351 already-bound K-wide calls with generated
straight-line functions.  It did not alter any FlopScope operation, operand,
destination, execution order, association, or prediction.

The exact-package receipt confirms:

- every R97 prediction hash equals its paired R92 prediction hash;
- every counted-FLOP total equals R92 (`123,408,987,951` on the measured row);
- the two blockwise R92-minus-R97 residual savings were `+11.007 ms` and
  `-6.989 ms`;
- the pooled median saving was `+4.997 ms`.

The direction therefore reversed between blocks.  R97 fails the preregistered
requirement that the candidate beat R92 in both blocks.  Its pooled apparent
saving is also below the roughly `13.9 ms` needed for a one-percent score lever
and far below the roughly `42.9 ms` needed to move the observed R92 aggregate to
`1.10e-7` at unchanged raw error.

The projection obtained by applying the noisy pooled delta to the observed R92
remote aggregate is `1.1307718222e-7`; it is not a receipt and must not be used
as an improvement claim.

Authoritative artifacts:

- preregistration: `PREREG_K129_R97_R92_KWIDE_CODEGEN_R1_20260810.md`
- source: `candidate_k129_r97_r92_kwide_codegen_v0100_20260810.py`
- package: `runtime/artifacts/k129_r97_r92_kwide_codegen_local_candidate_r1_20260810.tar.gz`
- measured receipt: `runtime/artifacts/k129_r92_r97_kwide_codegen_abba_r1_20260810.json`
- receipt SHA-256: `726848942a3829752ceb04d3b3288092acb57c6073bb27cc74229a30541f3a34`

No targets were opened and no upload or remote action was taken.
