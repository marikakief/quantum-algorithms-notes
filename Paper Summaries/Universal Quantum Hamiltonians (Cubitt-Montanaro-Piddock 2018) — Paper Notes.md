> **Source:** Toby Cubitt, Ashley Montanaro, Stephen Piddock, *Universal quantum Hamiltonians*, Proc. Natl. Acad. Sci. **115**(38):9497–9502, 2018
> **Links:** [arXiv](https://arxiv.org/abs/1701.05182) · [PNAS](https://doi.org/10.1073/pnas.1804949115)
> **Tags:** #universal-Hamiltonian #analogue-simulation #perturbation #QMA #Heisenberg #encoding

---

## What the paper does

Proves that certain simple 2D spin-lattice models — including the Heisenberg model and the XY model with variable coupling strengths — are **universal**: they can simulate the entire physics of *any* quantum many-body system to arbitrary precision. "Entire physics" means the full energy spectrum, all eigenstates, partition function, time-evolution, local observables, correlation functions, and local noise processes are all reproduced.

This is a much stronger statement than QMA-completeness (which only concerns ground-state energy). The paper makes analogue Hamiltonian simulation rigorous by defining precisely what it means for one Hamiltonian to simulate another, proving that a short list of operational requirements forces the simulation map to have a specific algebraic form, and then showing that the QMA-complete interactions from [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes|Cubitt-Montanaro (2016)]] are exactly the universal ones.

---

## The simulation framework

### Hamiltonian encodings

An encoding map $E$ takes a Hamiltonian $H$ to a simulator Hamiltonian $E(H)$. The paper requires $E$ to satisfy:
1. $E(A) = E(A)^\dagger$ (Hermiticity preservation)
2. $\mathrm{spec}(E(A)) = \mathrm{spec}(A)$ (spectrum preservation)
3. $E(\sum_i \alpha_i h_i) = \sum_i \alpha_i E(h_i)$ (linearity — encode terms separately)

Using Jordan- and C*-algebra techniques, they prove that any map satisfying just these three conditions must have the form:
$$
E(H) = U(H^{\oplus p} \oplus \bar{H}^{\oplus q})U^\dagger
$$
for some unitary $U$ and non-negative integers $p, q$ with $p + q \geq 1$, where $\bar{H}$ is complex conjugation. This is the key structural result — the form of the encoding is forced by very basic operational requirements.

### Approximate simulation

In practice, $H'$ simulates $H$ to precision $(\eta, \varepsilon)$ below an energy cutoff $\Delta$ if:
- There exists a local encoding $E$ (with local isometries $V = \bigotimes_i V_i$) such that the low-energy subspace of $H'$ (below $\Delta$) approximately implements $E$
- $\|H'_{\leq \Delta} - \tilde{E}(H)\| \leq \varepsilon$
- The encoding isometry is $\eta$-close to exact: $\|\tilde{V} - V\| \leq \eta$

### What simulation preserves

For a simulation to precision $(\eta, \varepsilon)$ with cutoff $\Delta$:

- **Static properties:** All energy levels preserved to $\varepsilon$. All local observables, order parameters, correlation functions reproduced via local maps.
- **Thermodynamic properties:** Partition function error: $|Z_{H'}(\beta) - (p+q)Z_H(\beta)| / ((p+q)Z_H(\beta)) \leq d^{m-n} e^{-\beta\Delta}/((p+q)e^{-\beta\|H\|}) + (e^{\varepsilon\beta} - 1)$
- **Dynamical properties:** Time-evolution error grows linearly in time: $\|e^{-iH't} E_{\mathrm{state}}(\rho) e^{iH't} - E_{\mathrm{state}}(e^{-iHt}\rho\, e^{iHt})\|_1 = O(t\varepsilon + \eta)$
- **Noise:** Local errors on the simulator approximate local errors on the original system: $E_{\mathrm{state}}(\mathcal{N}(\rho)) = \mathcal{N}'(E_{\mathrm{state}}(\rho)) + O(\sqrt{\eta})$

---

## The universality classification

The paper completely classifies all 2-qubit interactions by simulation power. The classification coincides exactly with the [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes|Cubitt-Montanaro complexity classification]]:

