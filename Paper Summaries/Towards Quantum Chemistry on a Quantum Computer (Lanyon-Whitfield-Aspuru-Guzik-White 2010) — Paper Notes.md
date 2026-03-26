> **Source:** B. P. Lanyon, J. D. Whitfield, G. G. Gillett, M. E. Goggin, M. P. Almeida, I. Kassal, J. D. Biamonte, M. Mohseni, B. J. Powell, M. Barbieri, A. Aspuru-Guzik, A. G. White, *Towards quantum chemistry on a quantum computer*, Nature Chemistry **2**, 106–111 (2010); arXiv:0905.0887
> **Links:** [arXiv](https://arxiv.org/abs/0905.0887) · [Nature Chemistry](https://doi.org/10.1038/nchem.483)
> **Tags:** #quantum-chemistry #phase-estimation #experiment #photonic #hamiltonian-simulation #hydrogen

---

## The computational problem

Compute the full energy spectrum of a molecule — all eigenvalues of its electronic Hamiltonian — to chemical precision ($\sim 1$ kJ/mol). On a classical computer, full configuration interaction (FCI) scales exponentially in the number of orbitals. This paper tackles the simplest case: H$_2$ in the STO-3G minimal basis.

## What the paper does

First experimental demonstration of a quantum algorithm computing molecular energies. Uses a photonic quantum computer (linear optics, polarisation-encoded qubits) to calculate the complete energy spectrum of H$_2$ to 20 bits of precision via the [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|iterative phase estimation algorithm]] (IPEA).

The result itself isn't computationally interesting — H$_2$ in minimal basis is trivial classically. The paper's value is as a proof of concept: it demonstrates the full algorithmic pipeline (state encoding → controlled time evolution → phase readout) on real quantum hardware, achieving chemical precision ($\pm 16$ J/mol, well below the $\sim 1$ kJ/mol threshold). It also provides resource estimates for scaling up, finding that the fully scalable Trotterized version of the same H$_2$ simulation requires 4 qubits and 522 gates.

This was the first implementation of the IPEA using entangling gates capable of generating genuine quantum correlations, distinguishing it from earlier NMR demonstrations.

## The algorithm pipeline

### 1. Molecular Hamiltonian construction

H$_2$ in STO-3G: two spatial orbitals (bonding $\sigma_g$, antibonding $\sigma_u$), four spin-orbitals, six two-electron Slater determinants. The 6×6 Hamiltonian matrix block-diagonalises by symmetry into:
- Two 2×2 blocks: $\hat{H}^{(1,6)}$ (ground state + highest excited state) and $\hat{H}^{(3,4)}$ (two singlet/triplet states)
- Two 1×1 blocks: $\hat{H}^{(2)}$ and $\hat{H}^{(5)}$ (trivial eigenvalues)

Each 2×2 block maps to a single qubit. The time-evolution operator for each block, $\hat{U}^{(p,q)} = e^{-i\hat{H}^{(p,q)}t/\hbar}$, is a single-qubit unitary.

### 2. Iterative phase estimation (IPEA)

The IPEA extracts the eigenphase $\phi$ one bit at a time, starting from the least significant bit. At iteration $k$:

1. Prepare control qubit in $|+\rangle = (|0\rangle + |1\rangle)/\sqrt{2}$
2. Apply controlled-$\hat{U}^{2^{k-1}}$ on the register qubit
3. Apply corrective rotation $\hat{R}_z(\omega_k)$ where $\omega_k = -2\pi b$ and $b = 0.0\phi_{k+1}\phi_{k+2}\ldots\phi_m$ encodes previously measured bits
4. Measure control qubit → bit $\phi_k$

The phase relates to energy via $E = 2\pi\phi \cdot E_h$, where $E_h$ is the Hartree energy unit. For chemical accuracy ($\sim 1$ kJ/mol), 14 bits suffice since $2\pi \cdot 2^{-14} E_h \approx 1.01$ kJ/mol. They demonstrated 20 bits.

### 3. Gate decomposition

Each single-qubit unitary decomposes as:

$$\hat{U} = e^{i\alpha} \hat{R}_y(\beta) \hat{R}_z(\gamma) \hat{R}_y(-\beta)$$

The controlled version conditions $\alpha$ and $\gamma$ on the control qubit. For the $j$-th power, replace $\alpha \to j\alpha$ and $\gamma \to j\gamma$, keeping $\beta$ fixed. This is a nice property of the single-qubit case: the gate count stays constant regardless of $j$, so error doesn't accumulate with increasing bit index.

### 4. Majority vote error mitigation

Each bit is sampled $n = 31$ times. The mode (most frequent outcome) is taken as the bit value. If the single-shot success probability exceeds 0.5, this drives the bit error probability down exponentially with $n$. Simple but effective for small circuits; doesn't scale to large implementations where gate error accumulates.

## Key results

- Complete H$_2$ energy spectrum computed at multiple bond lengths, reconstructing all four potential energy curves
- Ground state energy at equilibrium (73.48 pm): $-535.58 \pm 0.03$ kJ/mol, matching the classical FCI value to 20 bits
- Precision achieved: $\pm 2^{-20} E_h \approx 16$ J/mol

