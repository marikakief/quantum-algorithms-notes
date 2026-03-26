> **Source:** Ryan Babbush, Dominic W. Berry, Robin Kothari, Rolando D. Somma, and Nathan Wiebe, *Exponential quantum speedup in simulating coupled classical oscillators*, arXiv:2303.13012, Phys. Rev. X **13**, 041041 (2023)
> **Links:** [arXiv](https://arxiv.org/abs/2303.13012) · [Phys. Rev. X](https://journals.aps.org/prx/abstract/10.1103/PhysRevX.13.041041)
> **Tags:** #Hamiltonian-simulation #BQP-completeness #classical-to-quantum-reduction #exponential-speedup #coupled-oscillators #wave-equation #block-encoding #harmonic-approximation

---

## The computational problem

Simulate the dynamics of $N = 2^n$ coupled classical harmonic oscillators — point masses connected by springs, governed by Newton's equation:

$$M \ddot{\vec{x}}(t) = -F \vec{x}(t)$$

where $M$ is the diagonal mass matrix and $F$ is the force matrix (discrete Laplacian of a weighted graph). The output is a quantum state encoding the momenta and displacements:

$$|\psi(t)\rangle = \frac{1}{\sqrt{2E}} \begin{pmatrix} \sqrt{M}\,\dot{\vec{x}}(t) \\ i\,\vec{\mu}(t) \end{pmatrix}$$

where $E$ is the (conserved) total energy and $\vec{\mu}(t)$ collects terms $\sqrt{\kappa_{jk}}(x_j(t) - x_k(t))$ and $\sqrt{\kappa_{jj}} x_j(t)$.

This encoding is the key design choice. It balances kinetic and potential energy terms equally in the quantum state amplitudes, making both accessible via simple measurements. Other encodings (storing $\vec{y}(t)$ or $A\vec{y}(t)$ directly) introduce condition-number dependence that kills the speedup.

**Problem 2** (the main application): Estimate the fraction of total energy stored as kinetic energy in a subset $V \subseteq [N]$ of oscillators at time $t$, to additive error $\varepsilon$.

---

## What the paper does

Maps Newton's equations for $2^n$ coupled oscillators to a Schrödinger equation, then simulates the resulting Hamiltonian in $\text{poly}(n)$ time. The mapping works because Newton's equation $\ddot{\vec{y}} = -A\vec{y}$ factors into a first-order equation via the "square root" trick: define a block Hamiltonian $H$ such that $H^2$ has $A$ in its top-left block, and the quantum evolution under $H$ directly propagates the classical positions and momenta.

Three results:

1. **Algorithm (Theorem 1):** Simulate the oscillator dynamics with $O(\tau + \log(1/\varepsilon))$ oracle queries, where $\tau = t\sqrt{\kappa_{\max} d / m_{\min}}$ and $d$ is the sparsity of the spring constant matrix.

2. **Classical lower bound (Theorem 2):** Any classical algorithm requires $2^{\Omega(n)}$ queries — proved via reduction from the glued trees problem.

3. **BQP-completeness (Theorem 3):** The kinetic energy estimation problem is BQP-complete when oracles are replaced by efficient circuits. So coupled oscillator simulation is *exactly* as hard as arbitrary quantum computation.

This is one of the cleanest exponential quantum speedups known for a problem with natural physical motivation. The classical system is not contrived — mass-spring networks, electrical circuits, molecular vibrations, and structural mechanics all produce coupled oscillator dynamics.

---

## The algorithm / construction

### Step 1: Classical-to-quantum reduction

Change variables $\vec{y}(t) = \sqrt{M}\,\vec{x}(t)$, giving $\ddot{\vec{y}} = -A\vec{y}$ where $A = \sqrt{M}^{-1} F \sqrt{M}^{-1} \succeq 0$.

Observe that $\ddot{\vec{y}} + i\sqrt{A}\,\dot{\vec{y}} = i\sqrt{A}(\dot{\vec{y}} + i\sqrt{A}\,\vec{y})$, which is Schrödinger's equation with Hamiltonian $-\sqrt{A}$. But simulating $\sqrt{A}$ directly requires [[Qubitization (Quantum Walk for Spectral Encoding)|phase estimation]], which is expensive.

Instead, factor $A = BB^\dagger$ using an incidence-matrix construction. Define $B$ via the weighted graph incidence matrix:

$$\sqrt{M}\,B|j,k\rangle = \begin{cases} \sqrt{\kappa_{jj}}\,|j\rangle & j = k \\ \sqrt{\kappa_{jk}}(|j\rangle - |k\rangle) & j < k \end{cases}$$

Then the block Hamiltonian

$$H = -\begin{pmatrix} 0 & B \\ B^\dagger & 0 \end{pmatrix}$$

satisfies $H^2|_{\text{top block}} = A$, so $H$ acts as a "square root" of $A$ without computing $\sqrt{A}$ explicitly. The quantum state

$$|\psi(t)\rangle \propto \begin{pmatrix} \dot{\vec{y}}(t) \\ iB^\dagger \vec{y}(t) \end{pmatrix} = e^{-itH} \begin{pmatrix} \dot{\vec{y}}(0) \\ iB^\dagger \vec{y}(0) \end{pmatrix}$$

evolves under Schrödinger's equation with $H$.

### Step 2: Block encoding of $H$

The sparse oracle $S$ provides the positions and values of nonzero entries in $K$ (spring constants) and $M$ (masses). From this, block-encode $B$ using inequality testing for state preparation (following Sanders et al.'s approach from [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]]):

- Prepare $\frac{1}{\sqrt{d}} \sum_{\ell=1}^d |\ell\rangle$ (sparse state)
- Query oracle for nonzero positions $a(j,\ell)$
- Compute $\sqrt{\kappa_{jk}/m_j}$ via inequality testing with $O(\log(1/\varepsilon'))$ ancilla bits
- Conditional swap + phase for the $|j\rangle|k\rangle \mapsto |k\rangle|j\rangle$ asymmetry

The block encoding satisfies $\langle 0| U_B |0\rangle \approx B/\sqrt{2\aleph d}$, where $\aleph = \kappa_{\max}/m_{\min}$. The normalization $\Lambda = \sqrt{2\aleph d}$ is the block encoding factor.

The $\sqrt{d}$ factor (not the usual $d$) in $\Lambda$ comes from a single sparse-state preparation — standard Hamiltonian simulation uses matched preparation/inverse-preparation (each contributing $\sqrt{d}$) for an overall $d$ factor. Here only one preparation is needed because $B$ is already the "half" of $A$.

### Step 3: Hamiltonian simulation

Simulate $e^{-itH}$ using [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]/QSP (Low & Chuang) or the [[Truncated Taylor Series Simulation|truncated Taylor series]] approach. Query complexity:

$$Q = O(\tau + \log(1/\varepsilon)), \quad \tau = t\sqrt{\aleph d}$$

Gate complexity:

$$G = O\!\left(Q \cdot \log^2\!\frac{N\tau}{\varepsilon} \cdot \frac{m_{\max}}{m_{\min}}\right)$$

### Step 4: Energy estimation

The kinetic energy fraction $K_V(t)/E = \langle\psi(t)|P_V|\psi(t)\rangle$ where $P_V$ projects onto the first $N$ basis states restricted to subset $V$. Estimate via [[Iterative Phase Estimation (Kitaev)|amplitude estimation]]: $O(\log(1/\delta)/\varepsilon)$ uses of the simulation circuit, giving Heisenberg-limited scaling.

---

## Key results

**Theorem 1 (Quantum algorithm):** Problem 1 can be solved with $O(\tau + \log(1/\varepsilon))$ queries and $\tilde{O}(Q \cdot \text{polylog}(N/\varepsilon))$ gates, using $W$ (initial state oracle) once.

**Theorem 2 (Classical lower bound):** Any classical algorithm for Problem 2 requires $2^{\Omega(n)}$ queries to $K$. Holds even with $m_j = 1$, $\kappa_{jk} \in \{0,1\}$, $d \leq 3$.

**Theorem 3 (BQP-completeness):** Problem 3 is BQP-complete. Holds even with $\kappa_{jk} \leq 4$, $m_j = 1$, $d \leq 4$, and initial state $|\psi(0)\rangle = |0\rangle^{\otimes q} \otimes |-\rangle$.

**Theorem 4 (Generalized coordinates):** For the more general $\ddot{\vec{y}} = -A\vec{y}$ with PSD $A$, query complexity is:

$$O\!\left(\|A\|_{\max} d \log(1/\varepsilon) \min\!\left(\frac{t\sqrt{\|A^{-1}\|}}{\varepsilon},\, \frac{t^2}{\varepsilon^2}\right)\right)$$

**Theorem 5 (Kinetic energy estimation):** $O(\log(1/\delta)/\varepsilon)$ uses of the simulation circuit suffice for additive-$\varepsilon$ estimates of $K_V(t)/E$.

---

## Comparison with prior work

| Aspect | This paper | Costa, Jordan, Ostrander (2019) — wave equation | QLSP-based approaches (Berry 2014, Childs-Liu-Ostrander 2021, Krovi 2023) |
|---|---|---|---|
| Speedup | Exponential | Polynomial | Depends on encoding |
| Encoding | Energy-balanced ($\dot{\vec{y}}$, $B^\dagger \vec{y}$) | Position-based ($\vec{y}$, $B^+ \dot{\vec{y}}$) | $(\dot{\vec{y}}, \vec{y})$ or $(\dot{\vec{y}}, A\vec{y})$ |
| Condition number dep. | None | $O(\text{poly}(\kappa))$ in state prep | $O(\text{poly}(\kappa))$ |
| Applicable systems | General sparse coupling | Uniform, geometrically local | General (but slow) |
| BQP-completeness | Yes | No | No |

The energy-balanced encoding is the critical innovation. The Costa et al. wave equation algorithm uses an encoding where initial state preparation costs $O(\text{poly}(N))$ due to needing the pseudo-inverse $B^+$, limiting it to polynomial speedups. Standard ODE-to-QLSP reductions via Eq. (G1)–(G4) in the paper produce non-unitary dynamics whose simulation inherits condition number dependence that becomes unbounded for low-frequency modes.

---

## Limits / caveats

1. **Exponential speedup only for global/aggregate properties.** Extracting the full position vector $\vec{x}(t)$ takes $O(N)$ time — no speedup. The advantage is for quantities like total kinetic energy, potential energy of a spring subset, or similar projective measurements.

2. **Efficient oracle assumption.** Masses and spring constants must be queryable in $\text{polylog}(N)$ time. This holds for structured networks (lattices, hierarchical graphs, circuits) but not for arbitrary dense systems.

3. **Time must be short.** Complexity is linear in $\tau \propto t$, so $t$ must be $\text{polylog}(N)$ for exponential advantage. For geometrically local systems, the "light cone" grows as $N \sim t^D$ in $D$ spatial dimensions, making $t = \text{poly}(N)$ and erasing exponential advantage (though super-quadratic speedups remain).

4. **BQP-completeness instances are not physically natural.** The Feynman-Kitaev circuit-to-oscillator construction gives spring constants encoding universal quantum circuits. The glued trees instance is similarly artificial. Whether "natural" (physically occurring) oscillator systems are classically hard remains open.

5. **Harmonic approximation only.** Anharmonic potentials are not covered. The mapping to Schrödinger's equation relies on the quadratic structure of the potential.

6. **Initial state preparation.** The algorithm assumes oracle access to $W$ preparing $|\psi(0)\rangle$. The paper shows this can be done with $O(\sqrt{E_{\max}d/E})$ queries via amplitude amplification when position/velocity states are separately preparable, but this can be expensive when masses or spring constants vary widely.

---

## Reusable ideas

1. [[Incidence Matrix Factorization for Square-Root Block Encoding]] — Factor a graph Laplacian $F = BB^\dagger$ via the weighted incidence matrix to block-encode $\sqrt{A}$ without computing the matrix square root.

2. [[Energy-Balanced Encoding for Classical-to-Quantum Reduction]] — Encode classical positions and momenta in quantum amplitudes weighted by $\sqrt{m_j}$ and $\sqrt{\kappa_{jk}}$ so that the norm is the conserved total energy, yielding unitary evolution.

3. [[Square-Root Sparsity in Block Encoding]] — When block-encoding $B$ (a factor of $A = BB^\dagger$), only one sparse-state preparation is needed, giving $\Lambda = O(\sqrt{d})$ instead of the usual $O(d)$ for direct block encoding of $A$.

4. [[Feynman-Kitaev Construction with Non-Negative Entries]] — Replace Hadamard gates with a non-negative real-symmetric matrix that acts as Hadamard on the $|-\rangle$ subspace, enabling circuit-to-Hamiltonian reduction where all off-diagonal entries of $A$ are non-positive (i.e., valid spring constants).

5. [[Glued Trees as Coupled Oscillators]] — Map the glued trees graph to a mass-spring network and prove that classical dynamics propagates a disturbance from ENTRANCE to EXIT in $O(\text{poly}(n))$ time with $\Omega(1/\text{poly}(n))$ kinetic energy, reducing the oracle separation problem to oscillator simulation.

---

## References within this paper

### Papers with notes in the vault

- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — Inequality testing for state preparation (Lemma 8 relies on this), Kaiser windows for QPE
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — [[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]], [[QROM (Quantum Read-Only Memory)|QROM]], [[Unary Iteration]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa, An, Sanders, Su, Babbush, Berry (2021)]] — Optimal QLSP solver, cited in the comparison with linear-systems-based ODE approaches

### Papers without vault notes

- Childs, Cleve, Deotto, Farhi, Gutmann, Spielman (2003) — Glued trees problem: exponential quantum walk speedup. The classical lower bound in Theorem 2 is inherited from this result.
- Costa, Jordan, Ostrander (2019) — Quantum algorithm for the wave equation via Hamiltonian simulation. A special case of the coupled oscillators problem (uniform masses, local couplings, position-based encoding). Only polynomial speedup.
- Low & Chuang (2017, 2019) — QSP and [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]. The paper uses these for optimal Hamiltonian simulation of $H$.
- Berry, Childs, Cleve, Kothari, Somma (2015) — [[Truncated Taylor Series Simulation]]. Alternative simulation method applicable here.
- Berry, Childs, Kothari (2015) — Nearly optimal Hamiltonian simulation. Another usable simulation backend.
- Grover & Sengupta (2002) — Classical analog of Grover search via coupled pendulums. Presaged the classical-quantum oscillator connection.
- Harrow, Hassidim, Lloyd (2009) — HHL quantum linear systems algorithm. The paper argues that QLSP-based ODE approaches are inferior to direct Hamiltonian simulation for this problem.
- Childs, Liu, Ostrander (2021) — High-precision quantum PDE algorithms. QLSP approach yields worse scaling due to encoding issues.
- Knill, Ortiz, Somma (2007) — High-confidence amplitude estimation, used in Theorem 5.

---

## Cross-links

### Paper notes
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes]] — shorter summary of the same paper (arXiv:2303.13012); this note is the canonical detailed treatment
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes]] — inequality testing for state preparation
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] machinery
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]] — QLSP comparison; this paper argues QLSP-based ODE approaches are suboptimal for oscillator dynamics due to condition-number overhead
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes]] — follow-on to the optimal QLSP solver; constant-factor results affect practical cost of QLSP-based ODE approaches compared here
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — Same author group, same year; that paper addresses quantum chemistry dynamics, this one addresses classical dynamics
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — Real-space Hamiltonian simulation with sparse oracles
- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes]] — Same authors (Kothari, Babbush); quartic (not exponential) speedup via sparse Hamiltonian simulation of the Kikuchi matrix for planted inference problems
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]] — parallel quantum advantage analysis from the same group; honest speedup regime assessment
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — companion paper asking the same question for quantum chemistry; both papers use an "honest speedup assessment" framework