| Simulation class | Complexity class | Can simulate | Examples |
|---|---|---|---|
| **Universal** | QMA-complete | Any local Hamiltonian (qubits, qudits, bosons, fermions) | Heisenberg ($XX{+}YY{+}ZZ$), XY ($XX{+}YY$) |
| **Stoquastic-universal** | StoqMA-complete | Any stoquastic Hamiltonian | Transverse Ising ($ZZ + X$) |
| **Classical-universal** | NP-complete | Any classical (diagonal) Hamiltonian | Classical Ising ($ZZ$) |
| **Trivial** | P | Only 1-local Hamiltonians | Single-qubit terms |

The identity between the complexity classification and the simulation classification is not coincidental — it's a consequence of the gadget constructions in both proofs being simulation-preserving (not just preserving ground-state energy).

---

## The construction (proof of universality)

The proof chains together a sequence of simulations, each building on the previous:

### Step 1: Breaking symmetry (Heisenberg → Pauli interactions without $\sigma_y$)

The Heisenberg interaction $XX + YY + ZZ$ has full $\mathrm{SU}(2)$ symmetry. Same idea as in [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes|Cubitt-Montanaro (2016)]]: encode each logical qubit into a 4-qubit gadget where strong Heisenberg interactions create a 2-dimensional ground space, using the [[Symmetry-Breaking Encoding via Degenerate Ground Spaces|symmetry-breaking encoding]]. Within this encoded subspace, general 2-qubit Pauli interactions (without $\sigma_y$) can be generated.

### Step 2: Pauli interactions without $\sigma_y$ → all odd-weight real terms

Use [[Mediator Qubit Gadget for Interaction Generation|mediator qubit gadgets]]: a mediator with a strong penalty, flanked by 2-body interactions, generates effective cross-interactions at second order. This lifts the locality from 2 to $(2k+1)$ for arbitrary real interactions without $\sigma_y$.

### Step 3: Odd-weight real terms → all real $k$-local

Further mediator gadgets convert $(2k+1)$-local interactions to arbitrary $2k$-local real interactions.

### Step 4: Real Hamiltonians → complex Hamiltonians (the complex-to-real encoding)

All matrix elements of the Heisenberg or XY interactions are real, so any Hamiltonian built from them is real. To simulate a complex Hamiltonian $H$, use the encoding:
$$
H' = \mathrm{Re}(H) \oplus \mathrm{Im}(H) = H \otimes |{+_y}\rangle\langle{+_y}| + \bar{H} \otimes |{-_y}\rangle\langle{-_y}|
$$
where $|\pm_y\rangle = (|0\rangle \pm i|1\rangle)/\sqrt{2}$. This is manifestly real and encodes $H$ with $p = q = 1$. Made local by adding one ancilla per physical qubit and forcing them into $\mathrm{span}\{|{+_y}\rangle^{\otimes n}, |{-_y}\rangle^{\otimes n}\}$ via strong local interactions.

### Step 5: Qubits → qudits, fermions, bosons

- **Qudits:** Encode each $d$-dimensional spin into $\lceil \log_2 d \rceil$ qubits
- **Fermions:** Standard Jordan-Wigner or Bravyi-Kitaev transformation gives a qubit simulation
- **Bosons:** Truncate to finite local dimension, then encode as qudits

### Spatial embedding

Arbitrary interaction graphs can be embedded in a 2D square lattice using crossing gadgets (Oliveira-Terhal technique). This costs an exponential energy overhead for dense graphs but is efficient for spatially sparse Hamiltonians (including all 2D models).

---

## Key results

**Theorem (informal):** The 2D Heisenberg model with variable coupling strengths is universal — it can simulate any local Hamiltonian on qubits, qudits, fermions, or bosons, to any desired precision.