**Scalability resource estimate:** A fully Trotterized simulation of H$_2$ (avoiding classical pre-computation of eigenstates) requires:
- 4 qubits
- 522 elementary gates
- Gate count for general molecules scales as $O(N^5)$ where $N$ is the number of basis functions

## Comparison with prior and subsequent work

| Paper | System | Platform | Method | Precision |
|---|---|---|---|---|
| [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes\|Aspuru-Guzik et al. (2005)]] | H$_2$O, LiH | Classical simulation | Recursive PEA | Chemical accuracy (simulated) |
| Brown et al. (2006) | Pairing Hamiltonian | NMR | Phase estimation | Low precision, NMR limitations |
| **This paper (2010)** | **H$_2$** | **Photonic** | **IPEA** | **20 bits** |
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes\|Peruzzo-McClean et al. (2014)]] | He–H$^+$ | Photonic | VQE | Chemical accuracy |

The key advance over NMR demonstrations: this uses genuine entangling gates (linear-optical CNOT) that can in principle scale to universal quantum computing. NMR approaches are constrained by ensemble measurements and questions about whether they exploit entanglement.

The key limitation compared to later work: eigenstates are prepared classically and loaded, rather than prepared on the quantum computer. [[Adiabatic State Preparation for Chemistry|Adiabatic state preparation]] and the Trotterized time-evolution decomposition are discussed but not implemented.

## Limits / caveats

1. **Non-scalable eigenstate preparation.** Exact eigenstates come from classical diagonalisation. For anything beyond tiny molecules, this defeats the purpose. The paper acknowledges this and points to [[Adiabatic State Preparation for Chemistry|adiabatic state preparation]] as the scalable solution.

2. **Constant gate count per iteration is a special case.** Because each 2×2 block maps to a single-qubit unitary, the controlled-$\hat{U}^{2^{k-1}}$ has the same gate count for all $k$. In general, implementing higher powers of $\hat{U}$ requires deeper circuits, and gate errors accumulate. The 20-bit precision achieved here doesn't extrapolate to larger molecules without error correction.

3. **Non-deterministic gates.** The linear-optical controlled-unitary gate is heralded (post-selected on coincident detection). Success probability is low (~15 events/second). This isn't a fundamental limitation — scalable linear-optical QC schemes exist — but it limits throughput.

4. **Minimal basis.** STO-3G for H$_2$ is a two-dimensional problem per symmetry block. The 522-gate resource estimate for the Trotterized version gives a sense of the gap between "works in the lab" and "useful quantum chemistry."

## Reusable ideas

1. [[IPEA with Constant-Depth Controlled Powers]] — When the controlled-$\hat{U}^j$ has a parametric decomposition where the power $j$ enters only through rotation angles (not circuit depth), the IPEA achieves arbitrary precision without depth overhead. Applies whenever the Hamiltonian block is small enough for a closed-form unitary decomposition.

2. [[Majority Vote Bit Refinement for Phase Estimation]] — Repeat each IPEA iteration $n$ times and take the mode. Drives bit error probability down exponentially with $n$ when single-shot success probability exceeds 1/2. A classical post-processing trick that requires no additional quantum resources per shot.

## References within this paper

- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — foundational quantum simulation proposal
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams-Lloyd (1997)]] — quantum algorithm for fermionic eigenvalues
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes|Kassal et al. (2008)]] — polynomial-time chemical dynamics simulation
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|Aspuru-Guzik et al. (2005)]] — first quantum chemistry eigenvalue calculations (classical simulation)
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation algorithm
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi et al. (2000)]] — adiabatic quantum computation
- Dobšíček, Johansson, Shumeiko, Wendin, PRA 76, 030306(R) (2007) — IPEA algorithm details
- Brown, Clark, Chuang, PRL 97, 050504 (2006) — NMR pairing Hamiltonian simulation
- Knill, Laflamme, Milburn, Nature 409, 46 (2001) — linear optical quantum computing (KLM) scheme

## Cross-links

### Paper notes
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] — the theoretical precursor; this paper implements a simplified version experimentally
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — next-generation photonic quantum chemistry experiment using VQE instead of PEA
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — analyses the Trotter errors relevant to the scalable version of this approach
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — source of the phase estimation algorithm used here
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]] — related Aspuru-Guzik group work on chemical dynamics
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — the scalable superconducting successor; uses Bravyi-Kitaev instead of the configuration basis used here, making the precompilation polynomial-cost
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — practical strategies for VQE+UCC, the near-term alternative to this paper's PEA approach for molecular energies
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]] — extends the active-space idea inherent in this paper's minimal-basis approach to recover full-basis accuracy via classical postprocessing

### Trick cards
- [[Recursive Phase Estimation for Qubit-Efficient Readout]] — related qubit-efficient PEA variant from Aspuru-Guzik et al. (2005)
- [[Adiabatic State Preparation for Chemistry]] — the proposed (but not implemented) state preparation method
- [[IPEA with Constant-Depth Controlled Powers]] — new trick card from this paper
- [[Majority Vote Bit Refinement for Phase Estimation]] — new trick card from this paper
