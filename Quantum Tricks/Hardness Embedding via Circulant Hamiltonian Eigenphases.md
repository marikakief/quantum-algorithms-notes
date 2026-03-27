
> **Tags:** #trick #lower-bounds #circulant #oracle-model
> **Source:** arXiv:0908.4398, Sections 3–4

## What it does

Constructs explicit Hamiltonians for which simulation is hard in the [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes|entry-oracle]] model, by encoding a hard Boolean predicate into the eigenphases of a circulant matrix.

## The construction (Childs–Kothari)

A circulant $N \times N$ matrix $H$ is diagonal in the DFT basis: its eigenvalues are exactly the DFT coefficients of its first row. By choosing the first row appropriately, one can make a specific eigenvalue $\lambda_k$ depend on a global Boolean function $f(x)$ of the oracle input $x$ (e.g., parity or a distinguishability task).

Simulating $H$ for a carefully chosen time $t$ and then measuring in the Fourier basis recovers $e^{-i\lambda_k t}$, which encodes $f(x)$. Thus any simulation algorithm for $H$ solves the underlying computational problem.

Lower bounds on the query complexity of $f$ (e.g. $\Omega(N)$ for parity) then transfer to lower bounds on simulation.

## What this proves

1. **No-fast-forwarding for dense Hamiltonians (Theorem):** There exist dense [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes|entry-oracle]] Hamiltonians with $\|H\| = 1$ such that simulating to constant precision requires $\Omega(\|H\|t)$ queries. This generalizes the [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry–Ahokas–Cleve–Sanders]] sparse result.

2. **Stronger poly-time obstruction (main result):** There exist dense $H$ for which simulation using $\text{poly}(\|H\|t, \log N)$ queries is impossible. This rules out the possibility of simulating dense Hamiltonians with cost polynomial in $\|H\|t$ alone.

## Why circulant specifically

Circulants are easy to analyze (spectrum is explicit) and have $\|\text{abs}(H)\| = \|H\|$ (no norm-gap issue). So the hardness from circulant examples is genuinely about time complexity, not a norm-gap artifact.

## Caveat

These are worst-case constructions. They involve specially designed Hamiltonians that probably don't correspond to physical systems of interest. The results say nothing about the hardness of simulating, say, the Hubbard model.

## Related notes
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]]
- [[Norm-Gap Obstruction between H and abs(H) in Oracle Simulation]]
- [[Average-Case Lower-Bound Transfer via Randomization and Concentration]]
