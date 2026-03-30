> **Source:** Toby Cubitt, Ashley Montanaro, *Complexity classification of local Hamiltonian problems*, SIAM Journal on Computing **45**(2):268–316, 2016
> **Links:** [arXiv](https://arxiv.org/abs/1311.3161) · [SICOMP](https://doi.org/10.1137/S0097539714539391)
> **Tags:** #QMA #local-hamiltonian #complexity-classification #StoqMA #NP #perturbation

---

## The computational problem

Fix a finite set $S$ of Hermitian interactions, each acting on at most $k=O(1)$ qubits. The **$S$-Hamiltonian problem** is the restriction of the local Hamiltonian problem where every local term in the instance is a real scalar multiple of some interaction from $S$.

More explicitly, the input is a Hamiltonian
$$
H = \sum_i H^{(i)}
$$
on $n$ qubits, where each $H^{(i)}$ acts nontrivially on at most $k$ qubits and, after stripping off identities on untouched qubits, is proportional to an element of $S$. Given thresholds $a < b$ with $b-a \ge 1/\mathrm{poly}(n)$, decide whether the smallest eigenvalue of $H$ is at most $a$ or at least $b$.

The paper studies two versions:

1. **$S$-Hamiltonian with local terms**: all 1-qubit Hermitian terms are available for free.
2. **$S$-Hamiltonian** without that extra promise.

This is the quantum analogue of the maximisation version of Schaefer-style classical CSP dichotomy theorems: for a fixed menu of allowed interactions, what is the exact complexity of the resulting ground-energy problem?

---

## What the paper does

This paper gives a full complexity classification of all 2-qubit qubit-interaction families. Depending on $S$, the problem is exactly one of **P**, **NP-complete**, **StoqMA-complete**, or **QMA-complete**. There is no intermediate behaviour in the 2-local qubit setting.

The result is especially important because it proves for the first time that the **Heisenberg** interaction $XX+YY+ZZ$ and the **XY** interaction $XX+YY$ are QMA-complete **without adding arbitrary local fields**. That is the conceptual step beyond [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]: not just one hard 2-local problem, but a map of the whole landscape.

---

## The algorithm / construction

The proof has two main layers.

### 1. Put every 2-qubit interaction into a manageable normal form

Any traceless 2-qubit Hermitian matrix can be written as
$$
H = \sum_{i,j=1}^3 M_{ij}\, \sigma_i \otimes \sigma_j + \sum_{k=1}^3 v_k \sigma_k \otimes I + w_k I \otimes \sigma_k,
$$
where $M(H)$ is the $3\times 3$ Pauli correlation matrix.

Conjugating by the same 1-qubit unitary on both qubits,
$$
H \mapsto U^{\otimes 2} H (U^\dagger)^{\otimes 2},
$$
acts on $M(H)$ by an $\mathrm{SO}(3)$ rotation. This yields [[Normal Form for 2-Qubit Interactions via Pauli Correlation Matrix]]:

- if $H$ is symmetric under swap, then after a local basis change it has the form
  $$
  \alpha XX + \beta YY + \gamma ZZ + (\text{1-local terms});
  $$
- if $H$ is antisymmetric under swap, then after a local basis change it has the form
  $$
  \alpha(XZ-ZX) + (\text{1-local terms}).
  $$

This collapses a continuous space of possible interactions into a small set of canonical cases.

### 2. Use gadgets to move between canonical interactions

Once an interaction is in normal form, the job is to show what other interactions it can simulate. The paper uses several perturbative constructions.

#### Mediator-qubit gadgets

The main workhorse is [[Mediator Qubit Gadget for Interaction Generation]]. A heavy penalty on a mediator qubit forces the low-energy sector into a chosen state, and second-order perturbation theory generates an effective interaction between neighbouring system qubits. This lets the authors extract terms like pure $XX$ or pure $ZZ$ from mixtures such as $XX+\gamma ZZ$.

#### Symmetry-breaking encodings

The hardest cases are highly symmetric interactions, especially the Heisenberg model. A Hamiltonian built only from $XX+YY+ZZ$ is globally $\mathrm{SU}(2)$-invariant, so one cannot directly write down a generic encoded target Hamiltonian in the physical qubits. The fix is [[Symmetry-Breaking Encoding via Degenerate Ground Spaces]]: encode each logical qubit into the degenerate ground space of a small Heisenberg gadget, then use weaker couplings between gadgets to generate effective logical interactions.

For the Heisenberg case, the proof leans on an exactly solvable Lieb-Mattis-type construction to get a controlled 2-dimensional encoded ground space with computable correlation functions.

#### Generalisation beyond 2-local, with free local terms

For Theorem 5, the classification with free 1-local terms extends to arbitrary constant locality $k$. The idea is to project out one qubit of a $k$-body interaction using a heavy 1-local penalty, thereby reducing $k$-local to $(k-1)$-local, and repeat until the problem is 2-local.

---

## Key results

### Classification with free 1-local terms (Theorem 5)

Let $S'$ be obtained from $S$ by removing the 1-local part of every interaction and discarding any resulting 0-local terms.

Then for any fixed $k$:

- If $S'$ is empty, then **$S$-Hamiltonian with local terms is in P**.
- If there exists $U \in \mathrm{SU}(2)$ such that $U$ locally diagonalises every element of $S'$, then **$S$-Hamiltonian with local terms is StoqMA-complete**.
- Otherwise, **$S$-Hamiltonian with local terms is QMA-complete**.

For 2-qubit interactions, the QMA-complete cases remain QMA-complete even when the final Hamiltonian is restricted to a **2D square lattice** with **equal-weight** 2-body couplings.

### Classification without free 1-local terms (Theorem 7)

For any fixed set $S$ of interactions on at most 2 qubits:

- If every element of $S$ is 1-local, then **$S$-Hamiltonian is in P**.
- If there exists $U \in \mathrm{SU}(2)$ that locally diagonalises $S$, then **$S$-Hamiltonian is NP-complete**.
- If there exists $U$ such that each 2-qubit interaction $H_i \in S$ satisfies
  $$
  U^{\otimes 2} H_i (U^\dagger)^{\otimes 2} = \alpha_i ZZ + A_i\otimes I + I \otimes B_i,
  $$
  then **$S$-Hamiltonian is StoqMA-complete**.
- Otherwise, **$S$-Hamiltonian is QMA-complete**.

### Important examples

- $\{XX+YY+ZZ\}$-Hamiltonian is **QMA-complete**.
- $\{XX+YY\}$-Hamiltonian is **QMA-complete**.
- The transverse Ising family $\{ZZ, X\}$ is the canonical **StoqMA-complete** case.
- Diagonal interaction families give the **NP-complete** classical corner.

My read: the real contribution is not only the list of classes, but the fact that the proof exposes the structure behind them. The four classes are not an accident of case-bashing; they come from a rigid normal-form plus simulation picture.

---

## Comparison with prior work

| Problem / interaction family | Before this paper | After this paper |
|---|---|---|
| [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|5-local Hamiltonian]] | QMA-complete | baseline starting point |
| [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|3-local Hamiltonian]] | QMA-complete | already known |
| [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|2-local Hamiltonian]] | QMA-complete | already known |
| Heisenberg $XX+YY+ZZ$ without local fields | hardness not known | **QMA-complete** |
| XY $XX+YY$ without local fields | hardness not known | **QMA-complete** |
| All 2-qubit interaction sets $S$ | isolated examples only | **full P / NP / StoqMA / QMA classification** |

