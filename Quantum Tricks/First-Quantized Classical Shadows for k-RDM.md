# First-Quantized Classical Shadows for k-RDM

> **Source:** Babbush, Huggins, Berry et al., arXiv:2301.01203 (2023)
> **Tags:** #trick #classical-shadows #first-quantized #RDM #measurement #quantum-chemistry

## What it does

Estimates all elements of the $k$-particle reduced density matrix ($k$-RDM) of a first-quantized state using a number of measurement samples that scales **polylogarithmically in the basis size $N$** — independent of how many orbitals are in the simulation.

## The trick

In second quantization, the $k$-RDM has $O(N^{2k})$ elements and shadow tomography on $N$ qubits requires $O(N)$-dependent sample complexity. In first quantization, the state lives on $\eta$ particle registers of $\log N$ qubits each. The $k$-RDM can be reconstructed by:

1. **Sample a random $k$-subset** $S \subset \{1, \ldots, \eta\}$ of particle registers
2. **Apply a random Clifford** on the $k \cdot \lceil \log N \rceil$ qubits of those $k$ registers
3. **Measure** in the computational basis
4. **Reconstruct** using the classical shadows estimator

The particle-permutation symmetry of fermionic states (antisymmetry) concentrates the shadow estimator. The number of samples needed to achieve precision $\epsilon$ across all $\binom{N}{k}^2$ entries of the $k$-RDM is:

$$\widetilde{O}\!\left(\frac{k^k \eta^k}{\epsilon^2}\right)$$

For $L$ time points (dynamics), multiply by $L$. The cost per sample is $\mathcal{C}_\text{samp}$ (the cost of preparing $|\psi(t)\rangle$ once). No dependence on $N$ in the sample count.

The $N^{2k}$ matrix elements are reconstructed in classical post-processing, which does scale with $N$ but is purely classical compute.

## When to reach for it

- Extracting the 1-RDM (or higher-order RDMs) from first-quantized dynamics simulations where $N \gg \eta$
- Computing time-dependent observables from the [[Interaction Picture Simulation (Kinetic Frame)|interaction picture]] or first-quantized Trotter algorithms
- Any setting where you need all RDM elements, not just specific ones — the polylogarithmic $N$ scaling means the measurement budget doesn't grow with basis set refinement

## Complexity

- **Samples:** $\widetilde{O}(k^k \eta^k L / \epsilon^2)$ independent of $N$
- **Quantum cost per sample:** $\mathcal{C}_\text{samp}$ (one state preparation + random Clifford on $k \log N$ qubits)
- **Classical post-processing:** $O(N^{2k})$ to reconstruct all matrix elements

## Caveat

- The random Clifford on $k \log N$ qubits is the most expensive per-shot operation beyond state preparation. For $k=1$ and $N = 10^6$, this means a Clifford on $\sim 20$ qubits — manageable. For $k=2$, $\sim 40$ qubits — still feasible but non-trivial.
- The classical post-processing to fill $O(N^{2k})$ matrix elements could be significant for large $N$ and $k \geq 2$. In practice, you likely only need a subset of elements.
- This is specific to first quantization. Second-quantized algorithms don't get this $N$-independence because their shadow tomography acts on $N$ qubits, not $\eta \log N$.

## Related notes
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]]
- [[First-Quantized Plane-Wave Chemistry Encoding]]
- [[Shadow Tomography for QMC Overlap Estimation]]
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes]] — second-quantized analogue using matchgate circuits instead of Cliffords
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]]
