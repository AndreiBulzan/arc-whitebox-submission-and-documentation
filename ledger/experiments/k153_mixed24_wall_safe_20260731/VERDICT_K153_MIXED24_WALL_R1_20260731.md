# K153 mixed-24 wall contraction verdict

Date: 2026-07-31

Verdict: **keep as the active contracted K161 descendant; remote score and
wall remain projections.**

## Evidence

- **Remote:** K146 R4 submission 321898 graded `1.2760592169e-7` adjusted
  and `2.1028575759e-7` raw, with 100/100 telemetry rows and zero failures.
  Its all-row mean wall was `46.5984s`; maximum was `60.5491s`.
- **Broad statistical:** the literal, target-free mixed-24 selection improved
  raw MSE versus sparse-17 by `8.285%` on Full100 and `5.902%` on
  Generated128.
- **Measured whole:** exact archive
  `94c53657e93f2c475e11823ec1072593cea0dc846a38470c41cfdd05f1c5bc39`
  passed bare setup plus initialized and steady official-runner rows. Steady
  count/effective/wall were `150.729257228B`, `169.853076128B`, and
  `33.381423369s`; the repeated prediction hash was identical.
- **Measured whole, same-host control:** K161 steady wall was
  `35.181679974s`, so K153 saved `1.800256605s` (`5.12%`) and `7.827939368B`
  counted FLOPs.
- **Projection:** applying the paired wall ratio to K161's observed remote
  `62.186452074s` tail gives `59.0047s`. Combining K146's remote raw anchor,
  the broad raw ratios, and the contracted compute gives an honest remote
  adjusted band of approximately `1.20e-7` to `1.23e-7`.

The dense-pre2 association-changing wall branch is killed: in its decisive
steady comparison it added `8.724B` FLOPs and was `1.206s` slower. The old
column-pair-first pair-outer branch is also already measured and killed.

No upload or remote submission is authorized by this verdict.
