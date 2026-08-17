# Weighted complete-basis source W1/R1 verdict

Status: **no signed-weight survivor through eight bases; a real but
unavailable-output `S=16` capacity lead remains open**.

Evidence label: **component** on fixed Full640--641 and Generated88--89.
All basis costs are projections.  No Mini100, physical row, package, upload,
or remote action was used.

Arbitrary signed least-squares weights were allowed, making this a strict
capacity relaxation of the earlier equal-weight selector.  The target-free
selection used only exact packet-source vectors, never benchmark expectation
targets, but those vectors are themselves unavailable in production.

## Cheap range

The strongest full-replicate per-row output oracle at `S=8` has:

- pooled raw reduction `24.31%`;
- Full reduction `35.79%`;
- Generated reduction `5.48%`;
- pooled raw MSE `2.05265e-7`;
- projected basis propagation `17.2B`.

It misses the preregistered 35% pooled/two-family gate.  The two independent
half-trained fits are also unstable out of half: one retains `11.44%` pooled
on the opposite half while making Generated worse; the other retains
`-3.22%` pooled.  A universal four-row support is worse than canonical at
`S=8`.

Thus unequal weights do not rescue the 4--8 basis source bridge.

## `S=16` capacity lead

The per-row full-replicate output oracle at `S=16` reaches:

- pooled raw reduction `47.64%`;
- Full reduction `54.40%`;
- Generated reduction `36.55%`;
- pooled raw MSE `1.42007e-7`;
- source residual MSE `2.583e-8`;
- projected basis propagation `34.4B`.

Both half-trained fits retain substantial correction on the opposite half:
`37.13%` and `29.92%` pooled.  The capacity is therefore not a packet-noise
artifact.  However, the fit is per-network and unavailable-output, with
maximum Gram condition about `5.5e3`; the universal `S=16` fit retains only
`18.10%` pooled and makes Generated worse.

At the measured `47.64%` reduction, a purely arithmetic transplant onto the
public R87 raw baseline plus `34.4B` would imply an adjusted score around
`7.5e-8`.  That is a **projection conditional on transferring an oracle
reduction**, not an estimator forecast or receipt.

## Decision

Close signed sparse complete-basis reconstruction only through `S=8`.
Do **not** discard the `S=16` collective-basis mechanism.  Its next required
oracle is not another subset solver; it is an accessible, target-free
approximation to each network's basis-source Gram/total contraction that
does not first evaluate all 129 exact basis sources.  A proposed bridge must
show two-family held capacity and include the cost of producing those
features.  Candidate mechanisms include a batched basis-level JVP/VJP Gram
sketch or an analytic canonical-trajectory contraction, but neither is
established by this receipt.

Receipts:

- `runtime/artifacts/weighted_basis_source_w1_r1_targetfree_20260808.json`
- `runtime/artifacts/weighted_basis_source_w1_r1_postseal_20260808.json`

