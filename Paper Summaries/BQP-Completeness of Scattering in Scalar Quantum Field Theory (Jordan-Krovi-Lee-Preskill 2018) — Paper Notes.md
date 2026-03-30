> **Source:** Stephen P. Jordan, Hari Krovi, Keith S. M. Lee, John Preskill, *BQP-completeness of scattering in scalar quantum field theory*, Quantum **2**:44, 2018; arXiv:1703.00454
> **Links:** [arXiv](https://arxiv.org/abs/1703.00454) · [Quantum](https://doi.org/10.22331/q-2018-01-08-44)
> **Tags:** #BQP-complete #quantum-field-theory #scattering #phi-four #computational-complexity #quantum-simulation

---

## The computational problem

**Vacuum-to-vacuum transition amplitude decision problem:** Consider massive $\phi^4$ theory in $(1+1)$ dimensions with spacetime-dependent classical sources:

$$\mathcal{L} = \frac{1}{2}\partial_\mu\phi\,\partial^\mu\phi - \frac{1}{2}m^2\phi^2 - \frac{1}{4!}\lambda\phi^4 - J_2\phi^2 - J_1\phi$$

where $J_1(t,x)$ and $J_2(t,x)$ are externally specified classical fields. The system starts in the vacuum at $t = 0$ with all sources off. Sources are varied according to specified bit strings and return to zero at time $T$.

**Decision version:** Given bit strings specifying $J_1$ and $J_2$, decide whether the probability of remaining in the vacuum at time $T$ is $> 2/3$ or $< 1/3$ (promised one holds).

**Estimation version:** Compute the vacuum-to-vacuum transition probability to precision $\pm\varepsilon$.

## What the paper does

Proves that this problem is **BQP-complete**: it is both solvable by a quantum computer in polynomial time (BQP membership via [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes|Jordan-Lee-Preskill (2012)]]) and BQP-hard (any quantum computation can be encoded into the spacetime dependence of the classical sources).

This is the first BQP-completeness result for a natural quantum field theory problem. Combined with the efficient simulation algorithm of the earlier JLP papers, it shows that classical computers cannot efficiently compute scattering probabilities in $\phi^4$ theory unless BQP = BPP — even when the coupling is arbitrarily weak and the theory is in just one spatial dimension.

## The construction

The BQP-hardness proof designs a quantum computer architecture within $(1+1)$-dimensional $\phi^4$ theory. The three components:

### Qubit encoding

The source $J_2(t,x)$ creates a potential landscape $V(x) = J_2(x)/m$ for the nonrelativistic particles. Each qubit is a **double-well potential** containing one particle. The two logical states are the ground and first excited states of the double well (equivalently, particle localised in left vs right well when wells are sufficiently separated — the splitting is exponentially suppressed in well separation).

The nonrelativistic effective Hamiltonian at tree level is:

$$H_{\mathrm{NR}} = \sum_i \frac{p_i^2}{2m} + \sum_i \frac{J_2(x_i)}{m} + \frac{\lambda}{4m^2}\sum_{i < j}\delta(x_i - x_j) + O(\lambda^2)$$

At $O(\lambda^2)$, an exponentially decaying attractive Yukawa-like potential appears between particles.

### State preparation (from vacuum)

Particles are created in the wells by **adiabatic passage** — the source $J_1(t,x) = f(t)h(x)$ oscillates at a frequency sweeping through the vacuum-to-bound-state resonance:

$$f(t) = g\cos(\omega_0 t + Bt^2/T), \quad -T/2 \leq t \leq T/2$$

Anharmonicity from $\lambda\phi^4$ shifts multi-particle energies away from resonance, suppressing unwanted particle creation. The spatial profile $h(x)$ is chosen to make the matrix element for exciting unbound continuum states vanish.

Parameter scaling for $n$-qubit preparation with error $\varepsilon \sim 1/n$:

$$g \sim \varepsilon^5, \quad \lambda \sim B \sim \varepsilon^4, \quad T \sim 1/\varepsilon^8$$

So state preparation takes time $O(n^8)$ (parallelised across qubits).

### Quantum gates

**Single-qubit gates:** Move the wells of a double-well closer together — tunnelling between wells implements rotations in the qubit subspace.

**Two-qubit gates:** The naive approach (bringing wells close for the inter-particle $\delta$-interaction to generate a controlled phase) fails because tunnelling between wells from different qubits is parametrically larger than the interaction energy when $\lambda$ is small.

**The fix:** Work in the 6-dimensional subspace $S$ of $\binom{4}{2}$ configurations of two particles in four wells without double occupation. By smoothly varying well depths and separations, arbitrary unitaries on $S$ can be implemented, which include entangling gates on the 4D computational subspace. Transitions out of $S$ (double occupancy, unbound particles) are suppressed by adiabaticity — the Gevrey-class adiabatic theorem gives exponential suppression of leakage:

$$\varepsilon_{\mathrm{leak}} \sim \exp\!\left(-\tau^{1/(1+\alpha)}\right)$$

Each gate requires time $O(\lambda^{-2})$. Choosing $\lambda = O(1/G)$ (where $G$ is the total gate count) ensures each gate has $O(1/G)$ infidelity, sufficient without error correction.

### Reduction complexity

The total spacetime volume is $\tilde{O}(n \cdot G^2 D)$ where $D$ is circuit depth and $n$ is qubit count. The source functions $J_1, J_2$ can be specified by $\tilde{O}(nG^2D)$ bits (via Nyquist-Shannon), and computing them from the circuit description is classically polynomial.

