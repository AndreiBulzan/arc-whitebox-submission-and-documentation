# Exact R85 archive on official Mini100

Date: 2026-08-05

This is the corrective gate for submission 324457.  Run the unchanged R85
archive with SHA-256
`2917a1506222a2ba645cca190264e1b5365bbb28fa7e185f097a4132ee0a5897`
through one persistent official `LocalRunner` and the released FlopScope
0.10.0 participant client/server on every one of the 100 official Mini100
weight tensors.

The capture phase may open only `weights.npy`.  It must not open Mini100
targets.  It seals all 100 exact archive outputs, per-row ledgers, and
rowwise numerical association to the already sealed offline R85 prediction.
The score phase is a separate process that opens the targets only after the
exact capture exists.

The decisive diagnosis is:

- if the exact archive agrees with the offline R85 prediction broadly and
  reproduces its Mini100 score, submission 324457 was a genuine remote-family
  reversal;
- if the exact archive does not agree broadly, the one-row Full17 association
  was insufficient and the offline implementation was not a valid proxy for
  the package.

No upload or submission is authorized by this run.
