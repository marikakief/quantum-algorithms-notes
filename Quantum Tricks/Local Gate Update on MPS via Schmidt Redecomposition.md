# Local Gate Update on MPS via Schmidt Redecomposition

> **Source:** Vidal, arXiv:quant-ph/0301063
> **Tags:** #trick #tensor-networks #MPS #classical-simulation

## What it does
Updates an MPS representation after a two-qubit gate acts on adjacent sites, in $O(\chi^3)$ time independent of system size.

## The trick
Given the [[MPS Canonical Form for Efficient State Tracking|MPS canonical form]] with tensors $\Gamma^{[l]}, \lambda^{[l]}, \Gamma^{[l+1]}$ for sites $l, l+1$:

1. **Contract** the two-site tensor: compute $\Theta^{ij}_{\alpha\gamma} = \sum_\beta \sum_{kl} V^{ij}_{kl} \Gamma^{[l]k}_{\alpha\beta} \lambda^{[l]}_\beta \Gamma^{[l+1]l}_{\beta\gamma}$ where $V$ is the two-qubit gate.

2. **Redecompose:** Compute the reduced density matrix $\rho'^{[(l+1)\cdots n]}$ from $\Theta$, diagonalise it to get the new Schmidt vectors and coefficients.

3. **Extract:** Read off $\Gamma'^{[l]}, \lambda'^{[l]}, \Gamma'^{[l+1]}$ from the eigenvectors and eigenvalues.

The contraction and diagonalisation both cost $O(\chi^3)$. No other tensors in the MPS chain are affected — the update is strictly local.

## When to reach for it
- Time-evolution simulation of 1D systems (this is the core operation in TEBD)
- Classical simulation of quantum circuits where you need to track entanglement growth gate-by-gate
- Verifying quantum circuit outputs classically when the circuit has limited entanglement

## Complexity
$O(\chi^3)$ per gate, where $\chi$ is the bond dimension. For non-adjacent qubits, add $O(n)$ SWAP gates at $O(\chi^3)$ each, giving $O(n\chi^3)$ per non-adjacent gate.

## Caveat
If the gate increases entanglement, $\chi$ can grow (up to $2\chi$ for a single gate). For circuits that steadily increase entanglement, the bond dimension grows exponentially and the simulation becomes intractable. In practice, one truncates the Schmidt decomposition to keep only the largest $\chi_{\max}$ values, accepting a controlled approximation error $\delta$ per step.

## Related notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]] — TEBD algorithm that uses this update as its core operation
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Trotter Gate Decomposition for 1D MPS Simulation]]
