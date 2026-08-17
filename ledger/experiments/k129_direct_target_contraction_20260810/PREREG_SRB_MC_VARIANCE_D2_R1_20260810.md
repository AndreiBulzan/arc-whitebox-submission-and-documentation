# Preregistration: exact SRB-MC variance component D2/R1

**Date:** 10 August 2026  
**Scope:** target-free component oracle on the four sealed R1 rows
(`Full[650,651]`, `Generated[96,97]`)  
**Authorized actions:** GPU research capture under the benchmark lock and
offline analysis; no FlopScope physical row, official Mini100 row, package,
upload, submission, or remote action

## Purpose

The D1 direct-contraction replay failed its target-free family gate.  This
experiment isolates the only materially different assertion in SRB-MC:
integrating over Gaussian final-layer siblings with the exact arc-cosine
kernel may reduce the variance of the missing contraction enough that a small
fixed-network residual level is economical.

## Exact observables

For each row, reconstruct the 129 complete Kerdock layer-31 state clouds and
their actual repaired endpoint matrix `Y`.  Let `S = P Y` and
`A = S S^T / 256`.

For a fresh Gaussian input propagated to layer 31, with hidden state `h` and
actual final output `f(h)`, compute

`g_act(h) = S f(h) / 256`.

For each candidate layer-31 state `p`, use the exact Gaussian-row ReLU kernel

`kappa(p,h) = tau2 ||p|| ||h|| /(2 pi)
              * [sqrt(1-rho^2) + (pi-acos(rho)) rho]`,

with `tau2=2/256`.  Average `kappa` over each basis's 512 signed candidate
states, multiply the candidate side by the frozen expected-radius factor, and
center across bases to obtain `g_RB(h)`.

Then `g_res(h)=g_act(h)-g_RB(h)`.  Under the stated final-row model,

`E[g_act] = E[g_RB] + E[g_res]`

for the actual fixed network contraction.

## Sampling design

Use two independent frozen antithetic Gaussian pilot banks.  Each bank has
128 rows and cumulative counts `8,16,32,64,96,128`.  Save the per-pilot
`g_act`, `g_RB`, and `g_res` vectors so all allocation analyses are offline.
All dot products, norms, arc-cosine evaluations, basis reductions, and saved
contractions use float64 after the existing float32 neural propagation.

Associate the reconstructed actual endpoints to the sealed R1
`candidate_actual` within `2e-6`.  Use R1's independent 65,536-row
`dense_actual` only as a target-free component reference; it is not a
benchmark target and cannot be used to alter the pilot design.

## Frozen analysis

On the 128-dimensional nonconstant support of `A`, use `A+` to measure
contraction risk.  For a vector error `e`, define

`R_A(e) = e^T A+ e`.

Estimate per-sample variances `V_act`, `V_RB`, and `V_res` from the two pilot
banks.  Use the proposal-compatible nominal per-sample operation units

- `C_act = 32 * 256^2`;
- `C_RB = 31 * 256^2 + 66,048 * 256`;
- `C_res = C_RB + 256^2`.

The direct and optimally allocated two-level work-normalized risks are

`J_act = C_act V_act`,

`J_SRB = (sqrt(C_RB V_RB) + sqrt(C_res V_res))^2`.

Report `J_SRB/J_act` by row, family, and pooled.  This is optimistic for SRB:
it omits arc-cosine transcendental, reduction, movement, and storage costs.

Also replay the concrete independent telescopes

- `RB(replica0,96) + residual(replica1,16)`;
- the swapped spelling;
- their average,

and compare the resulting endpoint correction against the sealed dense
component reference.  Solve the normal equations with the same fixed relative
ridge grid as D1 and report the best target-free dense-reference capacity as a
diagnostic, not a production selector.

## Gates

Advance to an official-Mini100 capture only if all are true:

1. pooled `J_SRB/J_act <= 0.5`;
2. each family has `J_SRB/J_act < 1`;
3. the two swapped `96+16` telescopes agree in correction sign and both reduce
   dense-reference endpoint MSE by at least 20% on each family; and
4. the projected nominal increment remains below `2.5B` for the intended
   allocation.

If the variance-product gate fails, the exact Rao–Blackwell construction is
killed even if its annealed mean has high target-aware capacity: buying the
coarse term costs more than buying a direct trajectory.

## Evidence boundary

This is a component oracle on four previously selected research rows.  A pass
is permission to test official Mini100, not broad evidence.  A large
variance-product failure is sufficient to reject the mechanism because the
cost estimate is deliberately favorable to it.
