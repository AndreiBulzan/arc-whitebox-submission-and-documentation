# Deep ReLU-kernel support R1 preregistration

Date: 2026-07-29

Evidence sought: **component**. This is a target-free compact-geometry
support selection followed, only after the support artifact is sealed and
hashed, by a **broad statistical component** endpoint-atlas falsifier. It
authorizes no estimator propagation, GPU, FlopScope session, timed or
physical row, package, upload, submission, or remote action.

## Fixed hypothesis

The complete orientation-0 Kerdock arm contains all 129 bases. The candidate
orientation-1 arm contains exactly 17 bases chosen from runtime-eligible
basis IDs `1..128`; basis 0 remains excluded.

For unit directions with input correlation `rho`, define the normalized
He-ReLU correlation map

```text
kappa(rho) =
    (sqrt(1 - rho**2) + (pi - arccos(rho)) * rho) / pi.
```

R1 uses the kernel obtained by composing this exact map **31 times**. The
depth is fixed before seeing any support score and is not a grid.

Every Kerdock row is carried together with its antipode. Therefore the
cross-kernel contribution for an unsigned row pair with correlation `rho`
is, up to one support-independent positive factor,

```text
Ksym(rho) = 0.5 * (kappa^31(rho) + kappa^31(-rho)).
```

For orientation-1 basis `j`, the fixed selection score is

```text
score[j] =
    mean over every O0 basis and every row pair of Ksym(dot(x, y)).
```

The target kernel mean is rotationally invariant. At fixed support size,
the complete-O0 term, each O1 self term, and every distinct within-O1 term
are constant (orthonormal bases and mutual unbiasedness). Thus minimizing
the total RKHS discrepancy reduces exactly to selecting the 17 lowest
individual cross scores. Ties are broken by ascending basis ID. Selection
uses float64 CPU arithmetic on the hash-pinned compact Kerdock rotations and
phase table. Correlations are clipped to `[-1, 1]` only at the roundoff
boundary before each literal `kappa` evaluation.

No network weights, endpoints, predictions, or targets may be opened during
selection. The selected sorted support, full ranking, scores, input hashes,
and little-endian int16 support hash must be sealed before scoring.

## Post-seal falsifier

Unless the selected support is identical to the existing degree-6 support,
a separate scorer opens the already-frozen Full100 and Generated64 endpoint
atlas rows used by the sealed K146 broad capture. It compares:

```text
candidate =
    (129 * mean(O0 all 129)
      + 17 * mean(O1 deep-kernel support)) / 146

control =
    (129 * mean(O0 all 129)
      + 17 * mean(O1 incumbent support)) / 146
```

against exact signed-final-preactivation targets.

The scorer reports pooled MSE ratio, row-ratio median, p95 and maximum, and
the fraction of rows improved, separately for Full100 and Generated64.
There is no coefficient, reweighting, support edit, family-specific choice,
retry, or depth selection.

## Fixed gate

Promote only if both families independently satisfy:

```text
pooled candidate MSE / incumbent MSE <= 0.90
row-ratio p95                         <= 1.50
all arrays finite
```

If the support is identical to the degree-6 support, stop without opening
the endpoint atlas. If either endpoint family has pooled ratio `>= 1.0`, or
the hard gate fails, kill this exact spelling. A pass licenses only a later
full-prediction capture; it is not a final estimator or remote-score
receipt.

## Economics

Changing the 17 basis identities has prospective runtime operation delta
exactly `0B`. Offline CPU geometry work and endpoint scoring are not runtime
operations.
