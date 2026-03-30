> **Source:** Dave Wecker, Matthew B. Hastings, Nathan Wiebe, Bryan K. Clark, Chetan Nayak, Matthias Troyer, *Solving strongly correlated electron models on a quantum computer*, arXiv:1506.05135, Phys. Rev. A **92**, 062318 (2015)
> **Links:** [arXiv](https://arxiv.org/abs/1506.05135) · [Journal](https://doi.org/10.1103/PhysRevA.92.062318)
> **Tags:** #quantum-simulation #Hubbard #VQE #adiabatic-state-preparation #phase-estimation #Trotter #correlation-functions #condensed-matter #NISQ

---

## The computational problem

Determine the ground-state phase diagram and physical properties of the 2D square-lattice Hubbard model:

$$H_{\text{Hub}} = -\sum_{\langle i,j \rangle} \sum_\sigma t_{ij}\left(c^\dagger_{i,\sigma} c_{j,\sigma} + c^\dagger_{j,\sigma} c_{i,\sigma}\right) + U \sum_i n_{i,\uparrow} n_{i,\downarrow} + \sum_i \epsilon_i n_i$$

on a quantum computer. The central physics question: does the Hubbard model have a $d_{x^2-y^2}$ superconducting ground state at dopings relevant to cuprate superconductors ($0.05 \lesssim x \lesssim 0.25$, $U/t \sim 8$)? And if so, what are the gap, superfluid density, and correlation functions?

This is a complete algorithmic pipeline paper: state preparation → [[Iterative Phase Estimation (Kitaev)|phase estimation]] → measurement of observables, with explicit circuits and resource estimates for the Hubbard model on lattices up to $20 \times 20$ sites.

---

## What the paper does

Presents an end-to-end quantum simulation strategy for the Hubbard model, covering every step from initial state preparation through ground-state projection to measurement of static and dynamic correlation functions. The key contributions:

1. **Adiabatic state preparation** from mean-field starting states (BCS superconductor, antiferromagnet, RVB plaquette states), with a phase-diagram-by-elimination strategy: if the gap closes during the adiabatic path, you guessed the wrong phase.

2. **Efficient Slater determinant preparation** via [[Givens Rotation Slater Determinant Preparation|Givens rotations]], improving over prior methods (Ortiz et al. 2001). Cost: $O(N_e N)$ for $N_e$ electrons in $N$ orbitals, or $O(N^2 \log N)$ for translationally invariant systems using FFT-like decomposition.

3. **Optimized Trotter circuits** for the Hubbard model with snake-pattern Jordan-Wigner ordering: $O(N)$ gates and $O(\log N)$ circuit depth per Trotter step, exploiting nesting and JW string cancellation.

4. **Improved phase estimation** that halves the number of Trotter steps and quarters the rotation gate count by using uniformly controlled rotations (forward/backward evolution conditioned on ancilla).

5. **Measurement protocols** for local observables, Green's functions, pair correlations, and dynamic structure factors — including a frequency-domain importance sampling method via QPE and non-destructive measurement strategies with quadratic speedup.

6. **Boundary cancellation for adiabatic paths** — smooth annealing schedules that suppress diabatic errors as $O(1/T^{m+1})$ instead of $O(1/T)$, achieving subpolynomial total cost.

The bottom line: the Hubbard model on a $20 \times 20$ lattice (800 spin-orbitals) is simulable with $\sim 2000$ qubits and a parallel circuit depth of $\sim 10^6$ gates, reachable at $\sim 1\,\mu$s logical gate times in $\sim 1$ second. This was a bold but concrete estimate for 2015.

---

## The algorithm / construction

### Phase 1: Adiabatic state preparation

The strategy is to test candidate phases by adiabatic connection. For each candidate (d-wave superconductor, antiferromagnet, striped order, etc.):

1. Prepare the ground state of a simple mean-field Hamiltonian as a Slater determinant
2. Adiabatically interpolate to the target Hubbard Hamiltonian
3. If the gap stays open throughout, the candidate phase is correct
4. If the gap closes (detected via QPE), the candidate is wrong — try another

This is a **phase-diagram-by-elimination** protocol. The key insight: you don't need to know the answer in advance. You enumerate candidate phases and let the gap tell you which one is right.

**Mean-field starting states:**

For $d$-wave superconductivity, start from the BCS Hamiltonian:

$$H^{\text{MF}}_{\text{DSC}} = -\sum_{\langle i,j \rangle} \sum_\sigma t_{ij}\left(c^\dagger_{i,\sigma} c_{j,\sigma} + \text{h.c.}\right) - \sum_{\langle i,j \rangle} \Delta^{x^2-y^2}_{ij}\left(c^\dagger_{i\uparrow} c^\dagger_{j\downarrow} - c^\dagger_{i\downarrow} c^\dagger_{j\uparrow}\right) + \text{h.c.}$$

This is reduced to a Slater determinant via a staggered particle-hole transformation on down-spins. The adiabatic path includes a weak symmetry-breaking field to gap out Goldstone modes and nodal fermions — the field is small enough not to bias the physics, but large enough to keep the gap open if the system is indeed superconducting.

For antiferromagnetic order, start from a staggered-field Hamiltonian. The logic is symmetric.

**RVB plaquette preparation:** An alternative route builds up the state from 4-site plaquettes. Prepare each plaquette ground state (2 or 4 electrons) by adiabatic evolution from a product state ($T_1 \approx T_2 \approx 10/t$), then couple plaquettes adiabatically ($T_3 \approx 50/t$). Must break reflection symmetry explicitly via chemical potential bias to avoid getting stuck in excited states.

### Phase 2: Slater determinant preparation via Givens rotations

Any Slater determinant $|\Psi_{\text{SD}}(\rho)\rangle = \prod_{j=1}^{N_e} b^\dagger_j |0\rangle$ can be prepared by:

1. Initialize the trivial Slater determinant: electrons on the first $N_e$ sites
2. Apply a sequence of at most $N_e N$ [[Givens Rotation Slater Determinant Preparation|Givens rotations]] $R_{i,j}(\theta) = \exp(\theta c^\dagger_i c_j - \text{h.c.})$

Each Givens rotation is a two-qubit gate (up to JW strings). For translationally invariant systems with $k$-site unit cells, an FFT-like decomposition reduces the count to $O(N \log N)$ rotations. The JW strings cancel under the snake ordering, giving total cost $O(N_e N)$.

This improves over Ortiz et al.'s method, which required implementing $\exp(i\frac{\pi}{2}(b_m + b_m^\dagger))$ without an efficient circuit decomposition.

### Phase 3: Trotter evolution circuits

The 2D Hubbard Hamiltonian decomposes into 5 non-commuting groups: on-site repulsion + 4 sets of hopping terms (horizontal/vertical, even/odd). With the snake-pattern JW ordering:

- **Repulsion terms** ($n_{i,\uparrow} n_{i,\downarrow}$): all commute, applied in parallel. $O(N)$ gates, $O(1)$ depth.
- **Horizontal hopping**: neighbouring sites in the snake, no JW strings needed. All commute within a set. $O(N)$ gates, $O(1)$ depth.
- **Vertical hopping**: requires JW strings of length $O(\sqrt{N})$. Terms nest (Hastings et al. 2015), so a full set takes $O(N)$ gates and $O(\sqrt{N})$ depth. With tree parity computation, depth reduces to $O(\log N)$.

**Total per Trotter step:** $O(N)$ gates, $O(\log N)$ parallel depth.

Trotter step sizes: $\delta t = 0.25/t$ suffices for adiabatic preparation (tolerating some error, refined later by QPE). For phase estimation, $\delta t = 0.05/t$ gives relative errors $< 10^{-3}$; $\delta t = 0.003/t$ gives $< 10^{-5}$.

### Phase 4: Improved phase estimation

A simple but effective optimization: instead of implementing controlled-$e^{-iHt}$ (identity if ancilla $|0\rangle$, forward evolution if $|1\rangle$), implement $e^{+iHt/2}$ if ancilla $|0\rangle$ and $e^{-iHt/2}$ if $|1\rangle$. The phase difference is the same ($e^{-iHt}$), but:

- Number of Trotter steps halved (simulating $t/2$ instead of $t$)
- Each controlled rotation replaced by a **uniformly controlled rotation** (3 gates instead of 4)
- Net result: $> 2\times$ depth reduction, $4\times$ fewer arbitrary rotations

### Phase 5: Measurement protocols

**Local observables** (density, double occupancy, spin correlations): measured directly in the computational basis. All commute, so one measurement set suffices.

**Green's functions** ($\langle c^\dagger_{i,\sigma} c_{j,\sigma} \rangle$): two strategies:
- *Stabilizer strategy:* replace rotations in the evolution circuit with measurements
- *FSWAP strategy:* fermionic swap gates move sites to adjacent positions, then a basis-change circuit enables direct measurement. The FSWAP gate is a standard swap with a $-1$ phase on $|11\rangle$.

**Pair correlations** ($\Delta_{ij,kl}$): decompose into products of one-body terms measured via FSWAP.

**Dynamic correlations** ($C_{A,B}(t) = \langle \psi_0 | A^\dagger(t) B(0) | \psi_0 \rangle$):
- *Time domain:* controlled Hadamard test with the optimization of Section V.A
- *Frequency domain (new):* apply QPE to the state $A|\psi_0\rangle$; eigenstate $|\psi_n\rangle$ is sampled with weight $|\langle \psi_n | A | \psi_0 \rangle|^2$, directly giving the spectral function $S_A(\omega) = \sum_n |\langle \psi_n | A | \psi_0 \rangle|^2 \delta(\omega - (E_n - E_0))$

**Non-destructive measurements:**
- *Hellmann-Feynman:* perturb $H \to H + \lambda O$, measure energy shift. With $k$-th order finite differences: cost $O(k \epsilon^{-1-1/(k-1)})$, approaching $O(\epsilon^{-1} \log(1/\epsilon))$.
- *Recovery map + quadratic speedup:* measure projector $Q$, then recover ground state via $P_0$ measurement. Using amplitude estimation on $UV = (2Q-1)(2P_0-1)$: quadratic speedup from $O(1/\epsilon^2)$ to $O(1/\epsilon)$.

### Phase 6: Boundary cancellation for adiabatic paths

Smooth annealing schedules with $\partial_s^k f(s)|_{s=0,1} = 0$ for $k = 1, \ldots, m$ suppress diabatic errors from $O(1/T)$ to $O(1/T^{m+1})$. The schedule:

$$f(s) = \frac{\int_0^s y^m(1-y)^m\, dy}{\int_0^1 y^m(1-y)^m\, dy}$$

With optimal $m = O(\log(1/\delta))$ (for analytic Hamiltonians), the total cost of preparing the ground state within error $\delta$ is subpolynomial in $1/\delta$.

---

## Key results

**Hubbard simulation cost (Table-free summary):**

- **Qubits:** $2N$ ($N$ sites, 2 spin species)
- **Gates per Trotter step:** $O(N)$
- **Circuit depth per Trotter step:** $O(\log N)$
- **Trotter steps for adiabatic prep (1000-site chain):** $\sim 4000$ at $\delta t = 0.25/t$
- **Trotter steps for QPE:** $\sim 2000$ at $\delta t = 0.05/t$ for $10^{-3}$ relative error
- **Total parallel circuit depth:** $\sim 10^6$ gates for $20 \times 20$ lattice
- **Wall time at $1\,\mu$s logical gates:** $\sim 1$ second for state preparation

**Adiabatic prep success probabilities (8-site Hubbard, Fig. 1):**
- $U = 2.5t$: $> 90\%$ overlap at annealing time $T \sim 30/t$
- $U = 4t$: $> 80\%$ overlap at $T \sim 50/t$

**Plaquette coupling (8 sites, Fig. 4):**
- $T_1 = T_2 \approx 10/t$ for plaquette prep, $T_3 \approx 50/t$ for coupling
- Success probability $> 95\%$ at these timescales

**Boundary cancellation (Theorem, Appendix):**
- Diabatic error: $O\!\left(\frac{\max_s \|\partial_s^{m+1} H(f(s))\|}{\Delta(s)^{m+2} T^{m+1}}\right)$
- With Trotter, total cost is $O(\delta^{-2/m})$ for $2k = m$, which is subpolynomial for growing $m$

**Trotter error bound for time-dependent case (Appendix A, Eq. A2):**

$$\left\|\mathcal{T}e^{-i\int_0^1 H(u)\,du} - \prod_{j=1}^m e^{-iH_j(1/2)/2} \prod_{j=m}^1 e^{-iH_j(1/2)/2}\right\| \leq \frac{1}{24}\max_s \|H''(s)\| + \frac{1}{12}\|[H'(1/2), H(1/2)]\| + \text{Trotter commutator terms}$$

This extends the Wecker et al. 2014 bounds to time-dependent Hamiltonians, correcting a typographical error in the original.

---

## Comparison with prior work

| Paper | Focus | State prep | Circuits | Measurements | Non-destructive |
|---|---|---|---|---|---|
| Ortiz et al. (2001) | General lattice models | Slater via $e^{i\pi/2(b+b^\dagger)}$ | Yes but not optimized | Basic | No |
| Somma et al. (2002) | Hubbard observables | — | — | General strategy | No |
| Abrams & Lloyd (1997) | Hubbard concept | — | Not explicit | — | No |
| [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes|Wecker-Hastings-Troyer (2015)]] | VQE resources | Adiabatic ansatz | — | Scaling analysis | No |
| **This paper** | **Complete pipeline** | **Givens + adiabatic + plaquettes** | **Full circuits, $O(\log N)$ depth** | **Static + dynamic + frequency** | **Yes, quadratic speedup** |

This paper and [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes|Wecker-Hastings-Troyer (2015)]] are companion papers from the same group and era (both published in PRA 92, 2015). The companion focuses on VQE resource bottlenecks (measurement scaling) and the adiabatic ansatz as a variational circuit. This paper focuses on the fault-tolerant pipeline: adiabatic state prep + QPE + correlation function measurement. Together they bracket the near-term (VQE) and fault-tolerant (QPE) approaches to the Hubbard model.

