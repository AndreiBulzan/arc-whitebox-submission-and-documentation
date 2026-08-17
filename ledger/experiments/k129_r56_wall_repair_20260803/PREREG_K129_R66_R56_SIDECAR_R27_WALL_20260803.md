# K129 R66 — R56 statistic on the R27 wall chassis

Date: 2026-08-03

Initial evidence label: **projection**.  A surviving exact-archive run becomes
**measured whole**; the inherited R56 family accuracies remain **broad
statistical** only if numerical association is established.

## Question

Can the fixed mixed48/D6 R56 correction run on the older R27 base executor,
removing the R28/R30/R31 late-contraction stack that failed the remote
60-second tail, while keeping the correction statistic and challenge limits?

## Blocking prior-art search

Queries covered `R56 R27`, `sidecar R27`, `mixed48 R27`, `rebase`,
`transplant`, `drop R28`, `skip R28`, `DPS48 only`, and the exact R27/R34/R56
setup and `_predict_m208` call sites.  The nearest work is:

- R27: remotely anchored R26 descendant, `26.154990s` steady local wall;
- R28: exact late B7 rewrite, `32.348045s` steady local wall;
- R30/R31: lower-count descendants, `28.856990s` / `28.750284s` steady local
  wall;
- R34--R56: the mixed correction and its exact CSEs, always executed on R31;
- R32/R33 and view caching: controlled negative effective-compute results.

No capsule artifact composes the fixed R56 sidecar with a direct R27 base.
Outcome: **materially new observable** — wall-first composition after remote
R56 timeouts, not a new estimator statistic.

## Target ceiling

R56's steady counted work is `136.277381572B`; R31 contributes
`124.239224002B`, and R27 contributes `126.405951682B`.  If the sidecar cost
is unchanged, the R27 composition should count:

```text
136.277381572 + (126.405951682 - 124.239224002)
= 138.444109252B
```

At the completed-row remote R56 price ratio, this raises the frozen adjusted
projections by about `1.59%`, to roughly `1.106e-7` Full100 and `1.193e-7`
Generated128.  This is sufficient for the immediate `<=1.2e-7` checkpoint
only if numerical association holds.  The attempt is therefore worth one
focused physical row.

## Gates

1. production bare setup remains request-free;
2. initialized and steady ledgers are deterministic and below `272B`;
3. output is finite `(32,256)` and persistent rows are byte-identical;
4. Full0 association to R56 is quantified, not assumed;
5. steady local wall is materially below R56 and the five-lane tail stays
   below 60 seconds before packaging or any submission request.

No upload or submission is authorized.
