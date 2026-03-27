> **Source:** Daniel S. Abrams and Seth Lloyd, *Quantum algorithm providing exponential speed increase for finding eigenvalues and eigenvectors*, arXiv:quant-ph/9807070, Phys. Rev. Lett. **83**, 5162 (1999)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9807070) · [PRL](https://doi.org/10.1103/PhysRevLett.83.5162)
> **Tags:** #phase-estimation #eigenvalue #quantum-chemistry #hamiltonian-simulation #foundational

---

## What the paper does

Describes the first explicit algorithm for finding eigenvalues and eigenvectors of a Hamiltonian $H$ on a quantum computer, using [[Gapped Phase Estimation|phase estimation]] on the time evolution operator $U = e^{-iHt}$. Given an approximate eigenvector $|V_a\rangle$ with non-negligible overlap with the true eigenstate $|V\rangle$, the algorithm finds the eigenvalue $\lambda_V$ and collapses the system register into $|V\rangle$.

This is the paper that made eigenvalue problems — especially quantum chemistry — concrete targets for quantum computers. The estimate that 50–100 qubits could solve classically intractable atomic physics problems was influential.

---

## The algorithm

### Setup

- $m$ index qubits (for the QFT, accuracy $\sim 1/2^m$)
- $l$ qubits for the system Hilbert space
- $w$ ancilla/scratch qubits

### Step 1: Prepare initial state

$$
|\Psi\rangle = |0\rangle |V_a\rangle
$$

where $|V_a\rangle$ is a classically-obtained approximate eigenvector (e.g., from Hartree-Fock).

### Step 2: Create uniform superposition on index register

Apply Hadamards to index qubits:

$$
|\Psi\rangle = \frac{1}{\sqrt{M}} \sum_{j=0}^{M-1} |j\rangle |V_a\rangle
$$

### Step 3: Conditional time evolution

Apply $U^j$ to the system register conditioned on the index register:

$$
|\Psi\rangle = \frac{1}{\sqrt{M}} \sum_{j=0}^{M-1} |j\rangle U^j |V_a\rangle
$$

**Implementation:** Loop $i = 1$ to $M$: set a flag qubit if $i < j$, apply $U$ conditioned on the flag, uncompute the flag.

**Note:** The paper's loop-based construction for conditional-$U^j$ is functional but unnecessarily complicated. The standard modern approach uses controlled-$U^{2^k}$ for each index qubit $k$ (the standard [[Gapped Phase Estimation|phase estimation]] circuit from Cleve, Ekert, Macchiavello & Mosca 1998), which avoids the loop entirely. The more serious sorting bug — using data-dependent heap sort for antisymmetrization of fermionic states — is in the companion paper [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams-Lloyd (1997)]], not this one.

### Step 4: Decompose in eigenbasis

Write $|V_a\rangle = \sum_k c_k |\phi_k\rangle$ where $|\phi_k\rangle$ are eigenstates of $U$ with eigenvalues $\lambda_k = e^{i\omega_k}$:

$$
|\Psi\rangle = \frac{1}{\sqrt{M}} \sum_k c_k |\phi_k\rangle \sum_{j=0}^{M-1} e^{i\omega_k j} |j\rangle
$$

### Step 5: Quantum Fourier Transform

Apply QFT to the index register. The frequencies $\omega_k$ are resolved, giving eigenvalues $\lambda_k$. Eigenvalue $\lambda_k$ appears with probability $|c_k|^2 = |\langle V_a | \phi_k \rangle|^2$.

### Step 6: Measure

Measuring the index register collapses the system register into the corresponding eigenstate $|\phi_k\rangle$.

```
|0⟩^m ──[H]──┤ controlled-U^j ├──[QFT]──[Measure]── eigenvalue λ_k
              │                │
|V_a⟩ ────────┤    system      ├──────────────────── eigenstate |φ_k⟩
              │                │
              └────────────────┘
```

---

## Complexity

- [[Hamiltonian simulation]]: $O(t^2/\epsilon)$ gates via [[Order-Condition Cancellation in Product Formulas|Trotter]] (using Lloyd's method)
- Phase estimation: $M = 2^m$ controlled applications of $U$
- Total: $O(M \cdot t^2/\epsilon)$ for eigenvalue precision $\sim 1/M$
- Eigenvector found with probability $|c_k|^2$ per trial → $O(1/|c_k|^2)$ repetitions

For ground state: if $|V_a\rangle$ has expected energy below the first excited state by a non-exponentially-small amount, then $|c_0|^2$ is non-negligible.

---

## Application to quantum chemistry

For a system of $n$ particles with Hamiltonian $H = \sum_i (T_i + V_i) + \sum_{i>j} V_{ij}$:

- First-quantized representation: $l = n \cdot l_{\text{single}}$ qubits
- [[Order-Condition Cancellation in Product Formulas|Trotter]] splitting: $U(t) = e^{-iHt} \approx \prod_i e^{-iH_i t/m}$ for each local term $H_i$
- Position/momentum switching via QFT for kinetic vs. potential terms

**Boron atom estimate:** 5 electrons, 800 single-particle basis functions → $800^5 \approx 10^{15}$ many-body states. Requires ~60 qubits (50 system + 6–7 FFT + scratch). With position-space representation for Coulomb interactions: ~100 qubits total.

---

## Where this sits historically

| Paper | What it adds |
|---|---|
| **This paper (1998)** | First explicit eigenvalue/eigenvector algorithm via phase estimation |
| Cleve, Ekert, Macchiavello & Mosca (1998) | Phase estimation for eigenvalues of unitaries (cleaner circuit, no loop) |
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes\|HHL (2009)]] | Uses phase estimation + conditional rotation for matrix inversion |
| [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes\|Lin & Tong (2020)]] | Near-optimal ground state prep via [[QSVT Meta-Template\|QSVT]] polynomial filters |
| [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes\|Berry, Kieferová et al. (2018)]] | Fixes the sorting issue, adds early-reject and qubitization-based evolution |

