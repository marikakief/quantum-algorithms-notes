> **Source:** Anurag Anshu, *An alternating-minimization method for preparing low-energy states*, arXiv:2603.15495 (2026)
> **Links:** [arXiv](https://arxiv.org/abs/2603.15495)
> **Tags:** #ground-state-preparation #local-Hamiltonian #heuristic #variational #frustration-free #alternating-minimization #QMA

---

## The computational problem

Given a local Hamiltonian $H = \sum_{i=1}^m \Pi_i$ on $n$ qubits (each $\Pi_i$ a projector acting on $k$ qubits), prepare a quantum state with energy close to the ground energy. This is the state-preparation version of the [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|local Hamiltonian problem]], and is expected to be hard in general (assuming $\mathsf{QMA} \neq \mathsf{BQP}$). The paper targets the structural bottleneck: existing methods get stuck in low-variance states (e.g., eigenstates) of $H$ and cannot make further progress.

## What the paper does

Introduces a heuristic method for escaping local minima during energy-lowering procedures by exploiting the fact that local Hamiltonians with low frustration have families of "altered" Hamiltonians that share the same low-energy subspace but disagree at high energies. The central theoretical contribution is an **energy-dependent uncertainty principle** (Theorems 2.1 and 2.3): if a state $\psi$ has energy $E = \text{Tr}(H\psi)$ with respect to $H$, then the expected variance of $\psi$ with respect to a randomly sampled altered Hamiltonian is $\Omega(E)$ (local case) or $\Omega(E^2)$ (sparse case). High variance means the state is not an eigenstate of the altered Hamiltonian, so energy-lowering steps can make progress again.

**My assessment:** The theoretical idea — that frustration-free Hamiltonians come with a built-in family of non-commuting partners that can break stalling — is genuinely interesting and connects Hamiltonian complexity structure to algorithmic design in a way I haven't seen before. The uncertainty principle itself is clean and well-proved. The algorithm, however, is entirely heuristic: there are no convergence guarantees, and the numerics are on small systems (8–12 qubits). The paper is honest about this. It's best read as proposing a new structural lens on the ground-state preparation problem, not as delivering a working algorithm.

---

## The algorithm / construction

### Altered Hamiltonians

For $H = \sum_{i=1}^m \Pi_i$ (sum of projectors), define a family of altered Hamiltonians by adding random penalty terms within each projector's subspace:

$$H_{\vec{\phi}} = \sum_{i=1}^m (\Pi_i + \phi_i)$$

where each $\phi_i$ is a random pure state satisfying $\Pi_i \phi_i = \phi_i \Pi_i = \phi_i$ (i.e., $\phi_i$ lies in the range of $\Pi_i$). The distribution $D$ over $\vec{\phi}$ is pairwise independent with each pair forming a 2-design.

**Key property:** If $H$ has a zero-energy ground space, every $H_{\vec{\phi}}$ in the family shares the same ground space. But at higher energies, the altered Hamiltonians disagree — this is the content of the uncertainty principle.

### Energy-dependent uncertainty principle (local case)

**Theorem 2.1.** For any quantum state $\psi$:

$$\mathbb{E}_{\vec{\phi} \sim D}\, \text{Var}_\psi(H_{\vec{\phi}}) = \Omega(1) \cdot \text{Tr}(H\psi)$$

The proof uses 2-design moments to compute the expected variance exactly, then lower-bounds it. The key identity:

$$(\Pi_i + \phi_i)^2 = \Pi_i + 3\phi_i$$

