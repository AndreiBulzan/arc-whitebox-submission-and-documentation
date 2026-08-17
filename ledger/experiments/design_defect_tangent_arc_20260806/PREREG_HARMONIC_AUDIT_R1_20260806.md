# Preregistration: K129 NNGP harmonic audit R1

Date: 2026-08-06

Evidence sought: **component**. This is a deterministic mathematical audit.
It opens no benchmark weights, predictions, or targets; runs no FlopScope
session; changes no estimator; and authorizes no packaging, upload, or remote
submission.

## Question

Does the depth-32 He-ReLU NNGP model, under the exact dimension and maximal
real-MUB/Kerdock inner-product multiplicities used by K129, reproduce:

1. the reported angular cubature variance near `2.43366e-7`;
2. the Gaussian radial factor near `0.9980488`;
3. the reported cumulative contributions of even harmonic degrees 6 through
   40?

This is a convention gate for the proposed multi-band design-defect tangent
ARC mechanism. It is not an accuracy test of that mechanism.

## Prior-art preflight

Queries covered `harmonic`, `Gegenbauer`, `degree-6`, `Kerdock`, `higher
design`, `TensorSketch`, `cumulant`, `Hermite`, `Edgeworth`, `ARC`, `tail
closure`, `great-circle`, and the relevant production/capture sites.

Nearest controlled artifacts:

- `higher_design_quadrature_20260729`: proves the literal K129 support is a
  spherical 5-design and closes a same-budget exact higher design.
- `degree6_harmonic_support_20260729`,
  `degree6_adjoint_support_20260729`, and
  `polarized_degree6_control_20260729`: kill several degree-6 support/router
  spellings, not an exact multi-band signed-defect propagation.
- `tensor_sketch_connected_k4_20260729`: proves an ordinary low-order tensor
  sketch is not closed through ReLU. Any tangent-ARC continuation must use a
  materially different ARC diagram/power-cumulant update and must separately
  establish that its compressed state is transportable.
- `coupled_harmonic_residual_cloud_20260729`: kills a first-harmonic residual
  cloud; it does not touch the first uncontrolled even bands 6, 8, 10, ... .
- `harmonic_alias_decoder_20260729`: kills fixed affine decoding from paid
  basis endpoints; it does not use weight-conditioned high-order contractions.
- `exact_circle_partition_20260729`: closes exact-circle quadrature by cost and
  between-plane variance.
- `k129_closed_orbit_harmonic_20260806`: confirms large multi-frame teacher
  capacity but does not produce a one-frame signed correction.

Outcome: **materially new diagnostic, not yet a materially new deployable
observable**. The static audit may run. No tangent-ARC estimator work is
licensed until an overpowered representation-capacity oracle clears the
existing transport and ReLU-closure objections.

## Frozen formulas

- Dimension `d = 256`, depth `L = 32`, signed support `N = 66,048`.
- ReLU correlation map:

  `kappa(t) = (sqrt(1-t^2) + (pi-acos(t))*t) / pi`.

- Fixed-point inner-product multiplicities:

  `1:1`, `-1:1`, `0:510`, `+1/16:32768`, `-1/16:32768`.

- Normalized Gegenbauer polynomial:

  `P_k(t) = C_k^((d-2)/2)(t) / C_k^((d-2)/2)(1)`.

- The spherical expectation is evaluated by high-order Gauss-Jacobi
  quadrature with weight `(1-t^2)^((d-3)/2)`.

## Acceptance

The report's convention is reproduced if:

- angular energy differs from `2.43366036e-7` by at most `2e-11`;
- radial factor differs from `0.9980487861` by at most `2e-8`;
- every reported cumulative fraction through degree 40 differs by at most
  `0.5` percentage point.

Passing establishes only the spectral diagnosis. It does not establish that
the per-network signed errors are observable, low rank, or cheap.

