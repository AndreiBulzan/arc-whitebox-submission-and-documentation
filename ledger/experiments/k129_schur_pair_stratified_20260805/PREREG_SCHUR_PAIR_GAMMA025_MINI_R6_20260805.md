# Schur pair gamma=0.25 Mini100 transfer R6

Date: 2026-08-05

Before this Mini100 capture, `pair_gamma_0.25` is fixed because it strictly
dominates the earlier `pair_energy_high` (`gamma=0`) rule on both broad
families: Full100 0.928786 versus 0.931247 and Generated128 0.956392 versus
0.962494.  It is the closest tested interpolation point to the energy rule,
which independently scored 0.914523 on Mini100.

No parameter may change after capture.  Promotion requires Mini100 raw ratio
<=0.94, at least 50/100 rows improved, and projected adjusted score <=1.10e-7
with a 2.5B constructor allowance.

