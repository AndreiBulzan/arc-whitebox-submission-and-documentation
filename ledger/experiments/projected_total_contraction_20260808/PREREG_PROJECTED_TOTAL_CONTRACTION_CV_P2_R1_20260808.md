# Projected total contraction CV P2/R1 — local-GP residual audit

Date: 2026-08-08

Evidence class: **component** on fixed Full640--641 and Generated88--89.
This stage is target-free and is not a physical, broad-statistical,
projected-score, or remote receipt.

## Motivation

P1 showed that random 16--32-basis source spans retain nearly all clean
correction-source energy, but an unbiased audit of raw complement sources is
far too noisy.  Independently, the sealed local antipodal packet GP predicts
the individual complete-basis source field with `0.886--0.916` R2, while its
direct total fails because a small common-mode residual survives a much
larger cancellation.

The direct failure does not prevent using that prediction as a finite-
population control variate.  Let `s_b` be the exact complete-basis source and
`p_b` its target-free local-GP prediction.  For candidate support `A`,
complement size `M`, and `q` audits, define

```text
t_tilde_cv = sum_b p_b
             + sum_{b in A} (s_b - p_b)
             + (M/q) sum_{b in audit} (s_b - p_b).
```

Conditional on the predicted field, this is exactly unbiased for
`t=sum_b s_b`.  The projected estimator is `P_A t_tilde_cv`.  Its clean bias
is unchanged from P1, while its audit variance depends only on the residual
population `s_b-p_b`.

## Frozen inputs

- P1 oracle:
  `runtime/artifacts/projected_total_contraction_p1_r1_targetfree_20260808.npz`
  (`sha256 e032d47bc679c56efa13cfa8edf97b4180bd6068651d10c940561808eae7cd10`);
- P1 report:
  `runtime/artifacts/projected_total_contraction_p1_r1_targetfree_20260808.json`
  (`sha256 4ff84f0b8a76d550a2114299161a6cdb03b083101b5da51c9a065855f630409a`);
- local-GP source-fidelity arrays:
  `runtime/artifacts/local_antipodal_packet_gp_fidelity_l1_r1_targetfree_20260808.npz`
  (`sha256 67603c82fd5d738f4e7c94d91375ea4bcbba3f353f8090401b4ce8a0e68cd01b`);
- local-GP report:
  `runtime/artifacts/local_antipodal_packet_gp_fidelity_l1_r1_targetfree_20260808.json`
  (`sha256 7badd96e7d535a386f05ed72049a0045a9369c39ee10a6d226b12ffd0110fe54`).

No expectation target, Mini100 row, physical benchmark, package, upload, or
remote lane may be opened.

## Frozen variants and calculations

Test the three target-free local-GP variants `theory`, `global_energy`, and
the preregistered primary `basis_energy`.  Exclude `zero_variance`, which was
teacher-only in the original local-GP program.

Reuse P1's exact random supports, exact source spans, SVD cutoff, audit counts,
and `8.68e-9` allowance.  For each support:

1. reconstruct `P_A` from the exact 32-replicate source columns;
2. form complement residuals `s_b-p_b`;
3. compute their exact finite-population covariance after projection;
4. report control-variate audit variance, its ratio to P1 raw-source audit
   variance, total contraction MSE, and the exact integer audit count required
   to reach the allowance;
5. summarize by family and pooled with percentiles and pass fractions.

## Gates

- A score-compatible lead requires `q<=8` to reach or approach `8.68e-9` on
  both families for a material fraction of target-blind supports.
- `q=12` or `q=16` is retained only as a conditional research lead because
  existing complete-basis cost evidence makes it unlikely to repay compute.
- If even the best lawful variant still needs dozens of exact audits, close
  this complete-basis contraction spelling.  Do not run noisy-treatment or
  target-aware score replays merely because the clean source span is strong.

This remains optimistic: exact 32-replicate source columns define both the
candidate span and audited residual.  Any pass must next survive a separately
sealed individual-treatment replay.
