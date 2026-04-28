> **Source:** Andrew M. Childs and Tongyang Li, "Efficient simulation of sparse Markovian quantum dynamics," *Quantum Information & Computation* **17**(11–12), 901–947 (2017).
> **Links:** [arXiv:1611.05543](https://arxiv.org/abs/1611.05543) · [QIC](https://dl.acm.org/doi/10.26421/QIC17.11-12)
> **Tags:** #open-quantum-systems #Lindblad #sparse-access #quantum-walk #markovian-dynamics #density-matrix #simulation #quantum-channels

## The computational problem

Closed quantum systems evolve under unitary dynamics $e^{-iHt}$, but real quantum systems interact with environments. The standard Markovian model for an open quantum system is the Lindblad master equation:

$$\frac{d\rho}{dt} = -i[H, \rho] + \sum_k \left( L_k \rho L_k^\dagger - \frac{1}{2} L_k^\dagger L_k \rho - \frac{1}{2} \rho L_k^\dagger L_k \right)$$

where $H$ is a Hamiltonian and $\{L_k\}$ are Lindblad (jump) operators. The question is: given quantum query access to the entries of $H$ and $\{L_k\}$, can one efficiently simulate this dynamics on a quantum computer?

This paper gives the first efficient quantum algorithm for Lindblad simulation assuming only that $H$ and each $L_k$ are sparse — specifically $d$-sparse (at most $d$ nonzero entries per row/column). No structure beyond sparsity is assumed.

## What the paper does

The paper extends sparse Hamiltonian simulation machinery to the full open-system Lindblad setting. The key steps are:

1. **Vectorization (Choi-Jamiołkowski isomorphism)**: The density matrix $\rho \in \mathbb{C}^{N\times N}$ is vectorized as $|\rho\rangle\rangle \in \mathbb{C}^{N^2}$. The Lindblad equation then becomes a linear equation $\dot{|\rho\rangle\rangle} = \mathcal{L}|\rho\rangle\rangle$ where $\mathcal{L}$ is the Lindbladian superoperator acting on $\mathbb{C}^{N^2}$.

2. **Sparse access to the Lindbladian**: They define an appropriate sparse-oracle model for accessing entries of $\mathcal{L}$. When $H$ is $d_H$-sparse and each $L_k$ is $d_k$-sparse, the superoperator $\mathcal{L}$ inherits sparse structure.

3. **Quantum walk simulation**: The Lindbladian $\mathcal{L}$ is not Hermitian (indeed, it has non-positive eigenvalues), so naive product formulas do not directly apply. The authors use a Stinespring dilation to express the dynamics as a quantum channel, then simulate this channel using a [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians|quantum walk]] approach adapted from sparse Hamiltonian simulation ([[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry et al. 2005]], [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma 2014]]).

4. **Density matrix simulation**: The output is a quantum state encoding the vectorized density matrix $|\rho(t)\rangle\rangle$, from which one can estimate expectation values of observables.

## The algorithm / construction

**Step 1 — Vectorization.** Write $|\rho\rangle\rangle = \sum_{i,j} \rho_{ij} |i\rangle|j\rangle$. The Lindblad equation becomes

$$\frac{d}{dt}|\rho\rangle\rangle = \mathcal{L}|\rho\rangle\rangle, \quad \mathcal{L} = -i(H \otimes I - I \otimes H^T) + \sum_k \left(L_k \otimes \bar{L}_k - \frac{1}{2}(L_k^\dagger L_k \otimes I) - \frac{1}{2}(I \otimes L_k^T \bar{L}_k)\right)$$

The key difficulty is that $\mathcal{L}$ is not Hermitian, so its matrix exponential $e^{\mathcal{L}t}$ is not unitary.

**Step 2 — Dilation.** $\mathcal{L}$ generates a completely positive trace-preserving (CPTP) map. One can embed the non-unitary evolution into a larger unitary via a Stinespring dilation:

$$e^{\mathcal{L} t}(\rho) = \text{Tr}_E[U(t) (\rho \otimes |0\rangle\langle 0|_E) U(t)^\dagger]$$

The challenge is to simulate $U(t)$ efficiently given only sparse access to $H$ and $\{L_k\}$.

**Step 3 — Sparse simulation of the dilated unitary.** The dilated Hamiltonian governing $U(t)$ is constructed explicitly and shown to be sparse whenever $H$ and the $L_k$ are sparse. The authors then apply the quantum walk-based sparse simulation of [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry et al. 2014]] (the interpolated walk technique) to simulate this Hamiltonian.

**Oracle model.** The algorithm uses two standard oracles for each sparse matrix $A$:
- $O_A^{\text{col}}|i,k\rangle = |i, c_A(i,k)\rangle$ — returns the $k$th nonzero column index in row $i$
- $O_A^{\text{val}}|i,j,0\rangle = |i,j, A_{ij}\rangle$ — returns entry values

**Complexity.** For an $N$-dimensional system, $d$-sparse $H$ and $L_k$, $K$ Lindblad operators, simulation time $t$, and error $\epsilon$:

$$\text{Query complexity: } O\!\left(d^2 K \cdot \left(\frac{\Lambda_\mathcal{L} t}{\epsilon}\right)^{1/2} \cdot \text{poly}\log\!\left(\frac{N \Lambda_\mathcal{L} t}{\epsilon}\right)\right)$$

where $\Lambda_\mathcal{L} = \|H\|_{\max} + \sum_k \|L_k\|_{\max}^2$ is an effective spectral norm. The dependence on $t$ is $\tilde{O}(t^{1/2})$, matching the square-root-in-time behavior of quantum walks.

A secondary result covers the case where the Lindblad operators are given in terms of a density matrix decomposition (relevant for quantum Monte Carlo steps).