---

## Limits / caveats

1. **Adiabatic gap assumption.** The entire phase-diagram strategy assumes the spectral gap remains $\Omega(1/\text{poly}(N))$ along the adiabatic path when the correct phase is guessed. For first-order transitions or exotic phases, this can fail. The paper acknowledges this but doesn't resolve it.

2. **Small numerical benchmarks.** The explicit simulations are 8 sites (exact diag feasible). The $20 \times 20$ estimates are extrapolations of gate counts, not simulations. The gap behaviour at 800 sites could be qualitatively different.

3. **No noise analysis.** The entire treatment assumes fault-tolerant operation. No discussion of how errors propagate through the adiabatic path or affect correlation function measurements on noisy hardware.

4. **Adiabatic time for 2D lattices.** The free-fermion annealing benchmarks (1D chain) show $T \sim 1000/t$ for $\sim 1000$ sites at $\sim 50\%$ success. The 2D interacting case could be much worse, and the paper is explicit about this uncertainty.

5. **Symmetry-breaking fields are system-dependent.** Choosing the right magnitude for the pinning fields ($w$, $u$) requires physical intuition about the expected gap scales. Too large biases the physics; too small doesn't prevent Goldstone-mode issues.

6. **FSWAP strategy limited to local observables.** Measuring non-local correlation functions requires $O(N)$ FSWAP gates to bring sites together, adding depth. For long-range correlations in 2D, this cost is non-trivial.

