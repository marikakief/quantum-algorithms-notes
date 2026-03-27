# SOS Lower Bound via Gram Matrix SDP

> **Source:** King, Low, Babbush, Somma, Rubin, arXiv:2505.01528 (Section III, Appendix C)
> **Tags:** #trick #SOS #SDP #classical-preprocessing #ground-state-energy

## What it does

Given a Hamiltonian $H$ and an operator algebra $\vec{X}^{(k)}$ of degree $k$, finds the tightest lower bound $-\beta$ on the ground-state energy expressible as a sum-of-squares certificate, by solving a semidefinite program of dimension polynomial in the algebra size $L = |\vec{X}^{(k)}|$.

## The trick

Express the SOS decomposition $H + \beta\mathbb{1} = \sum_j B_j^\dagger B_j$ as a Gram matrix factorisation. Each generator $B_j = \vec{b}_j \cdot \vec{X}^{(k)}$ is a polynomial in the operator basis. Then:

$$\sum_j B_j^\dagger B_j = \vec{X}^{(k)\dagger} \left(\sum_j \vec{b}_j^\dagger \vec{b}_j\right) \vec{X}^{(k)} = \vec{X}^{(k)\dagger} \, G \, \vec{X}^{(k)}$$

where $G = \sum_j \vec{b}_j^\dagger \vec{b}_j \succeq 0$ is the **Gram matrix** ($L \times L$, positive semidefinite). The SDP is:

$$\min_G \; \beta \quad \text{s.t.} \quad H + \beta\mathbb{1} = \vec{X}^{(k)\dagger} G \vec{X}^{(k)}, \quad G \succeq 0$$

The equality constraint expands into linear constraints on $G$ by matching operator coefficients. For traceless operator bases, $\beta = \text{Tr}(G)$.

After solving, extract the generators by eigendecomposing $G = \sum_j \mu_j \vec{a}_j \vec{a}_j^\dagger$, giving $B_j = \sqrt{\mu_j}\,\vec{a}_j^\dagger \vec{X}^{(k)}$. The rank $R$ of $G$ determines the number of terms.

The dual SDP is the **pseudomoment problem**: minimise $\text{Tr}(J\tilde{\rho})$ over pseudoexpectation matrices $\tilde{\rho} \succeq 0$ satisfying algebraic consistency constraints. Strong duality holds (Slater's condition), so both programs give the same optimal $-\beta$.

## When to reach for it

- As the classical preprocessing step for [[SOS Spectral Amplification|SOSSA]]: the tighter $\beta$ is, the smaller $\Delta_{\text{SOS}}$, the bigger the quantum speedup
- To compute lower bounds on ground-state energies (classical application independent of quantum algorithms)
- When choosing the SOS algebra degree: solve the SDP at degree $k$ and degree $k+1$, compare the products $\Delta_{\text{SOS}} \cdot \lambda_{\text{SOS}}$ to decide which is better

## Complexity

Polynomial in $L = |\vec{X}^{(k)}|$. For $N$-qubit Hamiltonians:
- Degree-1 Pauli: $L = O(N)$, gives termwise SA (trivial)
- Degree-2 Majorana: $L = O(N^2)$, SDP size $O(N^2) \times O(N^2)$ — practical for $N \lesssim 150$ with GPU solvers
- Degree-3: $L = O(N^3)$, SDP size grows fast — becomes expensive

The companion paper [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low et al. (2025)]] uses the GPU-accelerated cuLoRADS solver for chemistry systems up to 150 orbitals.

## Caveat

Tighter bounds (larger algebra) produce smaller $\Delta_{\text{SOS}}$ but the resulting generators $B_j$ are more complex, potentially increasing $\lambda_{\text{SOS}}$ enough to negate the advantage. The SYK degree-3 example in the source paper shows this explicitly: $\sqrt{\Delta \cdot \lambda}$ returns to the LCU scaling despite a much tighter $\beta$. Optimising $\beta$ alone doesn't optimise the end-to-end gate cost.

## Related notes

- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes]] — source paper
- [[SOS Spectral Amplification]] — the quantum algorithm that uses this SDP output
- [[SOS Decomposition via Semidefinite Programming for Chemistry]] — chemistry-specific version
- [[Polynomial Completion via Sum-of-Squares]] — related SOS technique
- [[Rudin Sum-of-Squares Completion with Parity Control]] — another SOS trick
- [[2-RDM Physicality Restoration by PSD Projection]] — related: the dual SDP is the pseudomoment/RDM relaxation