Earlier hardness proofs such as Schuch–Verstraete and Biamonte–Love needed arbitrary 1-local terms. This paper shows that for the Heisenberg and XY models the local fields were not the source of hardness.

---

## Limits / caveats

- The classification allows **arbitrary positive and negative coupling strengths**. That is mathematically clean, but physically restrictive sign constraints can change the story.
- The full result without free local terms is only proved for **2-qubit interactions**.
- The $k>2$ extension only covers the **with-local-terms** setting.
- Translational invariance is not addressed.
- For the physically cleaner antiferromagnetic and lattice-restricted variants, this paper leaves gaps that were filled only later by [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes|later work in the same line]] and by the follow-up Piddock–Montanaro results.

So this is a classification of the unrestricted interaction-language problem, not yet the final word on every condensed-matter variant one might care about.

---

## Reusable ideas

1. **[[Normal Form for 2-Qubit Interactions via Pauli Correlation Matrix]]** — reduce arbitrary 2-qubit interactions to a tiny set of canonical forms using the Pauli correlation matrix and an $\mathrm{SO}(3)$ rotation induced by $U^{\otimes 2}$.
2. **[[Mediator Qubit Gadget for Interaction Generation]]** — generate new effective couplings from old ones by freezing a mediator qubit and using second-order perturbation theory.
3. **[[Symmetry-Breaking Encoding via Degenerate Ground Spaces]]** — when the available interaction has too much symmetry, encode logical qubits into a degenerate ground space where that symmetry is broken enough to recover a general effective Hamiltonian.

---

## References within this paper

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — the original QMA-completeness of local Hamiltonian.
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe-Regev (2003)]] — 3-local QMA-completeness via heavy-penalty perturbation.
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|Kempe-Kitaev-Regev (2006)]] — 2-local QMA-completeness and the perturbative gadget toolkit.
- Oliveira–Terhal (2008) — perturbative simulation framework and square-lattice hardness machinery.
- Schuch–Verstraete (2009) — QMA-hardness for Heisenberg/XY-type models with arbitrary local fields.
- Biamonte–Love (2008) — earlier gadget-based hardness results for restricted 2-local interactions.
- Bravyi–Hastings (2014) — transverse Ising / stoquastic hardness that identifies the StoqMA corner precisely.
- Schaefer (1978) and later CSP dichotomy work — classical template for the classification philosophy.
- Piddock–Montanaro — later work handling sign-restricted and lattice-restricted variants left open here.

---

## Cross-links

### Paper notes
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes]]
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]]

### Trick cards
- [[Normal Form for 2-Qubit Interactions via Pauli Correlation Matrix]]
- [[Mediator Qubit Gadget for Interaction Generation]]
- [[Symmetry-Breaking Encoding via Degenerate Ground Spaces]]
- [[Perturbation Gadgets for Locality Reduction]]
- [[Perturbation Lemma for Locality Reduction]]
- [[Subdivision Gadget for k-to-2-Local Reduction]]