---

## Reusable ideas

1. [[Phase-Diagram-by-Elimination via Adiabatic Gap Detection]] — test candidate phases by adiabatic evolution from their mean-field representatives; gap closure signals the wrong phase, gap preservation confirms it. Applicable beyond Hubbard to any system where candidate phases have known mean-field descriptions.

2. [[Uniformly Controlled Rotation for Phase Estimation]] — replace controlled rotations (4 gates) with uniformly controlled rotations (3 gates) by implementing forward/backward evolution conditioned on ancilla state. Halves Trotter steps, quarters rotation count. Works for any Hamiltonian simulation inside QPE.

3. [[Frequency-Domain Spectral Function via QPE Importance Sampling]] — measure dynamic structure factors by applying QPE to $A|\psi_0\rangle$ instead of Fourier-transforming time-domain correlation data. Each QPE sample directly draws from $S_A(\omega)$ with the correct spectral weights.

4. [[Non-Destructive Ground-State Measurement via Recovery Map]] — after measuring a projector $Q$ on the ground state, recover $|\psi_0\rangle$ by alternating $Q$ and $P_0$ measurements. Combined with amplitude estimation on $UV = (2Q-1)(2P_0-1)$, gives $O(1/\epsilon)$ scaling instead of $O(1/\epsilon^2)$.

