> **Source:** Frank Verstraete, Michael M. Wolf, J. Ignacio Cirac, *Quantum computation and quantum-state engineering driven by dissipation*, Nature Physics 5:633–636, 2009
> **Links:** [arXiv:0803.1447](https://arxiv.org/abs/0803.1447) · [Nature Physics](https://doi.org/10.1038/nphys1342)
> **Tags:** #dissipation #Lindbladian #open-quantum-systems #state-engineering #universal-computation #frustration-free #PEPS #MPS #topological-order

---

## The computational problem

Can engineered coupling to an environment (dissipation) — typically the enemy of quantum information — be turned into a *resource* for quantum computation and quantum state preparation? Specifically: can local, time-independent, Markovian dissipation drive a system to a unique steady state that encodes the result of an arbitrary quantum computation?

---

## What the paper does

Shows that the answer is yes on both counts, and the result is surprisingly strong:

1. **Dissipative quantum computation (DQC):** For any quantum circuit of depth $T$ on $N$ qubits, there exists a set of local Lindblad operators such that the unique steady state of the Lindbladian encodes the computation output. The steady state is reached in time $O(T^2)$, and the result is extracted by measuring an auxiliary "clock" register. No unitary gates, no state preparation, no coherent dynamics needed.

2. **Dissipative state engineering (DSE):** Any ground state of a frustration-free Hamiltonian can be prepared as the steady state of a local dissipative process. This includes [[MPS Canonical Form for Efficient State Tracking|MPS]], [[Projected Entangled Pairs as Variational Ansatz|PEPS]], graph/cluster states, and topological codes (Kitaev toric code, Levin-Wen string-net states).

3. **Dissipative phase transitions:** Since the physical properties of the steady state can change abruptly when parameters of the dissipative dynamics are varied, this implies the existence of quantum phase transitions driven by dissipation alone.

The conceptual shift is significant: dissipation isn't just something you fight — it can be the computational mechanism itself. The process is inherently self-correcting: the system is driven toward the steady state regardless of its initial state, recovering automatically from perturbations.

---

## The algorithm / construction

### Dissipative quantum computation (DQC)

Given a quantum circuit $\{U_t\}_{t=1}^T$ on $N$ qubits, define intermediate states $|\psi_t\rangle = U_t U_{t-1} \cdots U_1 |0\rangle^{\otimes N}$. Introduce an auxiliary "time" register with $T+1$ levels $\{|t\rangle\}_{t=0}^T$.

Define Lindblad operators:

$$L_i = |0\rangle_i\langle 1| \otimes |0\rangle_t\langle 0| \quad (i = 1,\ldots,N)$$

$$L_t = U_t \otimes |t+1\rangle\langle t| + U_t^\dagger \otimes |t\rangle\langle t+1| \quad (t = 0,\ldots,T)$$

The master equation is $\dot{\rho} = \mathcal{L}(\rho)$ with Lindbladian $\mathcal{L}(\rho) = \sum_k L_k \rho L_k^\dagger - \frac{1}{2}\{L_k^\dagger L_k, \rho\}_+$.

**Unique steady state:**

$$\rho_0 = \frac{1}{T+1} \sum_t |\psi_t\rangle\langle\psi_t| \otimes |t\rangle\langle t|$$

This is a uniform mixture over the history states. The computation result $|\psi_T\rangle$ is extracted by measuring the time register and postselecting on $|T\rangle$ (success probability $1/(T+1)$).

**Spectral gap:** The Liouvillian has gap $\Delta = \pi^2 / (2T+3)^2$, proved by a similarity transformation that replaces all gates $U_t$ with identities (the spectrum is independent of the computation!) followed by exact diagonalisation of the resulting tridiagonal structure. This gives convergence time $O(T^2)$.

The gap is retained under Kitaev's unary clock encoding, making all Lindblad operators strictly local (nearest-neighbour).

### Dissipative state engineering (DSE)

For a frustration-free Hamiltonian $H = \sum_\lambda H_\lambda$ (where each $H_\lambda$ is a local projector and the ground state minimises every term simultaneously), define a completely positive map:

$$T(\rho) = \sum_\lambda p_\lambda \left[ P_\lambda \rho P_\lambda + \frac{1}{m}\sum_{i=1}^{m} U_{\lambda,i} H_\lambda \rho H_\lambda U_{\lambda,i}^\dagger \right]$$

where $P_\lambda = \mathbf{1} - H_\lambda$ projects onto the local ground space and the unitaries $U_{\lambda,i}$ rotate excitations back toward the ground space. The map increases the ground-state overlap at each step: $\text{tr}[T(\rho)\Psi] \geq \text{tr}[\rho\Psi]$.

**Convergence proof:** For the completely depolarising choice of $U_{\lambda,i}$, the argument proceeds by showing that if $\text{tr}[\rho\Psi]$ is non-increasing under $T$, then $\text{tr}[\rho H] = 0$ (the state must be in the ground space). This uses a telescoping product over all interaction terms and a lower bound on partial traces by the smallest eigenvalue $\nu > 0$.

**Three speed classes:**

| Class | Hamiltonians | Convergence time |
|---|---|---|
| Fast ($\tau = O(\log N)$) | Commuting Hamiltonians where local corrections don't disturb neighbours (e.g., graph states with $U_\lambda = \sigma_z^{(\lambda)}$) | Graph states achieve $\tau = 1$ exactly via Eq. (9) |
| Medium ($\tau = O(N \log N)$) | Commuting Hamiltonians with injective PEPS ground states (excitations must propagate before annihilating) | Includes toric code, stabiliser states |
| Slow ($\tau = O(N^N)$) | General frustration-free Hamiltonians | Proof-of-principle; not practical |

### MPS preparation via tree-structured channels

For [[MPS Canonical Form for Efficient State Tracking|MPS]] ground states of injective parent Hamiltonians, a hierarchical channel with tree structure achieves convergence in $O(N \log^2 N)$ steps. The tree has $\log_2 N$ levels; level $r$ applies channels $R_{r,c}$ that project pairs of sites onto the correct reduced density operator, with probabilities $\epsilon_r = 1/M^r$ ($M = CN^2$) ensuring lower levels are applied more frequently.

---

## Key results

**DQC universality:** Dissipative quantum computation with local, time-independent Lindblad operators is as powerful as the quantum circuit model. The spectral gap $\Delta \geq \pi^2/(2T+3)^2$ is independent of both $N$ and the specific computation.

**DQC self-correction:** The steady state is reached regardless of initial state. Perturbations during evolution are automatically corrected — the system relaxes back. This is fundamentally different from circuit-model QC where errors accumulate.

**DSE for frustration-free Hamiltonians:** Every ground state of a frustration-free local Hamiltonian can be prepared dissipatively. For commuting Hamiltonians with injective PEPS ground states, convergence is $O(N \log N)$.

**Graph state preparation:** For graph states, the Lindbladian in Eq. (9) has spectral gap exactly equal to 1 (independent of system size), giving $\tau = O(1)$.

**Dissipative phase transitions:** Follow immediately: if the parameters of the cp-map interpolate between two frustration-free Hamiltonians whose ground states have different physical properties (e.g., different topological order), the steady state undergoes a phase transition.

---

## Comparison with prior work

| Model | Requirements | Error behaviour | Power |
|---|---|---|---|
| Circuit model | State init + unitary gates + measurement | Error accumulation; needs QEC | Universal |
| [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes\|Adiabatic QC]] | Initial ground state + slow path following | Sensitive to gap closing; path must be engineered | Universal (equivalent to circuit) |
| [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes\|MBQC]] | Cluster state preparation + adaptive measurements | Needs perfect cluster state | Universal |
| **DQC (this paper)** | **Local Lindblad operators** | **Self-correcting toward steady state; no state prep needed** | **Universal** |

