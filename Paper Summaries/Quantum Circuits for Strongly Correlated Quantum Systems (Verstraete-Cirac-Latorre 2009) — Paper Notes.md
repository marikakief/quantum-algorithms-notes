> **Source:** Frank Verstraete, J. Ignacio Cirac, José I. Latorre, *Quantum circuits for strongly correlated quantum systems*, Physical Review A 79:032316, 2009
> **Links:** [arXiv:0804.1888](https://arxiv.org/abs/0804.1888) · [PRA](https://doi.org/10.1103/PhysRevA.79.032316)
> **Tags:** #tensor-networks #MPS #quantum-circuits #integrable-systems #free-fermions #Hamiltonian-diagonalisation

---

## The computational problem

Given an integrable quantum many-body Hamiltonian on $n$ sites — one that can be solved via Jordan-Wigner + Fourier + Bogoliubov transformations — construct an explicit quantum circuit $U_{\text{dis}}$ of polynomial size that *exactly* diagonalises the Hamiltonian:

$$H = U_{\text{dis}} \tilde{H} U_{\text{dis}}^\dagger, \qquad \tilde{H} = \sum_i \omega_i \sigma_i^z$$

This gives access to the full spectrum, all eigenstates, time evolution for arbitrary $t$, and thermal states at any temperature — all from a fixed, finite-depth circuit.

## What the paper does

Shows that the exact diagonalising circuit for the XY model (including the quantum Ising chain as a special case) can be explicitly constructed from three ingredients: the Jordan-Wigner transformation (which is just a relabelling — no gates needed), a fermionic fast Fourier transform, and a Bogoliubov rotation. The total circuit has $O(n^2)$ gates and depth $O(n \log n)$. The same approach extends to the Kitaev honeycomb model and stabilizer Hamiltonians.

This is *not* about approximate simulation — it's exact diagonalisation via a unitary circuit. The price is restriction to integrable models, but the reward is complete access to the physics: any eigenstate, any time evolution, any thermal state, all from the same circuit.

## The algorithm / construction

### Step 1: Jordan-Wigner — no gates needed

The [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner transformation]] $c_i = \prod_{m<i} \sigma_m^z \cdot (\sigma_i^x - i\sigma_i^y)/2$ maps the spin Hamiltonian to a free-fermion Hamiltonian. The key observation: this transformation acts on the *labelling* of basis states, not on the coefficients. The amplitudes $\psi_{i_1 i_2 \cdots i_n}$ are unchanged. So there are literally no gates to implement — you just reinterpret the register as fermionic modes.

The catch: from this point on, any swapping of modes must carry a minus sign (fermionic statistics). This is handled by fermionic SWAP gates.

### Step 2: Fermionic fast Fourier transform

The Fourier transform on fermionic modes:

$$b_k = \frac{1}{\sqrt{n}} \sum_{j=1}^n e^{i 2\pi jk/n} c_j$$

diagonalises the translational invariance. For $n = 2^k$, this has the same butterfly structure as the classical FFT, but every crossing of lines becomes a **fermionic SWAP**:

$$U_{\text{fSWAP}} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & -1 \end{pmatrix}$$

The minus sign when both modes are occupied enforces anticommutation. The [[Fermionic Fast Fourier Transform (FFFT)|FFFT]] also uses phase gates:

$$F_k = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & \frac{1}{\sqrt{2}} & \frac{\alpha(k)}{\sqrt{2}} & 0 \\ 0 & \frac{1}{\sqrt{2}} & \frac{-\alpha(k)}{\sqrt{2}} & 0 \\ 0 & 0 & 0 & -\alpha(k) \end{pmatrix}$$

with $\alpha(k) = e^{i 2\pi k/n}$.

The FFT structure gives $O(n \log n)$ depth, but the fermionic SWAPs needed to bring non-adjacent modes together push the total gate count to $O(n^2)$. For $n = 2^k$ spins: $2^{k-1}(2^k - 1)$ total gates, of which only $kn$ are site-dependent interacting gates.

### Step 3: Bogoliubov transformation

In momentum space, the Hamiltonian still has pairing terms $b_k^\dagger b_{-k}^\dagger$. The Bogoliubov rotation:

$$a_k = \cos(\theta_k/2) b_k - i \sin(\theta_k/2) b_{-k}^\dagger$$

with

$$\theta_k = \arccos\left( \frac{-\lambda + \cos(2\pi k/n)}{\sqrt{(\lambda - \cos(2\pi k/n))^2 + \gamma^2 \sin^2(2\pi k/n)}} \right)$$

diagonalises completely. Each Bogoliubov rotation only mixes a pair of modes $(k, -k)$, implemented by a single two-qubit gate:

$$B_k = \begin{pmatrix} \cos\theta_k & 0 & 0 & i\sin\theta_k \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ i\sin\theta_k & 0 & 0 & \cos\theta_k \end{pmatrix}$$

### Full circuit structure

$$U_{\text{dis}} = U_{\text{FT}} \cdot U_{\text{Bog}}$$

After applying $U_{\text{dis}}$:

$$H_4[a] = \sum_{k=-n/2+1}^{n/2} \omega_k a_k^\dagger a_k, \qquad \omega_k = \sqrt{(\lambda - \cos(2\pi k/n))^2 + \gamma^2 \sin^2(2\pi k/n)}$$

This is equivalent to $\tilde{H} = \sum_i \omega_i \sigma_i^z$ — completely non-interacting.

## Key results

