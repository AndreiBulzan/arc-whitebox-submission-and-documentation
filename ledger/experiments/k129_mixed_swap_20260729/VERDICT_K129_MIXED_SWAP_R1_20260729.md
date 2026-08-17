# Fixed-total-K O0/O1 swap R1

Status: **killed**.

Accuracy evidence is **component** and compute arithmetic is a
**projection**. No physical row, package, upload, or remote action occurred.

The exact target-free refit selected:

- `128 O0 + O1[70]`: remove O0 basis `126`;
- `127 O0 + O1[70,45]`: remove O0 bases `12,34`.

Both fits are nonnegative and constant-exact. They minimize reconstruction
of the already frozen O1 proxy on Synthetic rows `0:512`, before target
access.

| fixed-total candidate | Synthetic512 raw ratio | Full100 current-q0 ratio |
|---|---:|---:|
| `128 O0 + O1[70]` | `0.993118` | `1.010898` |
| `127 O0 + O1[70,45]` | `1.018790` | `1.016929` |

The single swap gains `0.69%` on Synthetic but loses `1.09%` on Full. The
two-basis swap loses on both. Equal retained-O0 weights show the same
cross-family failure, so the constant-exact refit is not the cause.

For completeness, the additive single O1[70] proxy itself has raw ratios
`0.994823` Synthetic and `0.996192` Full. At the existing affine price of
`0.97505B` per added basis, those become adjusted factors `1.001418` and
`1.002796`: already negative before request residual. The additive two-basis
proxy is only `0.27–0.41%` adjusted-positive on count alone, below the
requested `0.5%` gate.

The fixed-total count projection is unchanged because the K129 and K146
anchors fit
`count = 1.656114B + 0.975050B × K`. This is not a physical arm-swap price.

Authoritative receipt: `k129_mixed_swap_r1_20260729.json`.
