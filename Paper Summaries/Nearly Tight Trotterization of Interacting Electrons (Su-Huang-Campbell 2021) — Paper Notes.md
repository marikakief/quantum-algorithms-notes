> **Source:** Yuan Su, Hsin-Yuan Huang, Earl T. Campbell, *Nearly tight Trotterization of interacting electrons*, Quantum **5**:495, 2021; arXiv:2012.09194  
> **Links:** [arXiv](https://arxiv.org/abs/2012.09194) · [Quantum](https://doi.org/10.22331/q-2021-07-05-495)  
> **Tags:** #trotter #product-formulas #electronic-structure #fermionic #commutators #hamiltonian-simulation #plane-wave #Fermi-Hubbard

---

## The computational problem

Simulate the time evolution $e^{-iHt}$ of an interacting-electronic Hamiltonian in second quantisation:

$$H = T + V := \sum_{j,k} \tau_{j,k} A_j^\dagger A_k + \sum_{l,m} \nu_{l,m} N_l N_m$$

where $A_j^\dagger, A_k$ are fermionic creation/annihilation operators, $N_l = A_l^\dagger A_l$ are number operators, and the system has $n$ spin orbitals with $\eta$ electrons. The two main instances are:

- **Plane-wave-basis electronic structure** (Coulomb Hamiltonian with kinetic + electron-electron + electron-nuclear terms)
- **Fermi-Hubbard model** (nearest-neighbour hopping + on-site interaction)

The question: how many Trotter steps (and gates) are needed when we account for commutativity, interaction sparsity, *and* the initial state being in the $\eta$-electron manifold?

---

## What the paper does

Proves nearly tight Trotter error bounds for electronic Hamiltonians by simultaneously exploiting three structures that prior work could only use one at a time: (1) commutativity between Hamiltonian terms (via [[Trotter Commutator-Scaling Bound|nested commutators]]), (2) sparsity of the interaction graph, and (3) restriction to the $\eta$-electron manifold. The result is a gate complexity of

$$\left(\frac{n^{5/3}}{\eta^{2/3}} + n^{4/3}\eta^{2/3}\right) n^{o(1)}$$

for second-quantised plane-wave electronic structure — improving all prior second-quantised results up to negligible factors, and beating first-quantised methods when $n = \eta^{2 - o(1)}$.

The bounds are shown to be nearly tight via explicit lower-bound constructions, so the analysis cannot be improved by more than an $n\eta$ factor (or $\eta$ factor for sparse interactions).

---

## The algorithm / construction

The simulation itself is standard $p$-th order [[Product Formulas|Trotterisation]] applied to the two-term split $H = T + V$. What's new is entirely in the *error analysis*.

### The fermionic seminorm

Rather than bounding Trotter error in spectral norm (which ignores that the state lives in the $\eta$-electron subspace), the paper introduces the **fermionic $\eta$-seminorm**:

$$\|X\|_\eta := \max_{|\psi_\eta\rangle, |\phi_\eta\rangle} |\langle \phi_\eta | X | \psi_\eta \rangle|$$

where $|\psi_\eta\rangle, |\phi_\eta\rangle$ are states with exactly $\eta$ electrons. This is equivalent to the projected spectral norm $\|X \Pi_\eta\|$ (Proposition 5). It captures how much of the error operator the physical state actually "sees."

### Two approaches to bounding the seminorm

Starting from the [[Trotter Commutator-Scaling Bound|commutator representation of Trotter error]] (Proposition 1 from [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu 2019]]):

$$\|S_p(t) - e^{-iHt}\|_\eta = O\!\left(\max_\gamma \|[H_{\gamma_{p+1}}, \cdots [H_{\gamma_2}, H_{\gamma_1}}]\|_\eta \cdot t^{p+1}\right)$$

the technical challenge reduces to bounding the fermionic seminorm of nested commutators of $T$ and $V$.

**Approach 1 — Recursive bound (proves the non-sparse result):**
- Rewrite $\|X\|_\eta = \max_{|\psi_\eta\rangle} \sqrt{\langle \psi_\eta | X^\dagger X | \psi_\eta \rangle}$ and bound $X^\dagger X$ via the positive-semidefinite ordering
- Contract summation indices using an operator Cauchy-Schwarz inequality and diagonalisation
- Extend to higher-order products via a Hölder-type inequality for fermionic expectations
- Apply recursively through the nested commutator layers

**Approach 2 — Path counting (proves the sparse result):**
- Expand the nested commutator and the $\eta$-electron state in the computational basis
- Count the number of "paths" (sequences of creation/annihilation operations) with nonzero contribution
- For $d$-sparse interactions, the path count is controlled by $d^{p+1}\eta$ rather than $n^{p+1}\eta$

---

## Key results

**Theorem 1 (Fermionic seminorm of Trotter error).** For a $p$-th order formula:

$$\|S_p(t) - e^{-iHt}\|_\eta = O\!\left((\|\tau\| + \|\nu\|_{\max} \eta)^{p-1} \|\tau\| \|\nu\|_{\max} \eta^2 t^{p+1}\right)$$

For $d$-sparse interactions:

$$\|S_p(t) - e^{-iHt}\|_\eta = O\!\left((\|\tau\|_{\max} + \|\nu\|_{\max})^{p-1} \|\tau\|_{\max} \|\nu\|_{\max} d^{p+1} \eta \, t^{p+1}\right)$$

No dependence on $n$ in either bound. The first depends on $\eta$ through the seminorm restriction; the second depends on sparsity $d$ instead of $n$.