## Key results

- **Theorem 1 (Main)**: The Lindblad dynamics of an $N$-dimensional system with $d$-sparse $H$ and $K$ Lindblad operators each $d$-sparse can be simulated to error $\epsilon$ in time $t$ using $\tilde{O}(d^2 K \Lambda_\mathcal{L}^{1/2} t^{1/2} / \epsilon^{1/2})$ queries to the sparse oracles.
- **Theorem 2 (Mixed initial state)**: The algorithm handles mixed initial states $\rho_0$ directly via the vectorization; preparing $|\rho_0\rangle\rangle$ can be done with an additional state preparation subroutine.
- The query complexity matches (up to poly-log factors) the optimal gate complexity for sparse Hamiltonian simulation in the same model, suggesting the result is near-optimal.
- The algorithm runs in quantum-accessible memory scaling as $O(N^2)$ in the dimension of the density matrix; no classical memory for the full state is required.

## Comparison with prior work

| Setting | Prior best | This work |
|---|---|---|
| Unitary (sparse $H$) | [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry et al. 2014]]: $\tilde{O}(d\|H\|t)$ | — (uses as subroutine) |
| Lindblad (dense) | Trivial: $O(N^4)$ classically | $O(N^2)$-qubit algorithm |
| Lindblad (local/geometrically local) | Poulin 2010 (product formula for local generators) | Extends to non-local sparse case |
| Open systems via Stinespring | [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes|Verstraete-Wolf-Cirac 2009]] (qualitative) | Quantitative with complexity bounds |

The main novelty is that *no geometric or algebraic structure* of the Lindblad operators is required beyond sparsity, and the complexity is polynomial in all parameters.

Compared to product formula approaches for open systems: product formulas apply when $[H, L_k] = 0$ or the jump operators commute, which almost never holds for arbitrary sparse $L_k$. The quantum walk approach avoids this constraint entirely.

## Limits / caveats

- The $t^{1/2}$ dependence (rather than $t^{1}$) comes from the quantum walk applied to the dilated Hamiltonian, which has norm $O(\Lambda_\mathcal{L}^{1/2})$. This is not obviously optimal; subsequent work on non-unitary dynamics (see [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin 2023]]) achieves different trade-offs.
- The algorithm outputs $|\rho(t)\rangle\rangle$ as a quantum state; extracting classical expectation values $\text{Tr}[O\rho(t)]$ still requires additional quantum measurement rounds.
- The output density matrix is approximate; the trace-nonpreserving errors from the walk-based simulation must be accounted for carefully.
- The dilation introduces an environment register of size $O(K \log d)$ qubits; for many jump operators this can be large.
- The algorithm does not produce a compact circuit description — it is query-model efficient, not immediately gate-model efficient without further compilation.
- No matching lower bound is proved; whether $t^{1/2}$ vs $t^1$ dependence is optimal for Lindblad simulation remains open.

## Reusable ideas

- **Vectorization as dimension-doubling**: Mapping a mixed-state evolution problem on $\mathbb{C}^N$ to a pure-state evolution on $\mathbb{C}^{N^2}$ allows applying all unitary simulation machinery. The cost is a quadratic blowup in dimension, which is acceptable when $N$ is exponentially large. See trick card: [[Vectorization Trick for Lindblad Simulation]].
- **Dilation of non-unitary channels to unitary dynamics**: Any CPTP map can be embedded in a unitary acting on system plus ancilla environment. This allows non-unitary dynamics to be simulated by Hamiltonian simulation techniques. See trick card: [[Stinespring Dilation for Open-System Simulation]] (trick already in vault).
- **Sparse-oracle-based Lindblad**: The sparse oracle model generalizes cleanly from Hamiltonians to superoperators; defining the "sparsity" of $\mathcal{L}$ in terms of input sparsity is the main bridge. See trick card: [[Sparse Oracle Model for Lindblad Superoperators]].

## References within this paper

- D. W. Berry, G. Ahokas, R. Cleve, B. C. Sanders (2007) — sparse Hamiltonian simulation via product formulas — [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- D. W. Berry, A. M. Childs, R. Cleve, R. Kothari, R. D. Somma (2014) — exponential improvement in sparse simulation — [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]]
- G. Lindblad (1976) — "On the generators of quantum dynamical semigroups" (foundational Lindblad paper)
- V. Gorini, A. Kossakowski, E. C. G. Sudarshan (1976) — GKSL equation
- F. Verstraete, M. M. Wolf, J. I. Cirac (2009) — dissipative quantum computation — [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]]
- M. Szegedy (2004) — quantum speed-up of Markov chains — [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- A. M. Childs (2010) — on the relationship between continuous- and discrete-time quantum walk — [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]]
- D. W. Berry, A. M. Childs, R. Kothari (2015) — nearly optimal Hamiltonian simulation — [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]]

## Cross-links

**Concept hubs:**
- [[Hamiltonian simulation]] — parent concept (Lindblad simulation as generalization)
- [[Product Formulas]] — contrasted approach for structured open systems

**Trick cards:**
- [[Single-Ancilla Lindbladian Dilation for Dissipative Simulation]] — related dilation technique already in vault
- [[Stinespring Dilation for Open-System Simulation]] — Stinespring trick card already in vault
- [[Vectorization Trick for Lindblad Simulation]] — new, from this paper
- [[Sparse Oracle Model for Lindblad Superoperators]] — new, from this paper

**Paper notes — sparse Hamiltonian simulation ancestors:**
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]]
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]]
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]]

**Paper notes — quantum walk machinery:**
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]]
- [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes]]

**Paper notes — open systems and dissipation:**
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]]
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes]]
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]

**Paper notes — competing simulation methods:**
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]]