### Trick cards
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — core simulation primitive
- [[Truncated Taylor Series Simulation]] — alternative simulation method
- [[Coherent Alias Sampling for PREPARE]] — used in block encoding construction
- [[Incidence Matrix Factorization for Square-Root Block Encoding]] — new trick from this paper
- [[Energy-Balanced Encoding for Classical-to-Quantum Reduction]] — new trick from this paper
- [[Square-Root Sparsity in Block Encoding]] — new trick from this paper
- [[Feynman-Kitaev Construction with Non-Negative Entries]] — new trick from this paper
- [[Glued Trees as Coupled Oscillators]] — new trick from this paper

---

## Assessment

This is a genuinely significant result. The exponential speedup is clean, the problem class is natural (not a cryptographic or algebraic problem smuggled into a physical wrapper), and the BQP-completeness proof is elegant — it shows that coupled oscillators are as computationally rich as any quantum computation.

The practical impact is harder to pin down. The strongest speedups require non-local coupling topologies and short evolution times relative to system size. For geometrically local systems (the ones engineers actually simulate), the advantage degrades to polynomial. The paper acknowledges this honestly.

The comparison with QLSP-based ODE approaches (Appendix G) is the most technically interesting part. The authors show that naive first-order formulations of the ODE produce encodings where the kinetic or potential energy terms are suppressed, introducing a condition-number bottleneck that doesn't exist with the energy-balanced encoding. This is a lesson that generalises beyond oscillators: when mapping classical dynamics to quantum simulation, the choice of encoding determines whether you get exponential or polynomial speedup.

Worth noting: Marika is thanked in the acknowledgments.