**Theorem 2 (Tightness).** There exist electronic Hamiltonians saturating these bounds up to factors of $n\eta$ (non-sparse) or $\eta$ (sparse). For large $p$, these gaps become $n^{o(1)}$ and $\eta^{o(1)}$ respectively.

### Application: plane-wave electronic structure

| Method | Gate complexity | Regime $\eta = \Theta(n)$ |
|---|---|---|
| Interaction-picture, 1st quantisation [Babbush et al.] | $\tilde{O}(n^{1/3}\eta^{8/3})$ | $\tilde{O}(n^3)$ |
| Qubitization, 1st quantisation [Babbush et al.] | $\tilde{O}(n^{2/3}\eta^{4/3} + n^{1/3}\eta^{8/3})$ | $\tilde{O}(n^3)$ |
| Interaction-picture, 2nd quantisation [Su et al.] | $\tilde{O}(n^{8/3}/\eta^{2/3})$ | $\tilde{O}(n^2)$ |
| Trotterisation, 2nd quant. [Babbush et al. 2018] | $(n^{5/3}\eta^{1/3} + n^{4/3}\eta^{5/3})n^{o(1)}$ | $n^{3+o(1)}$ |
| **Trotterisation, this paper** | $(n^{5/3}/\eta^{2/3} + n^{4/3}\eta^{2/3})n^{o(1)}$ | $n^{2+o(1)}$ |

### Application: Fermi-Hubbard model

$O(n\eta^{1/p})$ gates for constant $s, v, m$ — improves both the $O(n^{1+1/p})$ bound from [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran et al.]] (which ignored initial-state info) and the $O(n\eta^{1+1/p})$ bound from Şahinoğlu-Somma (which ignored commutativity).

---

## Comparison with prior work

| Feature exploited | Childs-Su-Tran-Wiebe-Zhu (2019) | Babbush et al. (2018) | Şahinoğlu-Somma (2020) | **This paper** |
|---|---|---|---|---|
| Commutativity (nested commutators) | ✓ | ✗ | ✗ | ✓ |
| Initial-state / $\eta$-electron manifold | ✗ | ✓ | ✓ | ✓ |
| Interaction sparsity | ✗ | ✗ | ✗ | ✓ |
| Tightness proof | Tight for $p=1,2$ | No | No | Nearly tight |

The key relationship to [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]]: that paper proved the general [[Trotter Commutator-Scaling Bound]] — Trotter error is controlled by nested commutators, not raw norms. This paper takes that framework and asks: within the $\eta$-electron manifold, how big are those nested commutators *really*? The answer: much smaller than the spectral norm suggests, because fermionic structure constrains which matrix elements contribute.

---

## Limits / caveats

- The $n^{o(1)}$ factor hides the cost of using high-order formulas ($5^p$ prefactor for Suzuki formulas). For practical circuit depths, low-order formulas are what you'd actually use, and the concrete prefactors matter more than the asymptotic scaling.
- The tightness gap of $n\eta$ (non-sparse) or $\eta$ (sparse) is small asymptotically but not zero. Whether it can be closed remains open.
- Applies only to the $T + V$ structure of Eq. (1) — doesn't directly handle Gaussian basis electronic structure or other Hamiltonian forms without the $[T, V]$ commutator structure.
- The comparison with first-quantised methods is conditional on the ratio $n/\eta$. When electrons are dense ($\eta \approx n$), the advantage shrinks to constant factors.
- Concrete gate counts for specific molecules (FeMoCo, etc.) would require evaluating the commutator norms numerically — the paper gives asymptotic scaling only.

---

## Reusable ideas

1. [[Fermionic Seminorm for Trotter Error]] — restrict Trotter error analysis to the $\eta$-electron manifold via a seminorm that equals the projected spectral norm; decouples error from total Hilbert space dimension
2. [[Recursive Operator Cauchy-Schwarz for Fermionic Bounds]] — bound $\|X\|_\eta$ by converting $X^\dagger X$ into a PSD upper bound via operator Cauchy-Schwarz + diagonalisation + a Hölder-type inequality, applied recursively through nested commutator layers
3. [[Fermionic Path Counting for Sparse Interactions]] — combinatorial argument bounding the number of nonzero paths through sparse interaction graphs; gives $d$-dependent rather than $n$-dependent bounds

---

## References within this paper

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — the general commutator-scaling framework that this paper specialises to fermions
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — plane-wave dual basis Trotter step; prior second-quantised bound
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan et al. (2020)]] — concrete Trotter error bounds for jellium/Hubbard using commutator norms
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush et al. (2015)]] — empirical observation that Trotter bounds are loose by $10^{16}$; this paper provides theoretical explanation via fermionic seminorm
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran-Su-Childs-Wiebe (2021)]] — symmetry protection for Trotter; complementary technique
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean et al. (2018)]] — fermionic swap network for implementing Trotter steps
- Şahinoğlu and Somma (2020), arXiv:2006.02660 — prior low-energy Trotter bound without commutativity
- Babbush et al. (2019), arXiv:1902.10673 — first-quantised interaction-picture simulation

---

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]]
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]]
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]]
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]

### Trick cards
- [[Trotter Commutator-Scaling Bound]]
- [[Finite Nested-Commutator Expansion]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Nuclear Charge Scaling of Trotter Error]]
- [[Fermionic Seminorm for Trotter Error]]
- [[Recursive Operator Cauchy-Schwarz for Fermionic Bounds]]
- [[Fermionic Path Counting for Sparse Interactions]]
- [[Fermionic Swap Network]]
