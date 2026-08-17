# Deep spherical-Stein control R1 preregistration

Date: 2026-07-29

Evidence scope: offline component screen only.  No FlopScope session,
physical row, package, upload, submission, or remote action is permitted.

## Hypothesis

For a bias-free one-homogeneous network output `f_j` on the unit sphere and
any target-independent fixed direction `a`,

```text
g_{a,j}(u) = a·grad f_j(u) - d (a·u) f_j(u)
E_sphere[g_{a,j}] = 0.
```

Unlike earlier path-ReLU proxies, R1 evaluates the actual deep directional
derivative through every realized ReLU gate.  It asks whether these exact
zero-mean controls remove structured quadrature error at substantially lower
support.

## Frozen screen

- Rows: Full `0,1`; Generated `0,1`.
- Literal complete antipodal production Kerdock clouds: total `K=16,32`,
  equally split across orientations 0 and 1.
- No H1 repair, compression, fitted target, or production readout.
- Four target-free input directions per MLP:
  - two leading left singular directions of `W0`;
  - two leading left singular directions of the target-free linearized
    end-to-end product `W0 @ ... @ W31`.
  Signs are canonicalized by the largest-magnitude coordinate.
- Prefixes of `R=1,2,4` controls are evaluated.
- Two coefficient spellings are frozen before targets:
  - `basis_crossfit`: regress complete-basis means on opposite-parity bases;
  - `point_crossfit`: regress literal point values on opposite-parity bases.
- Ridge is fixed to `1e-6 * mean diagonal Gram`, with a numerical floor.
- Full-row predictions are sealed before the separate scorer opens targets.

The capture also checks the identity on 32,768 IID spherical points for
Full row 0 and reports normalized mean-control residuals.

## Hard promotion gate

At either K and a fixed spelling/prefix:

- pooled raw-MSE ratio to the matching literal baseline `<= 0.50` on Full;
- pooled noise-corrected ratio `<= 0.50` on Generated;
- maximum observed row ratio `<= 2.0`;
- all values finite; IID identity diagnostic structurally valid.

Anything weaker is killed rather than tuned.  Passing this component gate
would license a larger frozen capture and a counted implementation study; it
would not itself establish a deployable score.
