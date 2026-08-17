# K129 R56 quota-constrained joint continuation — R63 preregistration

R62's unconstrained joint dictionary was novel but selected only four
alternate states and failed its target-free accuracy/compute gate.  Do not
repeat that unconstrained spelling.

The fixed follow-up prevents this greedy collapse.  Starting from the sealed
64-atom literal alternate bank and all 129 q0-basis contrasts, run nested
four-fold OMP with separate quotas:

- alternate quota in `{32,36,40,44,48}`;
- q0 quota in `{8,16,24,32,48,64}`;
- relative ridge in `{0.25,0.5,1.0}`.

At every OMP step, atoms in a family whose quota has been filled are masked.
All selected atoms are refit jointly.  Equal Full/Generated family weighting
is retained.  Price candidates with the same R62 trajectory slopes and
select by worst-family target-free reconstruction ratio times effective
compute ratio versus R56.

Promote only if both family product ratios are strictly below one.  No scored
target is opened here.  This is **component** evidence and its compute is a
**projection** until physically implemented.

