# Preregistration: exact axis-hybrid WCB C5-R1

Date: 2026-08-09

## Scope

Offline exact algebra only. No target, MLP, FlopScope, benchmark, physical
row, Mini100 row, package, upload, submission, or remote action is opened.
The result is a **projection**.

The C4 result killed rediscovery of the basis maps as monolithic circuits.
This gate instead uses the proved one-level identities

`U = S_A L`, `V = S_B R`, and `W = I S_C`

to choose a different exact schedule on the two tensor digits. `L`, `R`, and
`I` are the alternative bases and `S_A`, `S_B`, `S_C` the sparse core.

## Frozen schedules

- U: the live 74-addition production map.
- V: the live 53-addition production map.
- W: the exact 132-addition public direct map from `FMMa_6_3_3.m`.
- L/R/I: 74/21/72 additions.
- S_A/S_B/S_C: 44/36/(68 additions + one scaling operation).

All source hashes and commits must close. The exact public W matrix must be
identical to the live W matrix; otherwise the gate fails.

## Fixed depth-two hybrid

The left map uses the live direct recursion:

`C_U = 18*74 + 40*74 = 4,292`.

For the right map, in outer/inner tensor notation, evaluate

`(I_40 x S_B) (V x I_9) (I_9 x R) = V x V`,

costing `9*21 + 9*53 + 40*36 = 2,106` operations instead of 2,142.

For reconstruction, evaluate

`(I_18 x I) (W x I_18) (I_40 x S_C) = W x W`,

costing `40*(68+1) + 18*132 + 18*72 = 6,432` operations instead of 6,594.

The script must validate these identities on deterministic exact integer
vectors as well as close every one-level matrix identity.

## Gates

Reprice the fixed hybrid at 66,096 rows, five width-234 layers and eight
width-216 layers. Pass the first static gate at >=3.6B saving versus current
R90 and the implementation-headroom gate at >=4.5B.

A first-gate-only pass is not authorization to implement: it requires more
static headroom or an unusually cheap fused physical schedule first.

