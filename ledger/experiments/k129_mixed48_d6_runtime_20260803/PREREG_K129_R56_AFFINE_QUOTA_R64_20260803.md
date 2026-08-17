# K129 R56 affine quota-constrained continuation — R64 preregistration

R62/R63 are not evidence against joint continuation selection.  They
subtracted the paid primary output (RMS about `0.93`) from sidecar endpoint
atoms (RMS about `0.059`) without imposing a zero-mass coefficient
constraint.  Their greedy paths therefore selected a spurious shared offset.
Those exact non-affine spellings remain killed.

R64 fixes the mathematics by choosing q0-basis zero as an affine reference.
Every alternate and q0 atom is represented as `atom - reference`; q0-basis
zero itself is omitted from the selectable dictionary.  Any fitted linear
combination is therefore a zero-mass affine combination in the original
atoms, and R56's 48-alternate/64-q0 correction is exactly representable.

Run the same nested four-fold, equal-family quota grid as R63:

- alternate physical states `{32,36,40,44,48}`;
- q0 physical states `{8,16,24,32,48,64}`, including the one reference;
- relative ridge `{0.25,0.5,1.0}`.

Select by the worst Full/Generated target-free reconstruction ratio times
projected effective-compute ratio.  Promote only if both ratios are below
one.  No scored targets are opened.  Evidence is **component** and compute is
a **projection** pending an exact physical implementation.

