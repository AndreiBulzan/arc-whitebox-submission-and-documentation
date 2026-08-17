# K129 R31 remaining-view owner map — verdict

Date: 2026-08-03

Evidence label: **component** for the target-free owner census; **projection**
for the deliberately generous deletion ceiling. The diagnostic instrumentation
is heavy, so it provides no timing receipt.

## Result

Three consecutive R31 rows produced identical steady ledgers. A steady row had
3,777 `__getitem__` calls over 713 distinct parent handles:

- 2,159 calls used 95 parents that persisted across rows;
- 1,618 calls used 618 parents recreated on each row.

The large persistent-view opportunity anticipated in the older handoff has
already been implemented by the K129 ancestry (Morton rights, signed fold
cells, fixed boundary sources, and natural decoder cells). The remaining fresh
parents are fragmented: most creation roles expose only one to eight views per
parent. The largest roles are late-B7 temporaries (112 calls), FWHT temporaries
(88), natural-output chunks (76 and 72), cell-wave output chunks (70), and the
M267 selected-right result (64).

Even the impossible best case—deleting all 1,618 remaining calls at the slower
3.437 microsecond/view calibration—saves only 0.005561 seconds, or about
0.556B effective compute. That is roughly 0.4% of R31's diagnostic effective
compute, before paying any replacement requests or bookkeeping.

## Decision

Do not build another generic view cache; it was already measured net-negative.
Do not implement isolated 30--112-call view hoists. Revisit views only through
an aggregate rewrite that removes at least about 1,000 calls without taxing
the calls that remain. The next material path is the capsule-native mixed64 D6
implementation on R31, not further view housekeeping.

Artifact:

- `runtime/artifacts/k129_r31_remaining_view_owner_map_component_r1_20260803.json`
- SHA-256 `76840e32801d62b136dc081bcdb9468e9c2ffbb9eff5afb91155b8998c26b10d`
