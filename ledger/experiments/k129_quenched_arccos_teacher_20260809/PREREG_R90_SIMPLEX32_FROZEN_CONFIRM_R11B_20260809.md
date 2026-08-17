# Preregistration: frozen R90 plus 32-simplex confirmation R11B

Freeze the only score-positive R11 capacity spelling before a new bank:

- exact R90/q0 incumbent;
- 32 rotated 257-point regular simplexes;
- R11 replica-0 seed rule;
- one global offline development coefficient
  `beta=0.08616077469587277`;
- prediction `q0 + beta * (simplex32_final - q0)`.

The coefficient is target-trained once on the R9B development bank and then
fixed.  It is not selected per evaluation row and never reads an evaluation
target.

Capture on 64 rows not used by R9B/R10 or R10F:

- Full `724..755`;
- Generated `8..39`.

Seal q0, query, and prediction before mapping targets.  Post-seal, report
corrected raw ratios, family ratios, row wins, bootstrap uncertainty, and a
projection using `32 * 1.08B * 257/512 = 17.3475B` query cost.

Pass to a physical implementation only if pooled raw ratio is at most `0.888`,
both families improve, at least 35/64 rows improve, and the projected score is
below R90.  This is broad statistical evidence plus projection, not Mini100 or
physical evidence.

No package, physical row, Mini100 row, upload, submission, or remote action is
authorized.