### BQP membership

The Hadamard test on the quantum simulation algorithm of [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes|JLP (2012)]] efficiently estimates the vacuum-to-vacuum amplitude. Alternatively, particle detector measurements at the end of the scattering process also yield BQP-completeness.

## Key results

**Theorem (informal):** The problem of deciding whether the vacuum-to-vacuum transition probability in $(1+1)$-dimensional massive $\phi^4$ theory with spacetime-dependent classical sources exceeds $2/3$ or is below $1/3$ is BQP-complete.

**Corollary:** If the initial state is allowed to contain particles already bound in potential wells (rather than starting from vacuum), the scattering problem is also BQP-hard. The vacuum initial-state requirement is a strict strengthening.

**Corollary:** The estimation problem (computing the transition probability to $\pm\varepsilon$) is BQP-hard for $\varepsilon < 1/6$.

## Comparison with prior work

| Paper | Result | Theory |
|---|---|---|
| [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes\|JLP (2012)]] | Efficient quantum simulation of $\phi^4$ scattering | $\phi^4$ in $D = 2,3,4$ spacetime dims |
| JLP (2014) | Detailed algorithm and resource analysis for $\phi^4$ scattering | $\phi^4$ in $(1+1)$D |
| Childs-Gosset-Webb (2013) | BQP-hardness for graph scattering | Particles on graphs |
| **This paper** | BQP-completeness of $\phi^4$ vacuum-to-vacuum amplitude | $\phi^4$ in $(1+1)$D with classical sources |

The graph-scattering hardness results of Childs-Gosset-Webb encode the problem instance in the graph structure. Here, the problem instance is encoded in the spacetime dependence of external fields — a more physically natural formulation, closer to how experiments actually work.

## Limits / caveats

- The perturbative analysis of particle interactions in the presence of time-dependent sources is **assumed** reliable, not rigorously proven. Asymptotic perturbation theory is established for $\phi^4$ without sources in $(1+1)$D, but the extension to time-dependent sources is an open mathematical question.
- The coupling $\lambda$ must be $O(1/G)$ — it decreases with circuit size. This means the theory becomes weaker as the computation gets larger. The result says even *weakly-coupled* QFT is hard to simulate classically, which is arguably the most interesting regime (strong coupling is "obviously" hard).
- The time to implement each two-qubit gate scales as $O(\lambda^{-2}) = O(G^2)$, giving total evolution time $O(G^2 D)$. This polynomial overhead is large but acceptable for a complexity-theoretic result.
- The construction uses $(1+1)$D only. Extension to higher spacetime dimensions should be easier (more room for qubits), but isn't explicitly done.
- The vacuum initial state is essential for the strongest version of the result. Without it, the construction simplifies but the hardness statement is weaker.
- Does not address QFTs without a mass gap (conformal field theories, massless theories), where the adiabatic arguments break down.

## Reusable ideas

1. [[Qubit Encoding in Double-Well Potentials]] — Using double-well potentials in a QFT to encode qubits, with logical states as symmetric/antisymmetric combinations of the ground state. The well separation controls the energy splitting (exponentially small in separation), giving a stable qubit with long coherence against tunnelling errors.

2. [[Adiabatic Passage for Particle Creation from Vacuum]] — Creating exactly one particle in a potential well by sweeping a driving frequency through the vacuum-to-bound-state resonance. The anharmonicity from $\lambda\phi^4$ shifts multi-particle transition frequencies out of band, suppressing unwanted particle production. This is a non-perturbative technique (resums an infinite class of Feynman diagrams) adapted from AMO physics to QFT.

3. [[Leakage Suppression via Adiabatic Gate Design]] — When the computational subspace is embedded in a larger Hilbert space, implementing gates by smooth parameter variation keeps the system in the computational subspace via the adiabatic theorem. Gevrey-class smoothness gives exponential suppression of leakage. Applicable whenever gate operations must avoid exciting out of a low-energy manifold.

## References within this paper

- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes|Jordan-Lee-Preskill (2012)]] — The quantum simulation algorithm whose BQP membership this paper complements with BQP-hardness.
- Jordan-Lee-Preskill (2014), arXiv:1112.4833 — Detailed resource analysis for $\phi^4$ scattering simulation (the companion "upper bound" paper).
- Childs-Gosset-Webb (2013), arXiv:1305.2562 — BQP-hardness of graph scattering. Related technique (encoding circuits in scattering) but on graphs rather than QFT.
- Knill-Laflamme-Milburn (2001) — KLM construction: universality from linear optics + measurements. Contrasted here: the BQP-hardness construction deliberately avoids intermediate measurements.
- Glimm-Jaffe (1987), Rivasseau (1991) — Mathematical foundations for asymptotic perturbation theory in $\phi^4_{1+1}$.
- Haah-Hastings-Kothari-Low (2018) — Product formula bounds for time-dependent Hamiltonians, relevant to the Suzuki-Trotter simulation of the source-dependent dynamics.

## Cross-links

### Paper notes
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]]
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]

### Trick cards
- [[Qubit Encoding in Double-Well Potentials]]
- [[Adiabatic Passage for Particle Creation from Vacuum]]
- [[Leakage Suppression via Adiabatic Gate Design]]
- [[Adiabatic Wavepacket Preparation with Phase Cancellation]]
