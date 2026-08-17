# Preregistration: Haar16 probe-prefix development screen R9A

R6 exposed one score-compatible-size target-open lead: two independent Haar16
layer-31 mean estimates, shrink `0.75`, and relative ridge `0.003` reduced the
four-row component MSE by 18.24%.  Before a disjoint confirmation, R9A asks
whether fewer than 512 synthetic final-row probes preserve that capacity.

This is explicitly a **target-open development screen** on the already-open R6
four-row bank.  It cannot validate the estimator.  It may only freeze a design
for a later disjoint target-free capture.

Use the exact R1/R4/R6 sealed arrays and regenerate their pinned CUDA probes.
Evaluate probe prefixes `{32,64,128,256,512}`, shrink
`{0.25,0.5,0.75,1.0}`, and relative ridge
`{0.0003,0.001,0.003,0.01}`.  Candidate weights are shared across the 256
actual final outputs and sum to one.  Rank cells by pooled corrected MSE,
requiring both development families to improve.  Record all cells and freeze
the winning tuple before any new confirmation capture.

No physical FlopScope row, Mini100 row, package, upload, submission, or remote
action is authorized.
