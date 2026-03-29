> **Source:** Toby Cubitt, Ashley Montanaro, *Complexity classification of local Hamiltonian problems*, SIAM J. Comput. **45**(2):268–316, 2016
> **Links:** [arXiv](https://arxiv.org/abs/1311.3161) · [SICOMP](https://doi.org/10.1137/S0097539714539391)
> **Tags:** #QMA #local-hamiltonian #complexity-classification #perturbation #dichotomy #StoqMA

---

## The computational problem

Given a fixed set $S$ of Hermitian matrices (interactions), define the **$S$-Hamiltonian problem**: given an $n$-qubit Hamiltonian $H = \sum_i \alpha_i H^{(i)}$ where each $H^{(i)}$ is proportional to some matrix in $S$, and a promise gap $b - a \geq 1/\mathrm{poly}(n)$, determine whether the smallest eigenvalue of $H$ is $\leq a$ or $\geq b$.

This is the quantum analogue of the maximisation variant of Schaefer's dichotomy theorem for boolean constraint satisfaction problems. The question: for every possible $S$, what is the complexity of $S$-Hamiltonian?

---

## What the paper does

Completely classifies the computational complexity of the $S$-Hamiltonian problem for all 2-qubit interactions $S$. Depending on $S$, the problem is one of exactly four things: in P, NP-complete, StoqMA-complete, or QMA-complete. There is nothing in between — a quantum dichotomy theorem.

This is the first systematic classification of local Hamiltonian problems, and it settles a number of previously open cases. The Heisenberg model ($XX + YY + ZZ$) and XY model ($XX + YY$) are shown QMA-complete for the first time, even without local magnetic fields. Prior work (Schuch-Verstraete, Biamonte-Love) had only established QMA-completeness with extra 1-local terms.

---

## The classification

### Theorem 5 ($S$-Hamiltonian with local terms)

When all 1-local interactions are available for free, for any constant $k$ and any set $S$ of $k$-qubit interactions:

| Condition on $S'$ (after removing 1-local parts) | Complexity |
|---|---|
| $S'$ is empty (all interactions are 1-local) | **P** |
| $\exists\, U \in \mathrm{SU}(2)$ that locally diagonalises every $H \in S'$ | **StoqMA-complete** |
| Otherwise | **QMA-complete** |

The StoqMA-complete class is precisely the class of problems polynomial-time equivalent to the transverse Ising model: $H = \sum_{i<j} \alpha_{ij} Z_i Z_j + \sum_k \beta_k X_k$. This was subsequently confirmed by Bravyi and Hastings (2014), who proved $\{ZZ, X\}$-Hamiltonian is StoqMA-complete.

### Theorem 7 ($S$-Hamiltonian without local terms)

For 2-qubit $S$ without free access to 1-local interactions:

| Condition | Complexity |
|---|---|
| Every matrix in $S$ is 1-local | **P** |
| $\exists\, U \in \mathrm{SU}(2)$ locally diagonalising $S$ | **NP-complete** |
| $\exists\, U$ such that each $H_i$ maps to $\alpha_i ZZ + (\text{1-local})$ | **StoqMA-complete** |
| Otherwise | **QMA-complete** |

The NP-complete class appears because without free local terms, purely diagonal interactions give classical constraint satisfaction problems.

---

## The framework

The classification proceeds in two phases:

**Phase 1 — Normal form reduction.** Any 2-qubit symmetric Hermitian matrix can be brought via local unitaries $U^{\otimes 2}$ to the form $\alpha XX + \beta YY + \gamma ZZ$ (plus 1-local terms). Antisymmetric matrices reduce to $\alpha(XZ - ZX)$. This dramatically limits the cases to consider. The key lemma (Lemma 9) uses the correlation matrix $M(H)$ and the fact that $U^{\otimes 2}$ conjugation acts as an $\mathrm{SO}(3)$ rotation on the Pauli coefficient matrix.

**Phase 2 — Gadget-based reductions.** Once interactions are in normal form, [[Perturbation Gadgets for Locality Reduction|perturbative gadgets]] are used to show that having access to one type of interaction lets you simulate another. For instance, from $XX + \gamma ZZ$ you can extract pure $XX$ and pure $ZZ$ by using mediator qubits projected into $|1\rangle$ or $|{-}\rangle$. Then $\{XX, ZZ\}$ with local terms is QMA-complete by prior results.

---

## Key gadget constructions

### Mediator qubit gadgets (second-order)

The paper uses the Oliveira-Terhal perturbation theory framework (Theorem 10, Corollaries 11–12). The basic setup: add a mediator qubit $b$ between qubits $a$ and $c$, with a strong penalty $\Delta |{\psi}\rangle\langle{\psi}|_b$ on the mediator. In the low-energy subspace (mediator in state $|{\psi^\perp}\rangle$), the second-order effective Hamiltonian generates cross-interactions between $a$ and $c$ that weren't present in the original set $S$.

Concretely, for interactions $H^{(1)}_{ab}$ and $H^{(2)}_{bc}$, the effective interaction is:
$$
H_{\mathrm{eff}} = -2 \sum_{i,j} \mathrm{Re}\!\left(\langle \psi^\perp | B^{(i)} | \psi \rangle \langle \psi | C^{(j)} | \psi^\perp \rangle\right) A^{(i)}_a D^{(j)}_c
$$
where the decompositions are $H^{(1)} = \sum_i A^{(i)} \otimes B^{(i)}$ and $H^{(2)} = \sum_j C^{(j)} \otimes D^{(j)}$.

By choosing $|{\psi}\rangle$ appropriately ($|0\rangle$, $|1\rangle$, $|{+}\rangle$, $|{-}\rangle$, etc.), different effective interactions are produced.

### The Heisenberg model encoding (Section 5)

The most technically interesting construction. The Heisenberg interaction $XX + YY + ZZ$ has full $\mathrm{SU}(2)$ symmetry, so any Hamiltonian built from it inherits this symmetry. Direct encoding of a general Hamiltonian is impossible.

Solution: encode each logical qubit into a block of 3 physical qubits, using the 2-dimensional ground space of the Heisenberg interaction on 4 qubits (a "star" graph with 3 leaves). Within this encoded subspace, the symmetry is broken and general interactions can be generated.

The construction requires an exactly solvable model — the **Lieb-Mattis model** — whose ground state is unique, globally entangled, and has efficiently computable correlation functions $\langle \psi | X_i X_j | \psi \rangle = -2/n$.

### $k$-to-$(k{-}1)$-local reduction (Lemma 19)

For the generalisation to $k > 2$: "fix" one qubit of a $k$-local interaction using a strong 1-local penalty $\Delta |\psi\rangle\langle\psi|$. This projects the qubit into state $|\psi^\perp\rangle$, generating an effective $(k{-}1)$-local interaction on the remaining qubits. Applied iteratively (a constant number of times), this reduces any $k$-local Hamiltonian to 2-local.

---

## Comparison with prior work

| Problem | Status before | Status after |
|---|---|---|
| $\{XX + YY + ZZ\}$-Hamiltonian (Heisenberg, no local terms) | Not even known NP-hard | **QMA-complete** |
| $\{XX + YY\}$-Hamiltonian (XY, no local terms) | Not known to be hard | **QMA-complete** |
| $\{XX + YY + ZZ, X, Y, Z\}$-Hamiltonian | QMA-complete [Schuch-Verstraete] | QMA-complete (reproved) |
| $\{ZZ, X\}$-Hamiltonian (transverse Ising) | In StoqMA [Bravyi et al.] | **StoqMA-complete** (by Bravyi-Hastings) |
| All 2-qubit $S$ | Individual cases known | **Full classification** |

---

## Limits / caveats

- The classification requires interaction weights to be arbitrary (positive or negative, polynomially large). Restricting to positive weights (e.g. antiferromagnetic Heisenberg) was an open problem at the time, partially resolved later by Piddock-Montanaro (2015).
- Spatial geometry restrictions (e.g. planarity) are only partially addressed. For the "with local terms" case, QMA-completeness holds on a 2D square lattice. For the "without local terms" case, the interaction graph is unrestricted.
- The classification for $k > 2$ without local terms remains incomplete — only the "with local terms" case extends to arbitrary $k$.
- Translational invariance is not considered.
- Computer algebra was needed to find many of the gadgets. Checking correctness is straightforward; finding them is not.

---

## Reusable ideas

1. **[[Normal Form for 2-Qubit Interactions via Pauli Correlation Matrix]]:** Any traceless symmetric 2-qubit Hermitian matrix can be reduced to $\alpha XX + \beta YY + \gamma ZZ$ (plus 1-local parts) by local-unitary conjugation $U^{\otimes 2}$. The transformation acts as $\mathrm{SO}(3)$ rotation on the Pauli coefficient matrix $M(H)$. Reduces the space of cases from continuous to a manageable finite set.

2. **[[Symmetry-Breaking Encoding via Degenerate Ground Spaces]]:** When the target interaction has a local symmetry (like $\mathrm{SU}(2)$ for Heisenberg), encode logical qubits into a degenerate ground space of a multi-qubit gadget. The encoded subspace breaks the symmetry, allowing simulation of interactions that the original symmetry would forbid.

3. **[[Mediator Qubit Gadget for Interaction Generation]]:** Use a mediator qubit with a strong penalty to generate effective cross-interactions between non-adjacent qubits. The first-order term vanishes by design; the second-order perturbative term produces the desired interaction. More flexible than the [[Subdivision Gadget for k-to-2-Local Reduction|subdivision gadget]] because the effective interaction depends on the choice of penalty state $|\psi\rangle$.

---

## References within this paper

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — the original 5-local QMA-completeness result; starting point for the classification
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe-Regev (2003)]] — 3-local QMA-completeness via the [[Perturbation Lemma for Locality Reduction|perturbation lemma]]
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|Kempe-Kitaev-Regev (2006)]] — 2-local QMA-completeness; the [[Perturbation Gadgets for Locality Reduction|perturbation gadgets]] and [[Subdivision Gadget for k-to-2-Local Reduction|subdivision gadget]] used here are refinements of that work
- Oliveira-Terhal (2008) — perturbation theory framework (Theorem 10); 2D square lattice QMA-completeness
- Schuch-Verstraete (2009) — QMA-completeness of Heisenberg/XY with local terms
- Biamonte-Love (2008) — QMA-completeness of certain 2-local interactions; gadget techniques
- Bravyi-Hastings (2014) — proved $\{ZZ, X\}$-Hamiltonian is StoqMA-complete, sharpening the classification
- Schaefer (1978) — the classical dichotomy theorem for boolean CSPs that this work generalises
- Piddock-Montanaro (2015) — subsequent work resolving the antiferromagnetic case and 2D lattice restrictions

---

## Cross-links

### Paper notes
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]]
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — IQP/stoquastic boundary relates to the StoqMA class here
- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]] — sequel: uses these complexity results to identify universal Hamiltonian families

### Trick cards
- [[Perturbation Gadgets for Locality Reduction]]
- [[Perturbation Lemma for Locality Reduction]]
- [[Subdivision Gadget for k-to-2-Local Reduction]]
- [[Normal Form for 2-Qubit Interactions via Pauli Correlation Matrix]]
- [[Symmetry-Breaking Encoding via Degenerate Ground Spaces]]
- [[Mediator Qubit Gadget for Interaction Generation]]