In my assessment: this is a beautiful proof-of-principle that reframes quantum computation conceptually. The practical impact so far has been more in quantum state engineering (preparing topological codes, entangled states) than in computation per se, since the $1/(T+1)$ success probability for readout and the $O(T^2)$ convergence time aren't competitive with direct circuit execution for most tasks. But the *idea* — that dissipation is a resource, not just noise — has been very influential.

---

## Limits / caveats

- **Readout overhead:** The computation result is obtained with probability $1/(T+1)$ per measurement of the clock register. This adds a polynomial overhead.

- **Gap vs. circuit depth:** The convergence time scales as $O(T^2)$, so for deep circuits the dissipative approach is slower than direct execution.

- **NP-hard problems don't get easier:** If you try to encode an NP verification circuit with penalties on output qubits, the Liouvillian loses its nice block-diagonal structure and the gap can become exponentially small. The paper explicitly notes this — DQC doesn't circumvent complexity-theoretic barriers.

- **Engineering challenge:** Designing and implementing the specific Lindblad operators in a physical system is nontrivial. The paper outlines how to do it using ancilla qubits coupled to reservoirs (adiabatic elimination gives effective Lindblad terms), but practical realisations require careful engineering.

- **General frustration-free case:** The $O(N^N)$ convergence for general frustration-free Hamiltonians is a proof of existence, not a practical algorithm. The efficient cases (commuting Hamiltonians, injective PEPS) are the ones with real applications.

