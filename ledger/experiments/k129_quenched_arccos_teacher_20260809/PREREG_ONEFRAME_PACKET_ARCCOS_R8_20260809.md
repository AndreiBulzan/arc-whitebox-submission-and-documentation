# Preregistration: one-frame packetized arc-cosine self-calibration R8

Date: 2026-08-09

## Question

Can one replace the canonical K129 cloud by exactly one Gaussian-packet pair
per Kerdock line, then use that same cloud's layer-31 mean and the proven
arc-cosine readout to remove its own quadrature error?

This is materially different from the killed packet selector: it does not try
to predict the unavailable best candidate from eight outputs. A target-free
fixed rule selects one candidate before propagation. The selected packet
cloud has exactly the canonical cloud's 66,048 branch rows.

## Component construction

On the frozen R2--R7 rows, generate the existing nested eight-candidate packet
pool. Propagate all candidates in the oracle only, so multiple fixed one-frame
rules can be replayed from one capture. The production-shaped candidate uses
only its selected 66,048 rows.

Frozen selection rules:

```text
fixed0
line_mod8
basis_plus_line_mod8
basis_xor_line_mod8
random_a (seed 2026080918)
random_b (seed 2026080919)
```

For each selected cloud, retain 129 basis-level synthetic endpoints and 129
actual final endpoints. Let `m_S` be its exact layer-31 mean. With the sealed
canonical K129 covariance `C_K`, construct Gaussian-ReLU target features from

```text
m(alpha) = m_K + alpha * (m_S - m_K),
alpha in {0, .25, .5, .75, 1, 1.25, 1.5, 2}.
```

Also evaluate selected mean plus selected covariance. The unavailable
all-eight packet-pool mean and dense mean are retained only as capacity
diagnostics. Fit the same sum-one constrained 129-basis ridge readout used in
R4. Candidate arrays and predictions are sealed before benchmark expectation
targets open.

## Gates

- A production-shaped fixed rule passes component accuracy if one frozen
  `alpha` gives pooled raw ratio `<=0.75`, both family ratios `<=0.85`, and no
  row ratio above `1.25`.
- Preserve a weaker rule only if ratio `<=0.85` and every row improves; it
  must then pass broader target-free freezing before any implementation.
- If only the all-eight or dense mean teacher passes, retain the packetized
  basis representation but reject one-frame self-calibration; the missing
  object remains the mean estimator.
- If even the dense-mean teacher fails, reject packetized arc bases entirely.

This is **component** evidence only. No physical FlopScope row, official
Mini100 row, package, upload, submission, or remote action is authorized.