(since $\phi_i$ is a rank-1 projector within $\Pi_i$'s range), and the 2-design averages $\mathbb{E}[\phi_i] = \Pi_i/d_i$ and $\mathbb{E}[\phi_i \otimes \phi_i] = \Pi_i^{\otimes 2}(I + \text{Swap})\Pi_i^{\otimes 2} / d_i(d_i+1)$, where $d_i = \text{rank}(\Pi_i)$.

### Energy-dependent uncertainty principle (sparse case)

For classical Hamiltonians or Hamiltonians diagonal in a known basis, Anshu constructs sparse altered Hamiltonians $H_{T,f} = H + W^\dagger W$ where $W = \sum_{\alpha,\beta} T_{\alpha,\beta} f_{\alpha,\beta} |\xi_\alpha\rangle\langle\xi_\beta|$, with $f_{\alpha,\beta}$ Gaussian with variance $\sqrt{E_\beta / t_\beta}$ and $T$ an adjacency indicator (either banded or Hamming-distance-1 in binary representation).

**Theorem 2.3.** Under a continuity assumption ($E_{\beta'} = \Omega(E_\beta) - O(t)$ for neighbouring $\beta, \beta'$):

$$\mathbb{E}_f\, \text{Var}_\psi(H_{T,f}) = \Omega(1) \cdot \text{Tr}(\psi H^2) - O(t) \cdot \text{Tr}(\psi H)$$

This gives **quadratic** energy dependence — much stronger than the linear bound for local altered Hamiltonians. The catch: these sparse altered Hamiltonians are defined in the eigenbasis, so they're only algorithmically useful when $H$ is classical (diagonal in the computational basis).

### The alternating-minimization algorithm

Two variants, both alternating an energy-lowering step with a switch to a new altered Hamiltonian:

**Variant 1: Energy measurement (Algorithm 3).** Start with $K^L$ copies of an initial product state. At each of $L$ steps:
1. Divide copies into batches of $K$
2. Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] to measure energy of each copy w.r.t. $H_{\text{current}}$
3. Keep the lowest-energy copy from each batch
4. Sample a new altered Hamiltonian $H' \sim D$, set $H_{\text{current}} \leftarrow H'$

Total copies needed: $K^L$. Total energy measurements: $K^L + K^{L-1} + \cdots + 1$.

**Variant 2: Variational (Algorithm 4).** Maintain a single state with a classical circuit description. At each step:
1. Compute the gradient direction $G = \sum_i \mu_i P_i$ where $\mu_i = \text{sign}(i\langle\psi|[P_i, H]|\psi\rangle)$
2. Apply the variational update $|\psi\rangle \mapsto e^{i G \theta}|\psi\rangle$ with $\theta$ chosen to minimise energy
3. Switch to a new altered Hamiltonian

The energy decrease per variational step is bounded by a "local variance" (Equation 6):

$$\langle\psi|e^{-i\theta G} H e^{i\theta G}|\psi\rangle \leq \langle\psi|H|\psi\rangle - \Omega(1) \sum_{i=1}^m (\text{Tr}(P_i \cdot i[\psi, H]))^2$$

This quantity is zero exactly when the local marginals of $\psi$ match those of an eigenstate — the stalling condition the altered Hamiltonians are designed to break.

---

## Key results

| Result | Statement |
|---|---|
| Theorem 2.1 (local uncertainty principle) | $\mathbb{E}\, \text{Var}_\psi(H_{\vec{\phi}}) = \Omega(\text{Tr}(H\psi))$ |
| Theorem 2.3 (sparse uncertainty principle) | $\mathbb{E}\, \text{Var}_\psi(H_{T,f}) = \Omega(\text{Tr}(\psi H^2)) - O(t) \cdot \text{Tr}(\psi H)$ |
| Claim 2.2 (ground space preservation) | Zero-energy eigenstates of $H$ remain zero-energy eigenstates of $H_{T,f}$ |
| Claim 3.1 (quartile energy decrease) | Energy measurement with $K$ copies projects onto the subspace below $\text{Quar}_H(\rho)$ w.h.p. |
| Grover lower bound consistency | The algorithm provably fails on the Grover oracle $H = I - |\alpha\rangle\langle\alpha|$ — cannot find $|\alpha\rangle$ without prior amplitude, consistent with [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV lower bound]] |

---

## Numerical results

All simulations are on 8–12 qubits/qutrits.

| Model | Algorithm | System size | Outcome |
|---|---|---|---|
| 1D AKLT (frustration-free) | Variational (Alg. 4) | 8, 10 qutrits | Altered Hamiltonians reach lower energy than standard variational; does not converge to ground state |
| Heisenberg Max-Cut (classical) | Energy measurement (Alg. 3) | 12 qubits | Near-ground energy in $L=5$ steps, ~364 measurements with $K=3$ |
| Quantum Max-Cut (frustrated) | Energy measurement (Alg. 3) | 11 qubits | Needs $L=10$, ~88,573 measurements; algorithm degrades on high frustration |
| Sparse with energy barriers | Energy measurement (Alg. 3) | 12 qubits | Ground state in $L=4$–$8$; significantly outperforms simulated annealing |
| Quantum Max-Cut from $|+\rangle^{\otimes n}$ | Variational (Alg. 4) | 10 qubits | Standard variational makes zero progress (highest eigenstate); alternating method achieves significant energy reduction |

The sparse Hamiltonian numerics are the most striking: the algorithm navigates energy landscapes with multiple local minima where simulated annealing gets stuck (reaching 0.46 vs ground energy 0 after 50,000 steps, while the alternating method reaches 0 in ~9,841 measurements).

---

## Comparison with prior work

| Approach | When it stalls | How this paper differs |
|---|---|---|
| [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes\|Phase estimation]] + projection | Low-variance states (eigenstates) | Switches Hamiltonian to break eigenstate stalling |
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes\|VQE]] / variational | [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes\|Barren plateaus]] or local minima of the loss landscape | Changes the Hamiltonian itself, not just the ansatz parameters |
| [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes\|QAOA]] | Fixed depth, local minima | Conceptually similar alternating structure, but QAOA alternates operators while this alternates Hamiltonians |
| [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes\|Dissipative/Lindbladian]] methods | Mixing time | Both change the dynamics to escape stalling; dissipative methods add jump operators with fixed $H$, while this changes $H$ |
| DMRG (classical) | Local minima in MPS parametrization | Both alternate across components; DMRG alternates tensors, this alternates Hamiltonians |
| [[Frustration-Free Gap Amplification via Walk Operator]] | Only applies to frustration-free | Gap amplification speeds up adiabatic paths; complementary to this approach |

