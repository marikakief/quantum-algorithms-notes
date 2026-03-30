> **Source:** Dave Wecker, Matthew B. Hastings, Matthias Troyer, *Progress towards practical quantum variational algorithms*, arXiv:1507.08969, Phys. Rev. A **92**, 042303 (2015)
> **Links:** [arXiv](https://arxiv.org/abs/1507.08969) · [Journal](https://doi.org/10.1103/PhysRevA.92.042303)
> **Tags:** #VQE #NISQ #adiabatic-ansatz #variational #Hubbard #quantum-chemistry #measurement-scaling #practical-resources

---

## The computational problem

Two related problems:
1. **Quantum chemistry:** Find the ground-state energy of molecular Hamiltonians using short variational circuits without quantum error correction.
2. **Fermi-Hubbard model:** Find the ground-state energy and properties of the Hubbard model on ladders (up to 12 sites, 24 spin-orbitals) using a variational ansatz motivated by adiabatic state preparation.

The central resource question: how many measurements (shots) are needed to estimate the energy to chemical precision using VQE, and how does this scale with system size?

---

## What the paper does

This paper makes two important negative-positive contributions to the VQE landscape circa 2015:

**Negative (quantum chemistry):** For molecular Hamiltonians, the number of measurements required to estimate the energy to chemical accuracy $\varepsilon \sim 10^{-3}$ Ha scales as $O(N^8/\varepsilon^2)$ where $N$ is the number of spin-orbitals. This is **astronomically large** for any chemically interesting system (e.g., $\sim 10^{12}$–$10^{14}$ shots for a 50-orbital molecule). VQE as implemented in the era of this paper is not practically competitive with classical methods for quantum chemistry.

**Positive (Hubbard model):** For the Hubbard model and related strongly-correlated lattice systems, the measurement requirements are much more favorable. A **variational adiabatic ansatz** (discretizing the adiabatic path into a finite number of steps) achieves accurate energies with shallow circuits, and the measurement count is tractable for the problem sizes relevant to near-term hardware.

The paper proposes the adiabatic ansatz — now sometimes called the "QAOA-like" or "quantum alternating operator ansatz" — as a physically motivated alternative to UCC for lattice systems.

---

## The algorithm / construction

### Adiabatic ansatz

The ansatz approximates the adiabatic evolution from a trivial reference Hamiltonian $H_{\rm ref}$ to the target $H_{\rm target}$:

$$|\psi_{\rm adiabatic}(\vec\alpha, \vec\beta)\rangle = \prod_{k=1}^{p} e^{-i\beta_k H_{\rm target}} e^{-i\alpha_k H_{\rm ref}} |+\rangle^{\otimes n}$$

where $|+\rangle^{\otimes n}$ is the ground state of $H_{\rm ref}$ (e.g., all-pluses for a transverse-field reference).

This is the **QAOA-style ansatz** (predating the Farhi-Goldstone-Gutmann QAOA paper for optimization, but using the same structure). The parameters $(\vec\alpha, \vec\beta)$ are optimized variationally.

For the Hubbard model on a ladder:
- $H_{\rm ref}$: nearest-neighbor hopping (easily solved by free fermions)
- $H_{\rm target}$: full Hubbard Hamiltonian including on-site interaction $U$

### Circuit depth analysis

For the Hubbard model on a ladder with $L$ sites (linear chain or 2-leg ladder):

| System | Sites | Spin-orbitals | Ansatz depth $p$ | Two-qubit gates |
|---|---|---|---|---|
| Hubbard ladder | 4 | 8 | $p = 2$–$5$ | ~$20L$ per step |
| Hubbard chain | 12 | 24 | $p = 5$–$10$ | ~$20L$ per step |

Each step of the adiabatic ansatz requires implementing $e^{-i\alpha H_{\rm ref}}$ (free-fermion circuit, $O(L)$ depth on a chain) and $e^{-i\beta H_{\rm target}}$ (includes on-site interaction $e^{-i\beta U n_\uparrow n_\downarrow}$ and hopping terms).

**Convergence:** The adiabatic ansatz is shown to converge to the ground state energy faster than the UCC ansatz at the same circuit depth for the Hubbard model.

### Measurement scaling analysis for molecular Hamiltonians

The energy variance in VQE is:

$$\text{Var}[E] = \text{Var}\left[\sum_k c_k P_k\right] \leq \sum_k |c_k|^2$$

For a molecular Hamiltonian with $N$ spin-orbitals, the number of Pauli terms scales as $O(N^4)$ and the coefficients scale as $O(N^{-1})$ on average but with large variance. The total number of measurements to achieve precision $\varepsilon$ scales as:

$$M \sim \frac{\|H\|^2}{\varepsilon^2} \sim \frac{N^8}{Z^2 \varepsilon^2}$$

where $Z$ is the nuclear charge. For a 50-orbital molecule at $\varepsilon = 10^{-3}$ Ha: $M \sim 10^{13}$. At 1 kHz measurement rate, this is $\sim 30$ years. The paper concludes VQE is **not practical for molecular quantum chemistry** without dramatic improvements.

**The Hubbard contrast:** For the Hubbard model, the Hamiltonian has only $O(L)$ terms (hopping + on-site), so $\|H\|^2 \sim O(L)$ and $M \sim O(L/\varepsilon^2)$ — tractable for near-term hardware at $L = 12$.

### State preparation via adiabatic ramping

The adiabatic ansatz also provides a method to prepare trial ground states for QPE or DMRG comparison. The paper demonstrates that $p = 5$–$10$ layers give energies within $\sim 1\%$ of the exact ground state for 12-site Hubbard models.

---

## Key results

**Measurement scaling (negative result for quantum chemistry):**
- For molecules, measurement count grows as $O(N^{8+}/\varepsilon^2)$ — impractical for $N > 20$ at current rates
- "While the number of quantum measurements required to use variational methods for quantum chemistry is astronomically large..."

**Adiabatic ansatz for Hubbard (positive result):**
- Ansatz converges to ground-state energy within $\sim 0.5$–$1\%$ for 12-site ladders at depth $p \sim 10$
- Faster convergence than UCC at the same circuit depth
- Feasible for near-term quantum computers with $\sim 24$ qubits and $\sim 200$ two-qubit gates

**Key table (measurement count comparison):**

| Method | System | Measurements needed | Tractable? |
|---|---|---|---|
| VQE (Pauli grouping) | 10-orbital molecule | $\sim 10^{10}$ | No |
| VQE (Pauli grouping) | 50-orbital molecule | $\sim 10^{13}$ | No |
| VQE (adiabatic ansatz) | 12-site Hubbard | $\sim 10^4$–$10^5$ | Yes |

---

## Comparison with prior work

| Paper | Focus | Main contribution |
|---|---|---|
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al. 2014]] | HeH⁺ proof of concept | First VQE experiment |
| [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes|McClean et al. 2015]] | Theory | VQE formalism, adiabatic connections |
| **Wecker et al. 2015** | **Practical resources** | **Shows chemistry VQE impractical; Hubbard VQE tractable** |
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley et al. 2016]] | H₂ experiment | References this paper's measurement count analysis |
| [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes|Kandala et al. 2017]] | Hardware-efficient VQE | Proposes HE ansatz to reduce circuit depth |