**Theorem (informal):** For the XY model on $n = 2^k$ qubits, the diagonalising circuit $U_{\text{dis}}$ has:
- Total gate count: $2^{k-1}(2^k - 1) = O(n^2)$
- Circuit depth: $O(n \log n)$
- Only $kn = O(n \log n)$ site-dependent gates (the rest are fermionic SWAPs)
- All gates are two-qubit

**Ising model special case ($\gamma = 1$):** For $n = 4$ qubits, the full dynamics requires only **6 local gates**. This is experimentally feasible with current ion trap technology.

**What the circuit gives you:**
1. Any eigenstate of $H$ — prepare a product state, apply $U_{\text{dis}}$
2. Time evolution $e^{-itH}$ — $U_{\text{dis}} e^{-it\tilde{H}} U_{\text{dis}}^\dagger$, where $e^{-it\tilde{H}}$ is just $n$ single-qubit rotations. Total gate count is **independent of $t$**.
3. Thermal states $e^{-\beta H}$ — prepare a product mixed state, apply $U_{\text{dis}}$

### Extensions

- **Kitaev honeycomb model:** Same approach works. The mapping to Majorana fermions requires ancillas (one spin → two fermions → 4 Majoranas), but these can be disentangled at the end.
- **Stabilizer Hamiltonians:** Sum of commuting Pauli products. Always diagonalisable by Clifford circuits. For topologically ordered stabilizer states, the circuit depth must scale linearly in system size (proved by [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes|Bravyi, Hastings & Verstraete (2006)]]).
- **Bethe ansatz models:** These are integrable so a diagonalising circuit must exist, but finding it explicitly is an open problem.

## Comparison with prior work

| Approach | Gate count | Depth | Access to spectrum |
|---|---|---|---|
| Trotter simulation of $e^{-itH}$ | $O(n^2 t / \varepsilon)$ | $O(n t / \varepsilon)$ | No — only time evolution |
| [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes\|Vidal (2003)]] MPS simulation | Classical poly$(n, \chi^2)$ | N/A (classical) | Only low-$\chi$ states |
| This paper — exact diagonalisation | $O(n^2)$ | $O(n \log n)$ | Full spectrum, all eigenstates |

The key advantage over Trotter methods: the gate count for time evolution is *independent of $t$*. You pay for the diagonalisation once, then any time evolution costs only $n$ single-qubit gates.

The limitation: restricted to free-fermion (integrable) models. But this class includes the XY model, quantum Ising chain, and Kitaev honeycomb — all of which exhibit quantum phase transitions, critical phases, and topological order.

## Limits / caveats

- **Only works for integrable models.** The paper explicitly constructs circuits only for models solvable by Jordan-Wigner + Fourier + Bogoliubov. Non-integrable models (e.g., Heisenberg with next-nearest-neighbour) are out of reach.
- **Gate count is $O(n^2)$, not $O(n)$.** The fermionic SWAPs dominate. For models without translational invariance, a different Fourier-like transform might reduce this, but it's not addressed.
- **Periodic boundary conditions require care.** The Jordan-Wigner transformation maps periodic spin chains to fermions with modified boundary terms. The paper handles this but notes the extra terms are suppressed for large $n$.
- **Extension to Bethe ansatz remains open.** The authors note this would be "very interesting" but don't provide a construction.
- **Not approximate — but also not general.** This is exact diagonalisation for a restricted class, not approximate simulation for general Hamiltonians. Different tool, different purpose.

## Reusable ideas

1. **[[Fermionic SWAP for Anticommutation Enforcement]]** — The fermionic SWAP gate (standard SWAP with a $-1$ phase on $|11\rangle$) enforces anticommutation statistics within a quantum circuit. Any fermionic algorithm on qubits needs this or an equivalent.

2. **[[Circuit Diagonalisation for Free-Fermion Hamiltonians]]** — Any free-fermion Hamiltonian (quadratic in creation/annihilation operators) can be exactly diagonalised by a polynomial-size circuit composed of fermionic Fourier transform + Bogoliubov rotation. This gives constant-cost time evolution and thermal state preparation.

3. **[[Jordan-Wigner as No-Op on Amplitudes]]** — The Jordan-Wigner transformation doesn't change the state amplitudes — it only reinterprets the basis. In circuit terms, it costs zero gates. The anticommutation signs are handled by subsequent fermionic SWAPs.

## References within this paper

- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal (2003)]] — MPS/tensor network simulation that this paper's circuits connect to
- Fannes, Nachtergaele, Werner (1992) — original MPS (finitely correlated states) definition
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes|Kitaev (2003/2006)]] — Kitaev honeycomb model, one of the applications
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes|Bravyi, Hastings, Verstraete (2006)]] — proves linear-depth lower bound for creating topological order (ref [14])
- Bravyi & Kitaev (2002) — fermion-to-qubit mappings
- Gottesman (1996, 1997) — stabilizer states and Clifford circuits

## Cross-links

### Paper notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — MPS framework that this paper compiles into circuits
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]] — alternative measurement-based QC; both show structured entanglement enables computation
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — uses the [[Fermionic Swap Network]] for chemistry circuits, same fermionic SWAP idea
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes]] — proves circuit depth lower bounds for topological states referenced here
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]] — MPS state preparation on quantum hardware, building on the MPS-to-circuit idea
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — uses [[Fermionic Fast Fourier Transform (FFFT)]] for plane-wave simulation

### Trick cards
- [[Fermionic SWAP for Anticommutation Enforcement]]
- [[Circuit Diagonalisation for Free-Fermion Hamiltonians]]
- [[Jordan-Wigner as No-Op on Amplitudes]]
- [[Fermionic Fast Fourier Transform (FFFT)]]
- [[Fermionic Swap Network]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Local Gate Update on MPS via Schmidt Redecomposition]]
