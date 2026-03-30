# Imaginary Time Evolution for Ground State Preparation (Classical)

> **Source:** Vidal, arXiv:quant-ph/0310089
> **Tags:** #trick #tensor-networks #MPS #TEBD #ground-state #classical-simulation

## What it does
Finds the ground state of a 1D Hamiltonian by evolving a product state in imaginary time using TEBD, projecting onto the lowest-energy eigenstate.

## The trick
Replace the real-time evolution $e^{-iH\delta}$ with imaginary time evolution $e^{-H\delta}$ in the [[Trotter Gate Decomposition for 1D MPS Simulation|TEBD Trotter decomposition]]. Starting from any product state $|\Phi_\otimes\rangle$ with non-zero overlap with the ground state $|\Psi_{\text{gr}}\rangle$:

$$|\Psi_{\text{gr}}\rangle = \lim_{\tau \to \infty} \frac{e^{-H_n \tau} |\Phi_\otimes\rangle}{\|e^{-H_n \tau} |\Phi_\otimes\rangle\|}$$

Each imaginary-time step applies local gates $e^{-F^{[l]}\delta}$ via MPS update, then renormalises. The convergence rate is controlled by the spectral gap $\Delta$ of $H_n$: excited states are suppressed relative to the ground state by $e^{-\Delta \tau}$.

Because $e^{-H\delta}$ is not unitary, the MPS update must include normalisation at each step (the Schmidt coefficients are no longer normalised after the gate application). This is straightforward: just normalise $\lambda^{[l]}$ after each redecomposition.

## When to reach for it
- Finding ground states of gapped 1D Hamiltonians when you don't have a DMRG implementation handy
- Alternative to [[Adiabatic Quantum State Preparation for Chemistry|adiabatic state preparation]] for classical simulations
- Works well when the ground state has low entanglement (small $\chi_\epsilon$)

## Complexity
Convergence time scales as $O(1/\Delta)$ where $\Delta$ is the spectral gap. Each step costs $O(nd^3\chi^3)$ via [[Trotter Gate Decomposition for 1D MPS Simulation|TEBD]]. For gapped non-critical chains, Vidal reports convergence in ~30 minutes for $n = 80$, $\chi_\epsilon = 20$ on a 2004-era desktop.

## Caveat
Convergence is slow for critical systems where $\Delta \to 0$ as $n \to \infty$. Also, at critical points the required $\chi_\epsilon$ grows (the [[Schmidt Coefficient Decay as Truncation Diagnostic|Schmidt coefficient decay]] slows), compounding the cost. For critical 1D systems, conformal field theory methods or more sophisticated tensor network approaches (MERA) are better suited.

## Related notes
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]]
- [[Trotter Gate Decomposition for 1D MPS Simulation]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]]
