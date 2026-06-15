# Tranche 1 Resolution

Resolved Tranche 1: foundational/oracle spine.

## Summary

All substantive supervisor comments in Tranche 1 are already reflected in the current paper notes and trick cards. I did not need to make additional content edits in this pass. The fixes mainly clarify historical scope and access models: Feynman's argument is not a modern complexity lower bound, Deutsch/Deutsch-Jozsa use later cleaned-up circuit presentations, Bernstein-Vazirani's superpolynomial separation is recursive Fourier sampling rather than the basic hidden-string algorithm, Shor uses approximate power-of-two Fourier sampling, and Grover is an oracle/query result rather than an automatic literal database-lookup speedup.

## Notes Checked

- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes]]: already distinguishes Feynman's local physical-simulation argument from complexity-theoretic lower bounds, separates later Lloyd/Deutsch-Barenco-Ekert developments from the 1982 references, and scopes negativity/Bell/contextuality as later related lenses.
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]]: already keeps Deutsch's physical Church-Turing principle distinct from later efficiency language, flags the modern phase-kickback circuit as later, and describes Shor as Fourier sampling/order finding rather than a simple phase-oracle algorithm.
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]]: already labels the separation as exact quantum versus deterministic classical query complexity, includes the randomized-classical caveat in the table, and calls promise problems an early influential quantum use rather than a new invention.
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]: already separates the basic hidden-linear-function algorithm from the recursive Fourier-sampling oracle separation and states the paper's highlighted containment `BQP subset P^#P`.
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]: already frames Simon as an exponential bounded-error oracle separation, handles the `s=0` injective case, and distinguishes exact Simon coset sampling from Shor's approximate Fourier samples.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]: already defines `N`, bit length `m`, Fourier modulus `q`, and order `r`; separates per-experiment cost from repetition overhead; and treats the optional watchdog measurement as model-dependent, not fault tolerance.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]: already separates query complexity, oracle circuit cost, and QRAM/data-loading setup, fixes the optimality attribution, and scopes unknown-solution-count caveats.
- [[Phase Kickback from Oracle Queries]]: already cites Deutsch 1985 and Deutsch-Jozsa/Cleve-Ekert-Macchiavello-Mosca, says "no additional oracle calls" rather than zero overhead, and includes finite-abelian Fourier-state generalization.
- [[Hadamard Sandwich for Global Function Properties]]: already limits the trick to efficiently measurable Fourier/spectral information and distinguishes Simon's exact case from Shor's approximate QFT/continued-fraction recovery.
- [[Superposition Query for Global Properties]]: already avoids the "all values at once" misconception, separates query-model examples from Shor's bit/gate-complexity example, and weakens the algebraic-structure heuristic.
- [[Coset Sampling via Fourier Transform]]: already presents the exact finite abelian HSP template, moves the nonabelian caveat upfront, and handles Shor/order finding as approximate power-of-two Fourier sampling.
- [[Interference-Based Period Finding]]: already distinguishes exact divisor-period peaks from Shor-style concentration near multiples of `q/r`, and narrows classical collision lower bounds to black-box period settings.
- [[Order-Finding via QFT and Continued Fractions]]: already defines modulus versus bit length, separates per-experiment cost from direct repetition overhead, and includes candidate validation.
- [[Inversion About the Mean]]: already includes sign conventions, later-descendant labels, multi-controlled-phase cost assumptions, and marked-fraction overshoot caveats.

## Verification

Read the Tranche 1 review files against the current notes/cards. No supervisor suggestion in this tranche required rejection; the current versions already implement the requested corrections.