---

## Limits / caveats

1. **No convergence guarantee.** The algorithm could in principle get stuck by having the Hamiltonian switch increase energy faster than the energy-lowering step decreases it. The paper acknowledges this and distills relevant quantities for a future convergence proof (Appendix .3), but no such proof exists.

2. **Linear vs quadratic variance.** The local uncertainty principle gives only linear variance scaling $\Omega(E)$, which for product states is tight — you can't distinguish progress from noise. The quadratic bound $\Omega(E^2)$ is only available for sparse Hamiltonians in the eigenbasis, which limits algorithmic utility to classical Hamiltonians.

3. **Variance ≠ spectral weight below mean.** Theorem 2.1 shows high variance, but what the algorithm actually needs is that a significant fraction of spectral weight lies below the mean energy ("quartile energy" gap). This is not proved — only supported numerically.

4. **Copy overhead.** The energy measurement variant (Algorithm 3) requires $K^L$ copies of the initial state. For $K=3, L=10$ (quantum max-cut), that's ~59,000 copies. A quantum memory scheme to recycle states is mentioned but not developed.

5. **Small system sizes.** All numerics are on 8–12 qubits/qutrits. Frustration effects scale with system size, so the good performance on these small instances may not extrapolate.

6. **No noise model.** The variational variant requires estimating commutators $\langle\psi|[P_i, H]|\psi\rangle$ to determine gradient direction. On real hardware with noise, these estimates would be noisy, and the interaction with [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes|barren plateaus]] is explicitly left as an open question.

---

## Reusable ideas

1. [[Altered Hamiltonian Family for Escaping Eigenstates]] — constructing a family of Hamiltonians that agree in the ground space but disagree at high energies, to break eigenstate stalling in energy-lowering algorithms
2. [[Energy-Dependent Uncertainty Principle for Local Hamiltonians]] — the result that high-energy states have high variance w.r.t. randomly altered Hamiltonians, with the variance scaling at least linearly in energy
3. [[Sparse Altered Hamiltonians via Gaussian Perturbation]] — the sparse-matrix construction $H_{T,f} = H + W^\dagger W$ that achieves quadratic variance scaling for classical Hamiltonians

---

## References within this paper

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation, used as the energy measurement subroutine
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo-McClean et al. (2014)]] — VQE, the variational approach that stalls on low-variance states
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|Farhi-Goldstone-Gutmann (2014)]] — QAOA, related alternating-operator structure
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes|Ortiz Marrero-Kieferová-Wiebe (2021)]] — barren plateaus in variational algorithms
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV (1997)]] — Grover search lower bound, used to verify consistency of the algorithm's failure on the Grover oracle
- Affleck, Kennedy, Lieb, Tasaki (1987) — the AKLT model used as a test case; not in vault
- White (1992) — DMRG; alternating minimization over tensor components; not in vault
- Landau, Vazirani, Vidick (2015); Arad, Landau, Vazirani, Vidick (2017) — rigorous DMRG-inspired algorithms; not in vault
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — local Hamiltonian is QMA-complete; Feynman-Kitaev mapping mentioned for frustration reduction
- Childs (2010) — sparse Hamiltonian simulation; not in vault

---

## Cross-links

### Related paper notes
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — the benchmark for provably efficient ground state preparation (requires initial overlap and gap knowledge; this paper aims to work without those guarantees, at the cost of being heuristic)
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]] — another approach to ground state preparation that avoids eigenstate stalling, using dissipation rather than Hamiltonian switching
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]] — early fault-tolerant ground state prep, same problem with different tools
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]] — another approach to ground-state energy via Krylov subspaces; also faces ill-conditioning / stalling issues
- [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes]] — eigenvalue search via filtering; related energy-measurement subroutine
- [[Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) — Paper Notes]] — quantum simulated annealing; related classical-to-quantum optimisation transfer
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]] — the complexity-theoretic backdrop for why ground state preparation is hard
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]] — the tightest locality result for QMA-completeness

### Related trick cards
- [[Altered Hamiltonian Family for Escaping Eigenstates]] — new (this paper)
- [[Energy-Dependent Uncertainty Principle for Local Hamiltonians]] — new (this paper)
- [[Sparse Altered Hamiltonians via Gaussian Perturbation]] — new (this paper)
- [[Frustration-Free Gap Amplification via Walk Operator]] — complementary technique for frustration-free systems
- [[Dolph-Chebyshev Eigenstate Filtering]] — eigenstate filtering as the energy-lowering step this paper plugs into
- [[Adiabatic State Preparation for Chemistry]] — an alternative approach to initial state preparation
- [[Alternating Operator Ansatz]] — related alternating structure in QAOA context
