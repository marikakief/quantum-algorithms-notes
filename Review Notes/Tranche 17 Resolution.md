# Tranche 17 Resolution

Resolved Tranche 17: QLSP / Differential Equations / Spectral PDEs.

## Summary

All substantive supervisor comments were implemented or addressed with scoped qualifications. The main corrections were to separate QLSP output tasks, keep access models distinct, avoid empirical-overclaim wording for adiabatic/randomized comparisons, and replace bare smoothness claims for spectral methods with quantitative derivative-bound assumptions.

## Notes Updated

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]: added a title, separated solution-state preparation from observable estimation and full classical readout, scoped BQP-completeness to state-preparation promises, softened the classical comparison, and corrected the condition-number caveat.
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]: added a title, explained why HHL had polynomial precision dependence, clarified the quantum-walk/Chebyshev implementation layer, and separated state-preparation and expectation-estimation lower-bound statements.
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]: added a title, scoped the lower-bound/output task to normalized solution-state preparation with block-encoding access, removed the informal author aside, and treated the filtering crossover as a benchmark-regime statement.
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes]]: added a title, corrected the AQC(exp) exponent direction, fixed QAOA time-step angle normalization, and marked variational improvements as empirical.
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]]: added a title, restricted constant-factor claims to the benchmark model and random instances studied, and replaced universal dominance language with empirical benchmark wording.
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes]]: added a title, clarified the role of the fast-inversion normalization factor, and replaced the false blanket `f(A+B)=g(W)` claim with the paper-specific inverse-transform setup.
- [[Quantum Linear Matrix Equations — Paper Notes]]: added a title, made vectorized-output and block-encoding normalization explicit, added a conservative theorem scaffold for Sylvester-type equations, and softened the vague BQP/classical-classification claim. Because the note is for a 2025 arXiv paper, I avoided fabricating theorem-symbol detail not already verified.
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]]: added a title, replaced invertibility language with singular-value cutoff/condition-number promises, removed an anachronistic qubitization label, and clarified that the algorithm prepares/estimates fit objects rather than printing the full fit vector.
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes]]: added a title, made the constant-coefficient theorem scope explicit, corrected the multistep recurrence accordingly, and separated later time-dependent ODE and QLSP follow-ups.
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]: added a title and the key parameters, clarified the history-state/Taylor linear-system construction, and kept the output model as solution-state preparation.
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]]: added a title and replaced the bare `C^\infty` convergence claim with quantitative high-derivative-bound assumptions.
- [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]]: added a title, exposed the PDE assumptions table, and corrected the spectral-convergence wording to require quantitative derivative bounds rather than qualitative smoothness alone.

## Verification

Scoped `git diff --check`, trailing-whitespace, control-character, and stale-phrase scans were run for all Tranche 17 notes after the edits.
