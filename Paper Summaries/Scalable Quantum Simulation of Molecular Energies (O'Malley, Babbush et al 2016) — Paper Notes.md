> **Source:** P. J. J. O'Malley, R. Babbush, I. D. Kivlichan, J. Romero, J. R. McClean, R. Barends, J. Kelly, P. Roushan, A. Tranter, N. Ding, B. Campbell, Y. Chen, Z. Chen, B. Chiaro, A. Dunsworth, A. G. Fowler, E. Jeffrey, E. Lucero, A. Megrant, J. Y. Mutus, M. Neeley, C. Neill, C. Quintana, D. Sank, A. Vainsencher, J. Wenner, T. C. White, P. V. Coveney, P. J. Love, H. Neven, A. Aspuru-Guzik, and J. M. Martinis, *Scalable Quantum Simulation of Molecular Energies*, arXiv:1512.06860, Phys. Rev. X **6**, 031007 (2016)
> **Links:** [arXiv](https://arxiv.org/abs/1512.06860) · [Journal](https://doi.org/10.1103/PhysRevX.6.031007)
> **Tags:** #quantum-simulation #VQE #phase-estimation #quantum-chemistry #superconducting #Bravyi-Kitaev #Trotter #NISQ

---

## The computational problem

Compute the ground-state energy of a molecular Hamiltonian as a function of nuclear geometry (the Born-Oppenheimer energy surface). Formally, given the electronic structure Hamiltonian in the second-quantized form

$$H = \sum_{pq} h_{pq} a^\dagger_p a_q + \frac{1}{2} \sum_{pqrs} h_{pqrs} a^\dagger_p a^\dagger_q a_r a_s$$

find its lowest eigenvalue $E_0$. The target precision is "chemical accuracy": $1.6 \times 10^{-3}$ Hartree ($\approx 1$ kcal/mol), the threshold below which computed dissociation energies imply chemical reaction rates accurate to within an order of magnitude.

For $\text{H}_2$ in the minimal STO-6G basis, the second-quantized Hamiltonian maps to a 4-qubit operator via the [[Bravyi-Kitaev Transformation]], which can then be reduced to an effective 2-qubit Hamiltonian by exploiting symmetry:

$$\tilde{H} = g_0 \mathbf{1} + g_1 Z_0 + g_2 Z_1 + g_3 Z_0 Z_1 + g_4 X_0 X_1 + g_5 Y_0 Y_1$$

where the coefficients $\{g_\gamma\}$ are efficiently computable functions of bond length $R$.

## What the paper does

First experimental demonstration of a scalable quantum chemistry algorithm — one that does not require exponentially costly precompilation. Using two superconducting Xmon qubits (for VQE) and three qubits (for PEA), the paper runs both the [[Variational Quantum Eigensolver (VQE)]] and the canonical [[Trotterized Phase Estimation]] algorithm on the $\text{H}_2$ dissociation curve. The VQE run achieves chemical accuracy (dissociation energy error $(8 \pm 5) \times 10^{-4}$ Hartree); the PEA run does not ($( 1 \pm 1) \times 10^{-2}$ Hartree) because only a single Trotter step is feasible. The key comparison demonstrates that variational algorithms are more error-tolerant than fixed-circuit algorithms on pre-fault-tolerant hardware.

## The algorithm / construction

### Common preprocessing

1. **Born-Oppenheimer approximation:** fix nuclear positions, solve the electronic problem.
2. **Choose orbital basis:** STO-6G minimal basis gives 4 spin-orbitals → 4-qubit Fock space.
3. **[[Bravyi-Kitaev Transformation]]:** map the second-quantized Hamiltonian to a qubit Hamiltonian. For H₂, the result acts non-trivially on only 2 qubits after exploiting that qubits 1 and 3 are stabilized by the Hartree-Fock initial state.
4. Reduced Hamiltonian is Eq. (1) above, with $g_\gamma(R)$ tabulated for each bond length.

### Variational Quantum Eigensolver (VQE)

The ansatz uses the [[Unitary Coupled Cluster (UCC)]] parameterization. For H₂ in this basis, UCCSD reduces to a single parameter:

$$|\phi(\theta)\rangle = e^{-i\theta X_0 Y_1} |01\rangle$$

where $|01\rangle$ is the Hartree-Fock state and the exponent represents the only UCCSD excitation operator.

**Outer loop:**
1. Prepare $|\phi(\theta)\rangle$ with an 11-gate circuit (11 single-qubit + 2 CZ$_\pi$ gates).
2. Measure expectation values $\langle H_\gamma \rangle$ via [[Pauli Expectation Value Estimation]] (partial tomography: apply basis-change gates, then measure in the $Z$ basis).
3. Compute $E(\theta) = \sum_\gamma g_\gamma \langle H_\gamma \rangle$.
4. Classical optimizer suggests new $\theta$. Repeat until convergence.

In practice, the authors scan 1000 values of $\theta \in [-\pi, \pi)$ rather than running iterative optimization, which is fine for a single parameter but not scalable. Energy error bars use Gaussian process regression on the shot-noise-limited measurements.

**Gate count:** VQE — 11 single-qubit gates, 2 CZ$_\pi$ gates.

### Trotterized Phase Estimation (PEA)

Uses 3 qubits (1 ancilla + 2 register). Based on Kitaev's iterative phase estimation.

1. Prepare register in Hartree-Fock state $|\phi\rangle$.
2. Prepare ancilla in $(|0\rangle + |1\rangle)/\sqrt{2}$.
3. Apply controlled-$U_\text{Trot}(2^k t_0)$ where

$$U_\text{Trot}(t) \equiv \left(\prod_\gamma e^{-i g_\gamma H_\gamma t/\rho}\right)^\rho$$

with $\rho = 1$ Trotter step in the experiment.

4. The ancilla accumulates phase $e^{-i E_n t}$; measuring the ancilla gives one bit of the binary expansion of $E_0$.
5. Feed-forward classical phase kickback (Eq. 7): $\Phi^{(k)} = \pi \sum_{\ell=0}^{k-1} j_\ell 2^{\ell-k+1}$.
6. Repeat for $b$ bits; ground-state energy is $E_0^b = -\pi/t_0 \sum_{k=0}^{b-1} j_k 2^{-(k+1)}$.
7. Since $|\langle 0|\phi\rangle|^2 > 0.5$ for H₂, majority-vote over 1000 repetitions per bit.

**Gate count:** PEA — at least 51 single-qubit gates, 4 CZ$_{\phi\neq\pi}$ gates, 10 CZ$_\pi$ gates.

**Trotter ordering optimization:** For each bond length $R$, the authors classically search over orderings of the four terms $\{Z_0, Z_1, X_0X_1, Y_0Y_1\}$ to minimize the first-order Trotter error. This is tractable only for small molecules.

## Key results

**VQE result:**

$$\delta E_\text{diss}^\text{VQE} = (8 \pm 5) \times 10^{-4}\ \text{Hartree}$$

Below chemical accuracy threshold. The experiment runs at the theoretically optimal $\theta$ would yield $1.1 \times 10^{-2}$ Hartree error — more than 10× worse — showing the error-robustness is an active feature of the classical outer loop, not a coincidence.

**PEA result:**

$$\delta E_\text{diss}^\text{PEA} = (1 \pm 1) \times 10^{-2}\ \text{Hartree}$$

Fails chemical accuracy because a single Trotter step is insufficient; more steps require longer coherent evolution than the device supports.

**Variational principle:** VQE energies are always $\geq E_0$ by construction, providing a built-in error bound absent from PEA.

**Qubit overhead comparison:**

| Feature | VQE | PEA |
|---|---|---|
| Qubits | 2 | 3 |
| Gate depth | $\sim 13$ | $\sim 65+$ |
| Error correction needed | No (for H₂) | Likely, for chemical accuracy |
| Trotter steps | N/A | $\rho = 1$ (insufficient) |
| Achieved accuracy | Chemical | Not chemical |
| Scalable precompilation | Yes | Partial (ordering optimization) |

## Comparison with prior work

| Experiment | System | Algorithm | Scalable representation? | Chemical accuracy? |
|---|---|---|---|---|
| Lanyon et al. (2010) | Photonic | PEA | No (configuration basis) | Yes (H₂, with preprocessing) |
| Du et al. (2010) | NMR | PEA + adiabatic | No (configuration basis) | Yes (H₂) |
| Wang et al. (2015) | NV center | PEA | No (configuration basis) | Yes (HeH⁺) |
| Peruzzo et al. (2014) | Photonic | VQE | No (configuration basis) | Yes (HeH⁺) |
| Shen et al. (2015) | Ion trap | VQE+UCC | No (configuration basis) | Yes (H₂) |
| **O'Malley et al. (2016)** | **Superconducting** | **VQE + PEA** | **Yes (Bravyi-Kitaev)** | **Yes (VQE only)** |

The distinguishing contribution is not the accuracy per se (prior experiments were accurate too) but the scalability: all prior experiments used the configuration basis, which requires exponentially costly classical preprocessing to exponentiate the full Hamiltonian matrix. This paper is the first to use a scalable fermion-to-qubit mapping (Bravyi-Kitaev) on hardware, making the compilation cost polynomial in system size.

## Limits / caveats

- **Molecule size:** H₂ in the minimal basis. Two qubits. The experiment is a proof-of-concept; extrapolating to classically intractable molecules requires orders of magnitude more qubits and much deeper circuits.
- **Single Trotter step:** The PEA experiment uses $\rho = 1$, which is explicitly insufficient for chemical accuracy. Achieving chemical accuracy with PEA would require $\rho \sim 10$–$100$ steps, which needs circuits too deep for the device's coherence times.
- **Classical Trotter ordering optimization:** Finding the optimal Trotter ordering by classical simulation is only tractable for very small systems. Not scalable.
- **VQE scalability concern:** Scanning 1000 parameter values works here because there is one variational parameter. For larger molecules with many UCCSD amplitudes, the classical optimization becomes more expensive and the number of required measurements scales polynomially (not trivially).
- **No error correction:** Results are pre-fault-tolerant. The VQE's robustness to systematic errors (pulse imperfections, crosstalk) is empirically demonstrated but not analytically guaranteed for larger systems.
- **Hartree-Fock initial state overlap:** The majority-voting trick for PEA relies on $|\langle 0|\phi\rangle|^2 > 0.5$. For strongly correlated molecules, Hartree-Fock may have poor overlap with the ground state, making this approach unreliable.
- **Minimal basis:** STO-6G is not converged. Accurate chemistry requires larger basis sets, which increase qubit counts substantially.

## Reusable ideas

1. [[Bravyi-Kitaev Transformation]] — Maps fermionic second-quantized Hamiltonians to qubit operators with $O(\log N)$-local terms (vs. $O(N)$-local for Jordan-Wigner). Reduces gate overhead in simulations.

2. [[Symmetry Reduction in Qubit Hamiltonians]] — Exploit conserved quantum numbers (particle number, spin, parity) to freeze stabilized qubits and reduce the effective Hamiltonian. For H₂, a 4-qubit operator reduces to a 2-qubit operator.

3. [[Variational Quantum Eigensolver (VQE)]] — Hybrid classical-quantum algorithm: prepare parameterized state on quantum hardware, estimate energy via Pauli decomposition, optimize parameters classically. Error-tolerant because the classical outer loop absorbs systematic hardware noise.

4. [[Unitary Coupled Cluster (UCC) Ansatz]] — Parameterize the variational state as $e^{T(\vec\theta) - T^\dagger(\vec\theta)} |\phi_\text{HF}\rangle$ where $T$ is the cluster operator. Believed classically intractable to simulate generally; strictly more powerful than classical coupled cluster (CC) in principle.

5. [[Pauli Expectation Value Estimation]] — Decompose the molecular Hamiltonian as $H = \sum_\gamma g_\gamma P_\gamma$ (sum of Paulis) and measure each $\langle P_\gamma \rangle$ separately via basis rotation + $Z$-basis measurement. Scales polynomially in the number of Pauli terms.

6. [[Trotterized Time Evolution for Chemistry]] — Approximate $e^{-iHt}$ as $\prod_\gamma e^{-ig_\gamma H_\gamma t/\rho}$ repeated $\rho$ times. First-order error scales as $t^2/\rho$. Trotter term ordering affects the error and can be optimized per-system classically.

7. [[Iterative Phase Estimation (Kitaev)]] — Measure phase one bit at a time using single-qubit ancilla and majority voting, reducing coherence requirements compared to the textbook QPE circuit. Requires only $O(b \cdot 1/\delta^2)$ repetitions for $b$ bits at confidence $1-\delta$.

## References within this paper

| Ref | What it is | Vault note? |
|---|---|---|
| Aspuru-Guzik et al. (2005), Science 309, 1704 | Original proposal for quantum simulation of chemistry using QPE | No |
| Peruzzo et al. (2014), Nat. Commun. 5 | First VQE experiment (photonic, HeH⁺) | No |
| McClean et al. (2016), NJP 18, 23023 | Theory of variational hybrid quantum-classical algorithms | No |
| Wecker, Hastings, Troyer (2015), PRA 92, 042303 | Progress towards practical VQE | No |
| Tranter et al. (2015), Int. J. Quant. Chem. 115, 1431 | Analysis of Bravyi-Kitaev transformation properties | No |
| Seeley, Richard, Love (2012), JCP 137, 224109 | Bravyi-Kitaev transformation for electronic structure | No |
| Bravyi & Kitaev (2002), Ann. Phys. 298, 210 | Original Bravyi-Kitaev fermion mapping | No |
| Somma et al. (2002), PRA 65 | Jordan-Wigner transformation for simulation | No |
| Kitaev (1995), arXiv:9511026 | Iterative phase estimation algorithm | No |
| Trotter (1959) | Product formula for operator semi-groups | No |
| Barends et al. (2016), Nature 534, 222 | Digitized adiabatic quantum computing on same device | No |
| Kelly et al. (2015), Nature 519, 66 | State preservation by repetitive error detection (device paper) | No |
| Shen et al. (2015) | VQE + UCC in ion trap | No |
| Babbush et al. (2016), NJP 18, 033032 | Exponentially more precise fermion simulation | No |

## Cross-links

### Paper notes
*(None yet in vault — this is the first paper)*

### Trick cards
- [[Bravyi-Kitaev Transformation]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Pauli Expectation Value Estimation]]
- [[Trotterized Time Evolution for Chemistry]]
- [[Iterative Phase Estimation (Kitaev)]]
