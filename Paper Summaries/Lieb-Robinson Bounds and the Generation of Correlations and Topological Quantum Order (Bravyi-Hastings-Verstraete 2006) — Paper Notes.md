> **Source:** Sergey Bravyi, Matthew B. Hastings, Frank Verstraete, *Lieb-Robinson bounds and the generation of correlations and topological quantum order*, Physical Review Letters 97:050401, 2006
> **Links:** [arXiv:quant-ph/0603121](https://arxiv.org/abs/quant-ph/0603121) · [PRL](https://doi.org/10.1103/PhysRevLett.97.050401)
> **Tags:** #Lieb-Robinson #information-propagation #topological-order #entanglement-growth #area-law #circuit-depth-lower-bound #Hamiltonian-simulation

---

## The computational problem

Given a local Hamiltonian $H(t) = \sum_{(i,j) \in E} h_{ij}(t)$ on a graph $G = (V, E)$, how fast can:
1. Information propagate from region $A$ to region $B$?
2. Correlations be created between distant regions?
3. Topological quantum order be generated from a trivially-ordered state?
4. Entanglement entropy of a block grow under time evolution?

## What the paper does

Translates the Lieb-Robinson bound — an effective light cone for non-relativistic quantum lattice systems — into four concrete information-theoretic consequences. The results are:

1. **Information leakage is exponentially small outside the light cone.** The Holevo capacity of the A→B channel decays as $e^{-(L - v|t|)/\xi}$.
2. **Correlations can only spread at finite speed.** Connected correlation functions between regions separated by distance $L$ remain exponentially small until time $t \sim L/(2v)$.
3. **Topological order requires $\Omega(L)$ time to create.** No local Hamiltonian evolution can convert a non-topologically-ordered state to a topologically-ordered one in time less than linear in the system size.
4. **Entanglement entropy growth obeys an area law in time.** The rate of entropy increase for a block scales as its surface area, not its volume.

These are foundational constraints that underpin *all* Hamiltonian simulation algorithms. They explain why local simulation methods can work, why [[Product Formulas|Trotter methods]] scale with local structure, and why the [[Lieb-Robinson Decomposition for Lattice Simulation]] achieves optimal $O(nT)$ gate complexity.

## The construction / proofs

### The Lieb-Robinson bound (starting point)

For operators $O_A$ and $O_B$ acting on non-overlapping regions $A, B$ at distance $L$:

$$\|[O_A(t), O_B(0)]\| \leq c \, N_{\min} \|O_A\| \|O_B\| \exp\left(-\frac{L - v|t|}{\xi}\right)$$

where $N_{\min} = \min\{|A|, |B|\}$ and $c, v, \xi > 0$ are constants depending on the interaction strength $g = \max_{(i,j)} \max_t \|h_{ij}(t)\|$ and the maximum vertex degree of $G$. The velocity $v$ is the Lieb-Robinson velocity — an effective "speed of sound" for quantum information.

The original bound is from Lieb-Robinson (1972), with generalisations by Hastings (2004) and Nachtergaele-Sims (2005). This paper applies it to derive new consequences.

### Result 1: No-signalling with exponential precision

Alice encodes message $k$ by applying $U_A^k$ to her region. Bob measures $O_B$ after time $t$. The observed state at $B$:

$$\sigma_B^k(t) = \text{Tr}_{AC}\left[U_{ABC}(t) \, U_A^k \rho_0 U_A^{k\dagger} \, U_{ABC}(t)^\dagger\right]$$

satisfies $\|\sigma_B^k(t) - \sigma_B^0(t)\|_1 \leq \varepsilon$ where $\varepsilon = c \, N_{\min} \, e^{-(L - v|t|)/\xi}$.

Combined with the Fannes inequality, this gives the Holevo capacity bound:

$$C_\chi(t) \leq 2\varepsilon(|B| \log m - \log \varepsilon)$$

which decays exponentially for $L \gg v|t|$.

### Result 2: Finite velocity for correlation generation

Starting from a state $|\psi\rangle$ with correlation length $\chi$ (all connected correlators decay as $e^{-L/\chi}$), how fast can local Hamiltonian evolution create correlations at distance $L$?

The key technical step: approximate the time-evolved operator $O_A(t)$ by an operator $O_A^l(t)$ supported only on the "light cone" of $A$ (sites within distance $l$ of $A$):

$$\|O_A(t) - O_A^l(t)\| \leq c|A| \exp\left(-\frac{l - v|t|}{\xi}\right)$$

This approximation is constructed by averaging $O_A(t)$ over all unitaries on the complement of the light cone:

$$O_A^l(t) = \frac{1}{\text{Tr}_S(\mathbb{1}_S)} \text{Tr}_S(O_A(t)) \otimes \mathbb{1}_S$$

Then the connected correlator at time $t$ satisfies:

$$|\langle O_A(t) O_B(t) \rangle_c| \leq \bar{c}(|A| + |B|) \exp\left(-\frac{L - 2vt}{\chi'}\right)$$

where $\chi' = \chi + 2\xi$. Correlations cannot reach distance $L$ faster than $t \sim L/(2v)$.

**Application:** The GHZ state has infinite-range correlations. The ferromagnetic state $|+\cdots+\rangle$ has finite correlation length. Converting one to the other by local evolution takes time $\Omega(L)$ where $L$ is the system diameter.

### Result 3: Topological order requires $\Omega(L)$ time

A state pair $(|\psi_1\rangle, |\psi_2\rangle)$ has topological quantum order (TQO) with accuracy $(l, \varepsilon)$ if for any local observable $O_{\text{loc}}$ with $\|O_{\text{loc}}\| = 1$ supported on diameter $\leq l$:

$$|\langle\psi_1|O_{\text{loc}}|\psi_1\rangle - \langle\psi_2|O_{\text{loc}}|\psi_2\rangle| \leq 2\varepsilon, \qquad |\langle\psi_1|O_{\text{loc}}|\psi_2\rangle| \leq \varepsilon$$

**Proof by contradiction:** Suppose $|\psi_1\rangle = U|\psi_0\rangle$ where $|\psi_0\rangle$ has no TQO and $U$ is generated by local Hamiltonian evolution for time $t$. Define $|\tilde{\psi}_0\rangle = U^\dagger |\psi_2\rangle$. Since $\langle\psi_0|\tilde{\psi}_0\rangle = 0$ (orthogonal), and the Lieb-Robinson bound limits how much $U O_{\text{loc}} U^\dagger$ can differ from a local operator, the TQO properties of $|\psi_1\rangle, |\psi_2\rangle$ transfer back to $|\psi_0\rangle, |\tilde{\psi}_0\rangle$ — contradicting the assumption that $|\psi_0\rangle$ has no TQO.

The bound: if $vt \ll l_f/2$ (where $l_f$ is the TQO lengthscale of the final state), then the initial state must already be topologically ordered.

**Consequence:** Creating toric code states from product states requires circuit depth $\Omega(L)$. This proves that the Dennis-Kitaev-Landahl-Preskill procedure is essentially optimal.

### Result 4: Area law for entanglement generation

For a Hamiltonian coupling block $A$ to its complement $B$:

$$H(t) = H_A(t) + H_B(t) + \sum_{k=1}^P r_k(t) J_A^k \otimes J_B^k$$

where $\|J_A^k\|, \|J_B^k\| \leq 1$ and $P$ scales as the perimeter of $A$. Using the result of Childs-Leung-Verstraete-Vidal that the maximum entangling rate of a product Hamiltonian $J_A \otimes J_B$ is bounded by $c^* \approx 1.9$ (independent of Hilbert space dimension):

$$\frac{dS(\rho_A)}{dt} \leq c^* \sum_{k=1}^P |r_k(t)|$$

Therefore:

$$S(\rho_A(t)) - S(\rho_A(0)) \leq c^* g P t$$

The entanglement generated in a block scales as **perimeter × time**, not volume. If you start with an area-law state and evolve for finite time, you still have an area-law state.

## Key results

| Result | Statement |
|---|---|
| Information leakage | $C_\chi(t) \leq 2\varepsilon(|B|\log m - \log\varepsilon)$, $\varepsilon = c N_{\min} e^{-(L-v|t|)/\xi}$ |
| Correlation velocity | Correlations bounded by $e^{-(L-2vt)/\chi'}$ |
| TQO creation time | $\Omega(L)$ where $L$ is system size |
| Entanglement rate | $dS/dt \leq c^* g P$ where $P$ = perimeter |
| Max entangling rate constant | $c^* = 2\max_{0 \leq x \leq 1} \sqrt{x(1-x)} \log\frac{x}{1-x} \approx 1.9$ |

## Comparison with prior work

| Work | Contribution |
|---|---|
| Lieb-Robinson (1972) | Original commutator bound |
| Hastings (2004), Nachtergaele-Sims (2005) | Generalised to broader lattice geometries |
| Childs-Leung-Verstraete-Vidal (2003) | Entangling capacity of bipartite Hamiltonians |
| **This paper** | Translates LR bounds into information-theoretic consequences: no-signalling, correlation velocity, TQO lower bound, area law in time |

The TQO result was new and non-trivial — circuit depth lower bounds are notoriously hard to prove, and this was one of the first for a physically motivated task.

## Limits / caveats

- **The bound is for nearest-neighbour or short-range interactions only.** Long-range interactions (power-law decay) can violate the effective light cone — see later work by Hastings-Koma, Foss-Feig et al. on tightened Lieb-Robinson bounds for long-range systems.
- **The Lieb-Robinson velocity $v$ is a worst-case bound.** Actual information propagation in specific models is often much slower. The [[Commutator-Sensitive Lieb-Robinson Bound]] of Haah-Hastings-Kothari-Low (2021) gives tighter bounds when Hamiltonian terms nearly commute.
- **TQO result uses a "colloquial" definition.** The formal TQO definition involves error parameters $(l, \varepsilon)$; the colloquial version requires $l \sim L$ and $\varepsilon \sim e^{-l}$. More refined definitions may give tighter circuit depth bounds.
- **The entangling rate constant $c^* \approx 1.9$ is for real-time evolution only.** Imaginary-time evolution (used in thermal state preparation) can have unbounded entangling capacity.
- **No implications for non-local Hamiltonians.** Quantum algorithms using non-local operations (e.g., LCU with global oracles) are not constrained by these bounds.

## Reusable ideas

1. **[[Light-Cone Truncation for Operator Approximation]]** — Any local operator evolved under a local Hamiltonian can be approximated by an operator supported only within the light cone, with exponentially small error in the truncation distance. This is the workhorse behind the [[Lieb-Robinson Decomposition for Lattice Simulation]].

2. **[[Entangling Rate Bound for Bipartite Hamiltonians]]** — The maximum rate of entanglement generation by a product Hamiltonian $J_A \otimes J_B$ is bounded by $c^* \approx 1.9$, independent of Hilbert space dimensions. This gives area-law preservation under finite-time evolution.

## References within this paper

- Lieb, Robinson (1972) — the original Lieb-Robinson bound
- Hastings (2004) — generalised LR bound, area law connection
- Nachtergaele, Sims (2005) — further LR generalisations
- Childs, Leung, Verstraete, Vidal (2003) — entangling capacity of bipartite Hamiltonians (ref [14])
- Childs, Leung, Vidal (2004) — improved entangling capacity bounds (ref [15])
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes|Kitaev (2003)]] — toric code and topological quantum order (ref [9])
- Dennis, Kitaev, Landahl, Preskill (2002) — toric code state preparation procedure shown to be optimal (ref [10])
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal, Latorre, Rico, Kitaev (2003)]] — entanglement in ground states (ref [17])
- Hastings, Wen (2005) — quasi-adiabatic continuation between phases (ref [18])

## Cross-links

### Paper notes
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]] — uses Lieb-Robinson bounds to achieve optimal $O(nT \text{polylog})$ simulation; the [[Lieb-Robinson Decomposition for Lattice Simulation]] directly builds on Result 2
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]] — toric code whose preparation time is lower-bounded here
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]] — MPS simulation efficiency explained by the area-law preservation result
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes]] — cluster states have TQO properties; their preparation depth is constrained by Result 3
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes]] — references this paper for the circuit depth lower bound on topological states
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — commutator-based Trotter analysis uses similar locality arguments
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — the original local simulation result that these bounds justify
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry-protected simulation exploits the structure that LR bounds constrain

### Trick cards
- [[Light-Cone Truncation for Operator Approximation]]
- [[Entangling Rate Bound for Bipartite Hamiltonians]]
- [[Lieb-Robinson Decomposition for Lattice Simulation]]
- [[Commutator-Sensitive Lieb-Robinson Bound]]
- [[Circuit-Size Hierarchy for Gate-Complexity Lower Bounds]]
