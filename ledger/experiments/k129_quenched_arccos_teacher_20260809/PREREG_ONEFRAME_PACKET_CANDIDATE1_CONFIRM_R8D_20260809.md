# Preregistration: frozen one-frame packet candidate-1 confirmation R8D

Development evidence R8C used Full640--647 and Generated88--95. Candidate
index one was the only one of eight packet frames to improve both families,
with pooled corrected ratio `0.821901`. It is now frozen; no other candidate
may be selected on confirmation.

Capture exactly candidate index one from the unchanged packet construction
and its antipode on disjoint rows:

```text
Full1000:     660..675
Generated128: 112..127
```

The estimator is the radially normalized uniform mean of those 66,048 final
branch outputs. Canonical q0 is captured in the same target-free pass.

Confirmation gates:

- pooled noise-corrected raw ratio `<=0.85`;
- Full and Generated ratios each `<=0.90`;
- at least 24/32 rows improve;
- row-bootstrap probability that pooled ratio is below one `>=0.99`;
- bootstrap median ratio `<=0.85`.

A pass licenses production graph and current-meter physical accounting. It is
still **broad statistical** evidence, not an official Mini100, physical,
packaged, remote, uploaded, or submitted result.