---

## Reusable ideas

1. [[Feynman Clock Lindbladian for Dissipative Computation]] — Encode a quantum circuit's history as the steady state of a Lindbladian using Feynman's clock construction. The clock register labels time steps; Lindblad operators implement forward/backward transitions between adjacent time steps. The spectrum is independent of the actual gates — all computations converge at the same rate.

2. [[Local Projection Channel for Frustration-Free Ground States]] — Drive a system toward the ground state of a frustration-free Hamiltonian by randomly measuring local projectors and applying corrections. Each step increases the ground-state fidelity. For commuting Hamiltonians, corrections don't disturb previously fixed regions, giving fast convergence.

3. [[Dissipative Phase Transitions from Engineered Lindbladians]] — Varying the parameters of a dissipative process can cause the steady state to undergo a quantum phase transition. This follows from the ability to dissipatively prepare ground states of frustration-free Hamiltonians that themselves exhibit phase transitions.

---

## References within this paper

- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes|Feynman (1982)]] — the Feynman clock / history-state construction that DQC builds on
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — adiabatic QC equivalence; DQC is a dissipative alternative with self-correcting dynamics
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes|Kitaev (2003)]] — toric code as a target for dissipative state engineering
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes|Verstraete-Cirac (2004)]] — PEPS as target states for DSE
- Kitaev-Shen-Vyalyi (2002) — unary clock encoding for making Lindblad operators local
- Lindblad (1976) — the Lindblad master equation formalism
- Poyatos-Cirac-Zoller (1996) — quantum reservoir engineering; the experimental precursor
- Perez-Garcia-Verstraete-Wolf-Cirac (2007, 2008) — MPS mathematical theory and PEPS as unique ground states of local Hamiltonians (injectivity)

---

## Cross-links

### Paper notes
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — MPS; target states for dissipative preparation
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]] — PEPS; another class of target states for DSE
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]] — TEBD for MPS; the classical simulation that DSE replaces with a physical process
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — adiabatic QC; DQC is a dissipative alternative with self-correcting dynamics
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]] — MBQC via cluster states; DSE can prepare cluster states dissipatively
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]] — toric code; preparable via DSE with $O(N\log N)$ convergence
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]] — modern follow-up: Lindbladian-based ground state preparation (extends beyond frustration-free)
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]] — related: Metropolis approach to quantum Gibbs states

### Trick cards
- [[Feynman Clock Lindbladian for Dissipative Computation]]
- [[Local Projection Channel for Frustration-Free Ground States]]
- [[Dissipative Phase Transitions from Engineered Lindbladians]]
- [[Projected Entangled Pairs as Variational Ansatz]]
- [[MPS Canonical Form for Efficient State Tracking]]