The same holds for the 2D XY model with variable coupling strengths.

For spatially sparse target Hamiltonians: simulation is **efficient** — polynomial overhead in system size, energy, and inverse precision. For dense graphs: polynomial space overhead but exponential energy overhead.

---

## Comparison with prior work

| Concept | Lloyd (1996) | This paper |
|---|---|---|
| Type of simulation | Digital (gate-based) | Analogue (Hamiltonian engineering) |
| Error correction needed? | Yes (for scalability) | No (local errors map to local errors) |
| What's preserved? | Time-evolution | Everything: spectrum, observables, dynamics, thermodynamics, noise |
| Universality of? | Quantum computation | Quantum many-body physics |

Related classical results: De las Cuevas-Cubitt (2016) showed universality for classical spin systems. This paper extends the idea to the quantum setting.

---

## Limits / caveats

- **Energy overhead:** Simulating higher-dimensional or long-range Hamiltonians on a 2D lattice requires exponentially large coupling strengths. The paper does not claim this is avoidable.
- **Coupling control:** The constructions require precise, site-by-site control of coupling strengths over many orders of magnitude. This is beyond current experimental capability.
- **Not translationally invariant:** The coupling strengths must vary from site to site. Translational invariance is a fundamental constraint not addressed here (though Hamiltonian complexity results suggest it shouldn't be an obstacle to hardness).
- **Time-evolution error:** Grows linearly in $t$. Without active error correction, long-time dynamics will drift. The paper argues this is acceptable because the physical system being simulated has the same issue.
- **The encoding is perturbative:** Relies on energy scale separation. In practice, finite-size effects and thermal fluctuations could disrupt the simulation.

---

## Reusable ideas

1. **[[Jordan-Algebra Classification of Hamiltonian Encodings]]:** Any spectrum-preserving, Hermiticity-preserving, linear encoding of Hamiltonians must have the form $E(H) = U(H^{\oplus p} \oplus \bar{H}^{\oplus q})U^\dagger$. This is an algebraic constraint, not a choice — it follows from Jordan algebra structure theorems. Useful for characterising what any analogue simulation can look like.

2. **[[Complex-to-Real Hamiltonian Encoding]]:** Encode a complex Hamiltonian $H$ into a real one via $H' = H \otimes |{+_y}\rangle\langle{+_y}| + \bar{H} \otimes |{-_y}\rangle\langle{-_y}|$. Made local by adding ancilla qubits forced into a symmetric subspace. Converts any real-Hamiltonian simulation into a complex-Hamiltonian simulation at constant overhead.

---

## References within this paper

- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes|Cubitt-Montanaro (2016)]] — the complexity classification that this paper builds on; the universal class = the QMA-complete class
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|Kempe-Kitaev-Regev (2006)]] — perturbative gadget techniques that underpin the constructions
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe-Regev (2003)]] — early locality reduction techniques
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — digital quantum simulation; the analogue counterpart to this paper's results
- Oliveira-Terhal (2008) — perturbation theory framework; crossing gadgets for 2D embedding
- Bravyi-Hastings (2014/2017) — StoqMA-completeness of transverse Ising; stoquastic simulation results used here
- De las Cuevas-Cubitt (2016) — classical universality for spin systems; precursor to this work
- Piddock-Montanaro (2017) — resolved antiferromagnetic cases; extended lattice restrictions

---

## Cross-links

### Paper notes
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]]
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]]
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — digital simulation counterpart
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — adiabatic universality relates to this paper's analogue universality

### Trick cards
- [[Perturbation Gadgets for Locality Reduction]]
- [[Symmetry-Breaking Encoding via Degenerate Ground Spaces]]
- [[Mediator Qubit Gadget for Interaction Generation]]
- [[Jordan-Algebra Classification of Hamiltonian Encodings]]
- [[Complex-to-Real Hamiltonian Encoding]]
- [[Normal Form for 2-Qubit Interactions via Pauli Correlation Matrix]]