---

## Reusable ideas

1. **Phase estimation for eigenvalue extraction:** Apply $U^j$ conditioned on index register $|j\rangle$, QFT to resolve frequencies, measure to get eigenvalues. The eigenvector collapses onto the system register as a side effect. This became the standard subroutine for quantum chemistry and later for [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]].

2. **Approximate eigenvector as input:** The algorithm doesn't need an exact eigenstate — just a state with polynomial overlap. Classical methods (Hartree-Fock, CI) provide this for many physical systems.

3. **First-quantized representation with basis switching:** Represent particles in an orbital basis for storage, switch to position space (via QFT) for Coulomb interactions, switch back. Trades qubits for gates.

---

## References within this paper

- Lloyd (1996, Science 273, 1073) — [[Order-Condition Cancellation in Product Formulas|Trotter-based]] [[Hamiltonian simulation]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — eigenvalue measurement procedure (phase estimation)
- Cleve, Ekert, Macchiavello & Mosca (1998, quant-ph/9708016) — [[Gapped Phase Estimation|phase estimation]] for eigenvalues of unitaries (streamlined version of Kitaev's approach)
- Feynman (1982) — quantum simulation proposal
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams & Lloyd (1997, PRL 79, 2586)]] — earlier work on fermionic simulation, antisymmetrization, and Hubbard model

---

## Cross-links

### Paper notes
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]] — the companion paper on fermionic simulation (contains the heap sort bug)
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]] — fixes the sorting issue from the 1997 paper, improves antisymmetrization
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — uses phase estimation from this paper for matrix inversion
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — modern ground state prep via [[QSVT Meta-Template|QSVT]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — improved [[Hamiltonian simulation]] used in the $e^{-iHt}$ step
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]] — alternative ground-state-finding approach

### Trick cards
- [[Gapped Phase Estimation]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Basis Switching via QFT for Kinetic-Potential Splitting]]
