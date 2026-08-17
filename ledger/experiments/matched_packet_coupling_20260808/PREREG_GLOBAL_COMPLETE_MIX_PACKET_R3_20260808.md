# Preregistration: global complete-mix packet coupling R3

Date frozen: 2026-08-08

R1 and R2 show that response matching from 4--8 held common treatments does
not generalize to fresh packet noises.  R3 removes matching entirely and asks
whether the packet response contains a common linear component that can be
cancelled by an exact joint Gaussian mix.

For a fixed group of `k` lines, draw iid `A_i ~ N(0,I)` and set

```text
Z_i = sqrt(k/(k-1)) * (A_i - mean(A)).
```

Every `Z_i` is exactly `N(0,I)` and their group sum is exactly zero.  Hence
the packet target is unchanged at every line.  The arms use exactly one
antipodal candidate pair per line and differ only in group size:

```text
independent
mix2_random       fixed random line pairs
mix8_basis        consecutive 8-line groups within each Kerdock basis
mix256_basis      one complete Kerdock basis per group
mix_global        all 33,024 lines in one joint Gaussian mix
```

The same four reused packet-oracle networks and eight independent evaluation
replicates are used.  Capture is target-free and sealed before scoring.

Pass requires at least one complete-mix arm to retain 70% of ideal packet
gain pooled, 50% in each family, improve at least three of four rows, and have
lower target-free packet sampling MSE than independence.

No physical run, FlopScope session, Mini100 access, package, upload,
submission, or remote action is authorized.
