# B7 tensor-power circuit screen R1 — preregistration

Evidence class: target-free algebraic component screen.

Question: does directly synthesising the two-level Kronecker transforms of
the live B7 decomposition require fewer additions/subtractions than applying
the optimal one-level circuits twice?

The exact transforms are `U2 = kron(U,U)`, `V2 = kron(V,V)`, and
`W2 = kron(W,W)`.  Their live separable costs are respectively 44, 44, and
77 add/sub nodes.  The screen uses the already-audited signed pair-CSE
synthesiser and verifies the one-level multiplication tensor before search.
No MLP targets, FlopScope sessions, or physical rows are opened.

Promotion gate: at least 8 fewer activation-side nodes across U2 and W2
(about 6.6% of the separable activation transform).  Smaller improvements
are recorded but are not sufficient on their own to justify estimator work.

Kill gate: no activation-side reduction, or only a result whose coefficients
are not exact integers / whose tensor construction differs from the live B7
Kronecker power.