This paper is an early honest accounting of VQE scalability — an important counterweight to the optimism of the era. The measurement scaling argument was later refined by [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin et al. 2018]] and [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins et al. 2021]] who showed the scaling is more like $O(N^{5})$ with better grouping strategies.

---

## Limits / caveats

- **Conservative measurement scaling:** The $O(N^8)$ bound assumes naive Pauli measurement (no grouping). Later work showed that basis rotation grouping, shadow tomography, and fermionic measurements reduce this substantially.
- **Hubbard model vs. real materials:** Lattice models like Hubbard are much simpler than quantum chemistry — they have short-range interactions, translation symmetry, and tractable ansätze. The gap between Hubbard and full quantum chemistry is large.
- **Classical optimization:** The paper doesn't analyze classical optimization difficulty (barren plateaus). Later work showed variational optimization can be exponentially hard even when the ansatz is correct.
- **Adiabatic ansatz depth:** The $p \sim 10$ layers needed for 12-site Hubbard correspond to $\sim 200$ two-qubit gates — feasible now but was near the edge at time of writing.

---

## Reusable ideas

1. [[Alternating Operator Ansatz]] — The adiabatic variational ansatz alternating reference and target Hamiltonian operators is the QAOA structure applied to quantum simulation. The intuition: approximate the adiabatic path by a Trotterized version.

2. **Measurement Count as Critical Resource** — Systematic analysis of shot count scaling for VQE. Key formula: $M \sim \|H\|_F^2 / \varepsilon^2$ where $\|H\|_F$ is the Frobenius norm. For molecular Hamiltonians, $\|H\|_F^2 \sim N^8$; for Hubbard, $\|H\|_F^2 \sim N$.

3. [[Variational Quantum Eigensolver (VQE)]] — Core algorithm; this paper extends the analysis to lattice models.

4. **Near-Term Target Selection** — The paper implicitly recommends Hubbard-type models as the right target for near-term VQE: they have few measurement terms, physically motivated ansätze, and are classically hard in the thermodynamic limit.

---

## Cross-links

### Paper notes
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]] — Companion paper by overlapping author group; fault-tolerant pipeline for Hubbard (adiabatic state prep + QPE + correlation functions). This paper is the VQE perspective; that one is the QPE perspective on the same physics.
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes]] — Concurrent theory paper; Wecker et al. is the practical companion.
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — VQE original; this paper provides the resource-scaling analysis missing from the experimental demo.
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — Shows that fermionic marginal constraints reduce the measurement count scaling from $O(N^{5.7})$ to $O(N^5)$.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Shows basis rotation grouping reduces scaling to $O(N^{2.75})$ empirically.
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — Companion paper by overlapping author group on Trotter error analysis for quantum chemistry.
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes]] — Later analysis showing hardware-efficient ansätze have barren plateaus; related to the optimization difficulty flagged implicitly here.
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]] — The adiabatic ansatz used here is essentially the QAOA structure applied to condensed matter systems.

### Trick cards
- [[Alternating Operator Ansatz]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Pauli Expectation Value Estimation]]
- [[Adiabatic State Preparation for Chemistry]]
