> **Source:** A. Yu. Kitaev, A. H. Shen, and M. N. Vyalyi, *Classical and Quantum Computation*, AMS Graduate Studies in Mathematics **47** (2002); results from [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]]
> **Links:** [AMS](https://bookstore.ams.org/gsm-47)
> **Tags:** #QMA #local-hamiltonian #complexity #history-state #foundational

---

## What the paper does

Proves that the **$k$-local Hamiltonian problem** is QMA-complete for $k = 5$, establishing the quantum analogue of Cook-Levin. This is the complexity-theoretic foundation for understanding why finding ground state energies is hard — even for quantum computers.

The proof introduces two constructions that became standard tools far beyond complexity theory: the [[History-State Encoding with Unary Clock|history-state encoding]] and the **geometric lemma** for bounding spectral gaps.

---

## The local Hamiltonian problem

**Input:** A set of $k$-local Hamiltonians $H_1, \ldots, H_m$ on $n$ qubits (each $H_i$ acts on at most $k$ qubits, $0 \preceq H_i \preceq I$), and thresholds $a < b$ with $b - a \geq 1/\mathrm{poly}(n)$.

**Promise:** Either $\lambda_{\min}(\sum_i H_i) \leq a$ (YES) or $\lambda_{\min}(\sum_i H_i) \geq b$ (NO).

**Output:** Which case holds.

This is the quantum analogue of MAX-$k$-SAT: instead of asking whether all classical constraints can be simultaneously satisfied, we ask whether all quantum constraints can be approximately satisfied.

**Theorem (Kitaev):** The 5-local Hamiltonian problem is QMA-complete.

---

## The proof construction

The reduction from any QMA verification circuit to a local Hamiltonian uses the [[History-State Encoding with Unary Clock|Kitaev history state]]:

$$
|\eta\rangle = \frac{1}{\sqrt{L+1}} \sum_{\ell=0}^{L} |\alpha(\ell)\rangle \otimes |1^\ell 0^{L-\ell}\rangle_c
$$

The Hamiltonian $H = H_{\mathrm{in}} + H_{\mathrm{out}} + H_{\mathrm{prop}} + H_{\mathrm{clock}}$ penalizes:
- Wrong input ($H_{\mathrm{in}}$)
- Wrong output ($H_{\mathrm{out}}$)
- Incorrect propagation ($H_{\mathrm{prop}}$ — the 5-local terms)
- Illegal clock states ($H_{\mathrm{clock}}$)

**YES case:** There exists a witness $|w\rangle$ such that the verification circuit accepts with high probability → the history state built from this witness has low energy $\leq a$.

**NO case:** No witness makes the circuit accept → all history states have high energy $\geq b$.

The gap $b - a$ is $\Omega(1/L^2)$ from the spectral gap analysis, which is the same analysis later refined in [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]].

---

## The geometric lemma (Lemma 14.4)

> If $H_1, H_2$ are positive semidefinite with ground energies $a_1, a_2$, spectral gaps $\geq \Lambda$, and ground spaces at angle $\theta$, then:
> $$\lambda_{\min}(H_1 + H_2) \geq a_1 + a_2 + 2\Lambda \sin^2(\theta/2)$$

This is used throughout the local Hamiltonian complexity literature and in the [[Perturbation Lemma for Locality Reduction|locality reduction]] techniques.

---

## Reusable ideas

1. **[[History-State Encoding with Unary Clock]]:** The construction that makes local verification of quantum computation possible. Originated here for QMA-completeness; later used for [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|adiabatic equivalence]] and [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|quantum ODE algorithms]].

2. **[[Kitaev Geometric Lemma for Ground Space Angles]]:** Lower bound on the ground energy of a sum of Hamiltonians from the angle between their ground spaces.

---

## References within this paper

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — [[Gapped Phase Estimation|phase estimation]], used in the [[Hamiltonian-to-Projection via Phase Estimation|Hamiltonian-to-projection]] construction

---

## Cross-links

### Paper notes
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — uses the same history-state construction for adiabatic equivalence
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]] — reduces locality to 3
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]] — reduces locality to 2
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]] — VQE attempts to solve instances of this problem heuristically

### Trick cards
- [[History-State Encoding with Unary Clock]]
- [[Kitaev Geometric Lemma for Ground Space Angles]]
- [[Perturbation Lemma for Locality Reduction]]
