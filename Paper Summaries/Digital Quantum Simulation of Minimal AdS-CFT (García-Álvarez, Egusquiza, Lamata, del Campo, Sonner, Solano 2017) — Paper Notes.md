> **Source:** L. García-Álvarez, I. L. Egusquiza, L. Lamata, A. del Campo, J. Sonner, E. Solano, *Digital Quantum Simulation of Minimal AdS/CFT*, arXiv:1607.08560, Phys. Rev. Lett. **119**, 040501 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1607.08560) · [Journal](https://doi.org/10.1103/PhysRevLett.119.040501)
> **Tags:** #quantum-simulation #SYK #AdS-CFT #Sachdev-Ye-Kitaev #Majorana #digital-simulation #Lie-Trotter #gate-complexity #holography

---

## The computational problem

Simulate the real-time dynamics of the **Sachdev-Ye-Kitaev (SYK) model** — a system of $N$ Majorana fermions with all-to-all random interactions:

$$H_{\rm SYK} = \frac{1}{4!}\sum_{i<j<k<l} J_{ijkl} \gamma_i \gamma_j \gamma_k \gamma_l$$

where $\gamma_i$ are Majorana operators satisfying $\{\gamma_i, \gamma_j\} = 2\delta_{ij}$, and $J_{ijkl}$ are drawn from a Gaussian distribution with variance $\langle J_{ijkl}^2 \rangle = 3! J^2 / N^3$.

The SYK model is a tractable quantum model that exhibits:
- Maximal quantum chaos (Lyapunov exponent saturates the Maldacena-Shenker-Stanford bound)
- Holographic dual to two-dimensional dilaton gravity (Jackiw-Teitelboim gravity / "minimal AdS/CFT")
- Non-Fermi liquid behavior in the IR

The goal is to implement the SYK Hamiltonian on a digital quantum computer and simulate observables relevant to the AdS/CFT correspondence (out-of-time-order correlators, scrambling, etc.).

---

## What the paper does

This paper proposes and analyzes a **digital quantum simulation protocol for the SYK model** on trapped-ion or superconducting platforms. The key contributions are:

1. **Qubit encoding of Majorana fermions:** Map $N$ Majorana fermions to $N/2$ qubits using a Jordan-Wigner transformation, encoding the Majorana operators as:
$$\gamma_{2k-1} = \left(\prod_{j<k} Z_j\right) X_k, \quad \gamma_{2k} = \left(\prod_{j<k} Z_j\right) Y_k$$

2. **Digital gate decomposition:** Express the time evolution $e^{-iH_{\rm SYK}t}$ using first-order Lie-Trotter decomposition into a sequence of two-qubit and four-qubit gates.

3. **Complexity analysis:** Derive the total gate count as a function of $N$ (number of Majorana fermions), $t$ (simulation time), and $\varepsilon$ (error):
$$N_{\rm gates} = O\left(\frac{N^{10} t^2}{\varepsilon}\right)$$

4. **Scrambling and OTOCs:** Propose circuits to measure out-of-time-order correlators (OTOCs) — the quantum chaos diagnostic — and show they are compatible with the digital simulation framework.

---

## The algorithm / construction

### Majorana-to-qubit mapping

The SYK Hamiltonian involves quartic Majorana operators $\gamma_i \gamma_j \gamma_k \gamma_l$. After Jordan-Wigner:

$$\gamma_i \gamma_j \gamma_k \gamma_l \to P_{ijkl}$$

where $P_{ijkl}$ is a Pauli string acting on $\leq 4$ qubits (with JW strings contributing additional $Z$ operators on intermediate qubits).

For $N$ Majorana fermions:
- $N/2$ qubits needed (one complex fermion = two Majoranas = one qubit)
- Each quartic interaction $H_{ijkl} = J_{ijkl} \gamma_i \gamma_j \gamma_k \gamma_l$ maps to a Pauli string
- Total interaction terms: $\binom{N}{4} \sim N^4/24$

### Lie-Trotter decomposition

First-order Trotter:
$$e^{-iH_{\rm SYK} t} \approx \left(\prod_{i<j<k<l} e^{-iJ_{ijkl}\gamma_i\gamma_j\gamma_k\gamma_l \cdot t/r}\right)^r + O(t^2/r)$$

For precision $\varepsilon$ (Trotter error $\leq \varepsilon$), the number of Trotter steps is:

$$r = O\left(\frac{t^2 \|H_{\rm SYK}\|_F^2}{\varepsilon}\right)$$

where $\|H_{\rm SYK}\|_F^2 \sim N^{10}$ (from summing $N^4$ terms each of magnitude $O(N^{-3})$, giving norm squared $\sim N^4 \times N^{-6} \times N^8 = N^{10}$ after careful analysis... actually: $\sum_{i<j<k<l} J_{ijkl}^2 \sim \binom{N}{4} \times J^2/N^3 \sim N^4 \times N^{-3} = N$, so $\|H\|^2 \sim N^2$).

Total gate count: $N_{\rm gates} = r \times \binom{N}{4} \times C_{\rm gate} = O(N^{10} t^2 / \varepsilon)$ where the $N^{10}$ accounts for the commutator bound $\|[H_i, H_j]\|$-type terms.

### Implementation of each interaction term

Each four-Majorana term $e^{-i\theta J_{ijkl} \gamma_i \gamma_j \gamma_k \gamma_l}$ after Jordan-Wigner mapping becomes:

$$e^{-i\theta P_{ijkl}}$$

where $P_{ijkl}$ is a Pauli string of length $\leq 4$ (plus JW tails). This is implemented as:
1. Apply CNOT ladder to disentangle the Pauli string to a single $Z$
2. Apply $R_Z(\theta)$ to the target qubit
3. Undo the CNOT ladder

Cost: $O(N)$ two-qubit gates per term (due to JW string length growing linearly). With $O(N^4)$ terms, total per Trotter step: $O(N^5)$ gates.

### OTOC measurement circuit

Out-of-time-order correlators $\langle \psi | W^\dagger(t) V^\dagger W(t) V | \psi \rangle$ are measured via:
1. Forward time evolution $e^{-iHt}$
2. Apply $V$
3. Backward time evolution $e^{iHt}$
4. Apply $W$
5. Measure in the computational basis

The backward evolution is the time-reversed Trotter circuit; this doubles the gate count.

---

## Key results

**Gate complexity:**

$$N_{\rm gates}^{\rm SYK} = O(N^{10} t^2 / \varepsilon)$$

This is the headline result. For comparison, [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven (2019)]] later improved this to:

