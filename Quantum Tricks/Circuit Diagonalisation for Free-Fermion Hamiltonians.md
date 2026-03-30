# Circuit Diagonalisation for Free-Fermion Hamiltonians

> **Source:** Verstraete, Cirac, Latorre, arXiv:0804.1888
> **Tags:** #trick #free-fermions #quantum-circuits #diagonalisation #integrable-systems #Hamiltonian-simulation

## What it does
Compiles any quadratic (free-fermion) Hamiltonian into a quantum circuit that *exactly* diagonalises it, giving constant-cost access to the full spectrum, any eigenstate, time evolution at arbitrary $t$, and thermal states at any temperature.

## The trick

A free-fermion Hamiltonian is quadratic in creation/annihilation operators:

$$H = \sum_{i,j} A_{ij} c_i^\dagger c_j + \frac{1}{2} \sum_{i,j} (B_{ij} c_i^\dagger c_j^\dagger + B_{ij}^* c_j c_i)$$

This can always be diagonalised by:
1. **Fourier transform** (if translational invariance exists) — block-diagonalises $H$ in momentum space
2. **Bogoliubov rotation** — diagonalises each momentum-sector block by mixing $b_k$ and $b_{-k}^\dagger$

Both steps compile to polynomial-size circuits. The Fourier transform uses the [[Fermionic Fast Fourier Transform (FFFT)]] ($O(n \log n)$ depth, $O(n^2)$ gates with [[Fermionic SWAP for Anticommutation Enforcement|fermionic SWAPs]]). The Bogoliubov step uses $n/2$ independent two-qubit gates.

After diagonalisation: $H = U_{\text{dis}} \tilde{H} U_{\text{dis}}^\dagger$ where $\tilde{H} = \sum_k \omega_k a_k^\dagger a_k$. Then:
- **Eigenstates:** Apply $U_{\text{dis}}$ to any product state
- **Time evolution:** $e^{-itH} = U_{\text{dis}} e^{-it\tilde{H}} U_{\text{dis}}^\dagger$, where $e^{-it\tilde{H}}$ is just $n$ single-qubit $R_z$ gates. **Cost independent of $t$.**
- **Thermal states:** $e^{-\beta H} = U_{\text{dis}} e^{-\beta \tilde{H}} U_{\text{dis}}^\dagger$ — prepare a product mixed state, then apply $U_{\text{dis}}$

For models without translational invariance, replace the Fourier step with a general Givens rotation network to perform the Bogoliubov-de Gennes transformation. The circuit structure changes but the $O(n^2)$ gate count is preserved.

## When to reach for it

- Quantum simulation of any exactly solvable free-fermion model (XY chain, Ising chain, Kitaev honeycomb, SSH model, Majorana chains)
- Benchmarking quantum hardware against known exact results
- Preparing ground states, excited states, or thermal states of free-fermion systems as subroutines in larger algorithms
- Variational ansätze inspired by free-fermion structure (Gaussian states as initial guesses)

## Complexity

- **Gates:** $O(n^2)$ — dominated by fermionic SWAPs in the Fourier step
- **Depth:** $O(n \log n)$ with translational invariance; $O(n^2)$ without
- **Time evolution cost:** $O(n^2)$ gates total, *independent of $t$*
- **Qubits:** $n$ (no ancillas for the basic construction)

## Caveat

Only works for *free-fermion* (quadratic) Hamiltonians. Any quartic interaction term ($c^\dagger c^\dagger c c$) breaks the construction. For weakly interacting systems, this provides an excellent starting point for perturbative or variational methods but not an exact solution. The Bethe-ansatz solvable models are integrable but *not* free-fermion, so this trick doesn't apply to them (that remains an open problem).

## Related notes
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes]]
- [[Fermionic Fast Fourier Transform (FFFT)]]
- [[Fermionic SWAP for Anticommutation Enforcement]]
- [[Fermionic Swap Network]]
- [[A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) — Paper Notes]]
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