5. [[Boundary Cancellation Schedule for Adiabatic State Preparation]] — smooth the annealing schedule so $\partial^k_s f(s)$ vanishes at endpoints, suppressing diabatic transitions to $O(1/T^{m+1})$. With growing $m$, total simulation cost becomes subpolynomial in $1/\delta$.

---

## References within this paper

**Papers with notes in the vault:**
- [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes|Wecker, Hastings, Troyer (2015)]] — companion paper on VQE resource analysis for Hubbard; same group, same era
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush, McClean, Wecker, Aspuru-Guzik, Wiebe (2015)]] — Trotter error analysis for chemistry; Wecker is co-author on both. This paper's Appendix A extends those bounds to the time-dependent case
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo, McClean et al. (2014)]] — VQE original; this paper is the fault-tolerant counterpart
- [[Givens Rotation Slater Determinant Preparation]] — trick card for the Slater determinant prep method introduced here and refined in later work
- [[Adiabatic State Preparation for Chemistry]] — existing trick card; this paper provides the most detailed treatment of adiabatic prep for lattice models
- [[Iterative Phase Estimation (Kitaev)]] — QPE framework used throughout

**Papers not yet in vault:**
- Ortiz, Gubernatis, Knill, Laflamme (2001), PRA 64:022319 — prior Slater determinant prep and lattice simulation; this paper improves on their methods
- Somma, Ortiz, Gubernatis, Knill, Laflamme (2002), PRA 65:042323 — detailed measurement strategies for lattice models; this paper's Sec. VI builds on and extends their work
- Hastings, Wecker, Bauer, Troyer (2015) — nesting and JW cancellation techniques for Trotter circuits; directly used in Sec. IV
- Trebst, Schollwöck, Troyer, Zoller (2006) — RVB plaquette preparation in optical lattices; adapted here for digital quantum simulation
- Temme, Osborne, Vollbrecht, Poulin, Verstraete (2011) — recovery-map idea; this paper's Sec. VII.B extends it
- Lidar, Rezakhani, Hamma (2009) — boundary cancellation theory; used in Sec. V.C
- Wiebe, Babcock (2012) — error scaling for smooth annealing paths; provides the $O(1/T^{m+1})$ bound used here