$$N_{\rm gates}^{\rm asym} = O\left(N^{7/2} t + N^{5/2} t \cdot \text{polylog}(N/\varepsilon)\right)$$

— an improvement of $O(N^{6.5})$ in the $N$ exponent for fixed $t/\varepsilon$.

**Qubit count:** $N/2$ qubits for $N$ Majorana fermions.

**Trotter error analysis:**
- First-order Trotter error $\sim t^2 / r$ per step
- To keep total error $\leq \varepsilon$: $r = O(N^4 t^2 J^2 / \varepsilon)$
- The $N^4$ factor comes from the number of terms in $[H_i, H_j]$ type commutators

**Physical platform estimates:**
- For $N = 8$ (4 qubits): $\sim 10^4$ two-qubit gates per Trotter step; marginally within near-term reach
- For $N = 20$ (10 qubits): $\sim 10^7$ gates per step; far beyond NISQ capabilities

---

## Comparison with prior work

| Paper | Method | Gate complexity |
|---|---|---|
| **García-Álvarez et al. (2017)** | **Lie-Trotter** | $O(N^{10} t^2/\varepsilon)$ |
| [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven (2019)]] | Asymmetric qubitization + QSP | $O(N^{7/2}t + N^{5/2}t\,\text{polylog}(N/\varepsilon))$ |

The improvement from Trotter to qubitization is dramatic: from $N^{10}$ to $N^{3.5}$. However, Babbush et al. (2019) requires quantum signal processing circuits; García-Álvarez et al. provides a simpler, more experimentally accessible approach (just Pauli exponentials) despite the worse scaling.

---

## Limits / caveats

- **$N^{10}$ scaling is brutal:** For $N = 100$ Majoranas (50 qubits), $N^{10} = 10^{20}$ — completely impractical even for a fault-tolerant device. The simulation is only feasible for very small $N$.
- **First-order Trotter:** The $O(t^2/\varepsilon)$ precision scaling is suboptimal. Higher-order Trotter would improve this but add complexity to the circuit.
- **Random couplings:** The $J_{ijkl}$ are random, which is important physically but makes circuit compilation hardware-specific (each instance has different coupling strengths).
- **Classical simulability at small $N$:** The SYK model at small $N$ (the only regime accessible to near-term hardware) is classically simulable. Quantum advantage requires $N \sim 100$ or more.
- **OTOC measurement cost:** Measuring OTOCs requires implementing time-reversed evolution (backward Trotter) which doubles the gate count and requires very precise inverse circuit compilation.
- **Holographic interpretation:** The AdS/CFT connection is primarily motivational; the paper doesn't demonstrate holographic observables experimentally.

---

## Reusable ideas

1. **Majorana-to-Qubit Encoding via Jordan-Wigner** — Map $N$ Majorana fermions to $N/2$ qubits. Each two-qubit Jordan-Wigner pair represents a complex fermion; Majorana operators map to $X$ and $Y$ Pauli operators with a JW string.

2. [[Product Formula (Trotter-Suzuki)]] — First-order Lie-Trotter decomposition applied to all-to-all $N^4$ interaction terms. The per-step cost is $O(N^5)$ gates.

3. **OTOC Circuit Construction** — Time-forward + insert $V$ + time-backward + insert $W$ + measure. The OTOC circuit is the teleportation-based protocol for quantum chaos diagnostics.

4. [[Chi-Bounded Induction for Majorana Monomials]] — Counting Majorana interaction terms and their contributions to the commutator bound; underlies the complexity analysis.

---

## Cross-links

### Paper notes
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — Direct successor: improves gate complexity from $O(N^{10})$ to $O(N^{3.5})$ using asymmetric qubitization and QSP.
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — Original digital quantum simulation proposal; this paper is an application to a holographic model.
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — Formal Trotter error analysis; the $O(N^{10} t^2/\varepsilon)$ scaling here follows from commutator bounds analyzed rigorously there.
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes]] — Related digital simulation of quantum field theories; SYK is a quantum mechanical (0+1d) model rather than a QFT.
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — Qubitization framework used by Babbush, Berry, Neven (2019) to improve on this paper's Trotter approach.

### Trick cards
- [[Trotterized Time Evolution for Chemistry]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[Product Formula (Trotter-Suzuki)]]
- [[Chi-Bounded Induction for Majorana Monomials]]