---

## Cross-links

### Related paper notes
- [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes]] — companion paper; VQE perspective on the same physics
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — Trotter error analysis by overlapping author group
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]] — FeMoCo resource estimation; Wecker and Troyer co-authors; builds on the QPE pipeline developed here
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — refines the Givens rotation and swap network ideas
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — optimized Trotter for Hubbard with surface-code compilation; the natural successor to this paper's Trotter circuits
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]] — later analysis of variational optimization difficulty; relevant to the VQE aspects
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — rigorous commutator-based bounds that supersede the Appendix A analysis here
- [[Observation of Separated Dynamics of Charge and Spin in the Fermi-Hubbard Model (Arute et al. 2020) — Paper Notes]] — experimental NISQ Hubbard dynamics; this paper provides the fault-tolerant vision that experiment is trying to reach
- [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes]] — hardware-efficient alternative to the physics-motivated ansätze used here
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — formalises the "anneal with large Trotter step" idea from Sec. III.C into a rigorous discrete adiabatic theorem; boundary cancellation gives super-polynomial convergence

### Related trick cards
- [[Givens Rotation Slater Determinant Preparation]]
- [[Adiabatic State Preparation for Chemistry]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Fermionic Swap Network]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[Alternating Operator Ansatz]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Phase-Diagram-by-Elimination via Adiabatic Gap Detection]] — new trick from this paper
- [[Uniformly Controlled Rotation for Phase Estimation]] — new trick from this paper
- [[Frequency-Domain Spectral Function via QPE Importance Sampling]] — new trick from this paper
- [[Non-Destructive Ground-State Measurement via Recovery Map]] — new trick from this paper
- [[Boundary Cancellation Schedule for Adiabatic State Preparation]] — new trick from this paper

---

## Assessment

This is a thorough, well-executed infrastructure paper. It doesn't introduce a single headline result — it takes the existing toolbox (Trotter simulation, QPE, Givens rotations, adiabatic state prep) and assembles it into a complete, concrete pipeline for the Hubbard model with careful attention to every step.

The phase-diagram-by-elimination idea is the most original conceptual contribution: rather than trying to find the ground state directly, you enumerate candidate phases and let the quantum computer tell you which one is right via gap detection. This sidesteps the QMA-hardness of the general ground state problem by assuming physical systems have mean-field-describable phases — a reasonable assumption that matches experimental reality for most condensed matter systems.

The technical contributions are individually modest but collectively significant: the Givens-rotation Slater determinant prep (simpler and more efficient than Ortiz et al.), the uniformly controlled rotation trick for QPE ($4\times$ fewer rotations), the frequency-domain spectral function measurement, and the boundary-cancellation adiabatic schedules. Several of these became standard tools in later work.

The resource estimates ($\sim 2000$ qubits, $\sim 10^6$ depth for $20 \times 20$ Hubbard) were optimistic for the era but directionally correct. The $O(\log N)$ depth per Trotter step for the Hubbard model — compared to $O(N^4)$-term molecular Hamiltonians — is exactly why lattice models remain the most attractive targets for quantum simulation. [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan et al. (2020)]] later showed that the $O(1)$ T-count per Trotter step (with extensive error framing) makes classically intractable Hubbard instances feasible on $\sim 300$K physical qubits — a refinement of the vision laid out here.

The paper is a natural companion to [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes|Wecker-Hastings-Troyer (2015)]]: that paper asks "what can VQE do on near-term hardware?" (answer: Hubbard yes, chemistry no), while this one asks "what does the full fault-tolerant pipeline look like?" (answer: surprisingly tractable for Hubbard). Together they bracket the two regimes that quantum simulation research has been exploring ever since.
