> **Source:** Ryan Babbush, Jarrod McClean, Dave Wecker, Alán Aspuru-Guzik, Nathan Wiebe, *Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation*, arXiv:1410.8159, Phys. Rev. A **91**, 022311 (2015)
> **Links:** [arXiv](https://arxiv.org/abs/1410.8159) · [PRA](https://doi.org/10.1103/PhysRevA.91.022311)
> **Tags:** #trotter-error #quantum-chemistry #product-formula #simulation #nuclear-charge #state-preparation #CISD

---

## The computational problem

Simulate the electronic structure Hamiltonian

$$H = \sum_{pq} h_{pq} a_p^\dagger a_q + \frac{1}{2}\sum_{pqrs} h_{pqrs} a_p^\dagger a_q^\dagger a_r a_s$$

using second-order [[Order-Condition Cancellation in Product Formulas|Trotter-Suzuki]] product formulas on a quantum computer, via phase estimation. The key question: how many Trotter steps $\mu$ are actually needed to achieve chemical accuracy ($\sim 10^{-3}$ hartree) for real molecules?

Prior work bounded the error using the operator norm $\|V^{(1)}\|$ of the leading error term. This paper shows those bounds can be off by up to **sixteen orders of magnitude**.

---

## What the paper does

Three things:

1. **Numerically demolishes** the assumption that Trotter error scales primarily with the number of spin orbitals $N$. Instead, the dominant factor is the maximum nuclear charge $Z_{\max}$ in the molecule, with error scaling as $O(Z_{\max}^6)$ in an atomic orbital basis. For molecules like O, F, Ne the actual ground-state Trotter error is negligible despite error *bounds* predicting large errors.

2. **Explains why** through analysis of the leading-order error operator (Eq. 13 from BCH expansion): the inner-shell orbitals dominate the error because $|h_{pq}| = \Theta(Z^2)$ for atomic orbitals. The ground state has limited overlap with the high-error eigenstates of $V^{(1)}$, and atoms with filled or nearly-filled shells have extra error cancellation.

3. **Proposes practical fixes**: use a classical CI ansatz to estimate (and subtract) the Trotter error from the quantum simulation result, and provides an improved state preparation circuit based on number-theoretic gate synthesis.

---

## Key findings

### Error bounds are wildly loose

The second-order Trotter error operator (leading BCH term) is:

$$V^{(1)} = -\frac{\Delta t^2}{12}\sum_{\alpha \leq \beta}\sum_\gamma^{\gamma < \beta}\left[H_\alpha\left(1 - \frac{\delta_{\alpha,\beta}}{2}\right),\, [H_\beta, H_\gamma]\right]$$

The ground-state energy error is $\Delta E_i = \langle\psi_i|V^{(1)}|\psi_i\rangle + O(\Delta t^4)$. The paper computes this exactly (not bounded) for small molecules by constructing all $O(N^{10})$ nonzero terms and normal-ordering.

| Molecule | $\|V^{(1)}\|$ (norm) | $|\Delta E_0|$ (ground state) | Ratio |
|---|---|---|---|
| H₂ | ~10⁻¹ | ~10⁻² | ~10 |
| LiH | ~10⁰ | ~10⁻¹ | ~10 |
| O (atom) | ~10³ | ~10⁻¹³ | ~10¹⁶ |
| Ne (atom) | ~10⁴ | ~10⁻¹⁵ | ~10¹⁹ |

The discrepancy for closed-shell atoms is extraordinary. The operator norm sees high-energy eigenstates of $V^{(1)}$ that the ground state has essentially zero overlap with.

### Nuclear charge scaling

For atoms in a local (atomic orbital) basis, $|h_{pq}| = \Theta(Z^2)$ and $|h_{pqrs}| = \Theta(Z)$, obtained by rescaling spatial coordinates $r \to \rho = Zr$. Since the one-body terms dominate the double commutator in $V^{(1)}$, the error operator norm scales as:

$$\|V^{(1)}\| \in O(N^4 Z_{\max}^6 + N^{10} Z_{\max}^3)$$

The $Z_{\max}^6$ scaling fits the numerical data with $r^2 = 0.994$ in the local basis. In the canonical (Hartree-Fock) basis, the fit is closer to $Z_{\max}^5$.

### Orbital basis matters

Three bases compared: local (atomic), canonical (HF molecular), natural (diagonalizes 1-RDM). The error is basis-dependent — natural orbitals often reduce it by orders of magnitude vs. local orbitals. The ratio of ground-state error to norm error shrinks as the basis grows (Table I), because additional orbitals inflate $\|V^{(1)}\|$ without proportionally affecting $\langle\psi_0|V^{(1)}|\psi_0\rangle$.

### Inner-shell orbitals dominate

Normal-ordering the error operator and binning by orbital index shows that inner-shell orbitals (closest to nuclei) produce the largest error coefficients. Valence electrons — the ones that matter for chemistry — contribute relatively little to the Trotter error. This suggests that pseudopotentials (freezing core electrons) could substantially reduce Trotter cost.

### Haar-random states see much less error

A concentration-of-measure argument shows that for a Haar-random state $|v\rangle$:

$$|\langle v|V^{(1)}|v\rangle| \in O\!\left(\frac{\sqrt{\sum_k \lambda_k^2}}{2^N}\right)$$

which vanishes with system size. Real eigenstates of chemical Hamiltonians are far from Haar-random — they concentrate on low-excitation configurations — which is precisely why they see larger (but still much less than $\|V^{(1)}\|$) errors.

---

## Classical error estimation and subtraction

### The idea

Compute $\langle\psi_{\text{CI}}|V^{(1)}|\psi_{\text{CI}}\rangle$ using a classical CI ansatz (Hartree-Fock, CISD, CISDT, CISDTQ) and subtract it from the quantum result. This:

1. Gives an a priori estimate of how many Trotter steps are needed (avoiding the massive overestimate from $\|V^{(1)}\|$)
2. Reduces the effective error in the quantum simulation — often by an order of magnitude or more (Fig. 10)

The CISD ansatz is already good enough for most molecules tested. The approach doesn't require additional quantum resources — it's purely classical post-processing.

### State preparation circuit

The paper also gives an improved circuit for preparing a CISD initial state $|\psi_{\text{CISD}}\rangle$ using number-theoretic gate synthesis (à la [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Kliuchnikov-Maslov-Mosca]]). The state is a superposition over $D = O(N^4)$ Slater determinants (for CISD). The preparation proceeds by:

1. Iteratively rotating each amplitude into the $|0\rangle$ component of a 2D subspace
2. Using multiply-controlled H and T gates, synthesized via least-denominator-exponent reduction
3. Total T-count: $O(D\log(D/\delta) + ND)$ where $\delta$ is the synthesis precision

This is asymptotically better than the Ortiz et al. method ($\tilde{O}(D^2 N^2 \log(1/\delta))$) and the Wang et al. approach (which scales exponentially in electron number $n_e$ when $n_e \approx N/2$).

---

## Comparison with prior/subsequent work

| Paper | What it does for Trotter error |
|---|---|
| Wecker-Bauer-Clark-Hastings-Troyer 2014 | First numerical Trotter error study for chemistry; introduced interleaving |
| Hastings-Wecker-Bauer-Troyer 2015 | Analytical norm bounds; interleaving/nesting optimizations |
| Poulin-Hastings-Wecker-Wiebe-Doherty-Troyer 2014 | BCH-based norm bounds; coalescing strategies |
| McClean-Babbush-Love-Aspuru-Guzik 2014 | Locality exploitation; super-exponential decay of integrals in local basis |
| **This paper** | Shows norm bounds are loose by up to 10¹⁶; identifies $Z_{\max}^6$ scaling; proposes classical error subtraction |
| [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes\|Childs-Su-Tran-Wiebe-Zhu 2019]] | Tightens norm bounds using nested commutators; partially closes the gap this paper identified |

This paper was one of the first to seriously question whether worst-case Trotter bounds are meaningful for quantum chemistry. The answer — they're not, at least for the second-order formula — reshaped how people think about resource estimation. The [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs et al. 2019]] commutator bounds later closed some of the gap by exploiting the same cancellation structure (nested commutators instead of raw norms), but the fundamental insight that chemical structure determines Trotter cost came from here.

---

## Limits / caveats

- All numerics are on small molecules ($\leq 20$ spin orbitals, STO-6G minimal basis). The $Z_{\max}^6$ scaling is an empirical fit, not a rigorous bound — though the analytical argument for $O(Z^6)$ from one-body integral scaling is clean.
- The error subtraction trick requires a good classical ansatz. For strongly correlated systems where CISD is poor, the subtraction may not help much.
- Only the second-order Trotter formula is studied. Higher-order Suzuki formulas have different error structures — the dominance of $Z_{\max}$ may or may not persist.
- The state preparation cost $O(N^5)$ can be comparable to or exceed the simulation cost for large $N$ at half-filling. This limits the advantage when the Hartree-Fock state has poor overlap with the ground state.
- The paper predates [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] and [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]], which avoid Trotter entirely. The Trotter error question is less urgent for fault-tolerant algorithms using these methods, but remains relevant for near-term and product-formula approaches.

---

## Reusable ideas

1. **[[Nuclear Charge Scaling of Trotter Error]]** — for electronic structure in an atomic orbital basis, one-body integrals scale as $|h_{pq}| = \Theta(Z^2)$, driving Trotter error as $O(Z_{\max}^6)$ via the BCH double-commutator structure. Nuclear charge, not orbital count, determines simulation cost.

2. **[[Classical Ansatz Error Subtraction for Quantum Simulation]]** — evaluate the Trotter error operator on a classical CI wavefunction and subtract from the quantum result. Gives both an a priori step-count estimate and a post-hoc accuracy improvement, at zero quantum cost.

---

## References within this paper

- Wecker, Bauer, Clark, Hastings, Troyer (2014), PRA 90 — introduced interleaving for Trotter ordering; first numerical Trotter benchmarks
- Hastings, Wecker, Bauer, Troyer (2015), QIC 15 — analytical Trotter bounds; nesting and interleaving
- Poulin, Hastings, Wecker, Wiebe, Doherty, Troyer (2014), arXiv:1406.4920 — BCH-based error bounds; coalescing strategies
- McClean, Babbush, Love, Aspuru-Guzik (2014), J. Phys. Chem. Lett. — locality exploitation in local basis; super-exponential integral decay
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams & Lloyd (1997)]] — original fermionic simulation proposal
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2019)]] — later tightening of Trotter error bounds via nested commutators
- Seeley, Richard, Love (2012) — Bravyi-Kitaev transformation

---

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
- [[Quantum-Circuit Design for Efficient Simulations of Many-Body Quantum Dynamics (Raeisi-Wiebe-Sanders 2012) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — randomized multi-product formulas as a follow-on strategy to close the gap between norm bounds and actual Trotter cost that this paper identified
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry kicks as another route to achieve the practical Trotter error reduction this paper documented empirically
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]] — randomized corrections that double effective product-formula order; addresses the overestimation problem this paper raised
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — deterministic corrected product formulas; achieves provable factor-$\alpha$ improvement for perturbed systems, vindicating the practical-error framing of this paper
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — finds 8th-order product formulas ~100× more accurate; directly addresses the norm-bound looseness this paper identified
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — extends the error-tightening program to condensed-phase Hamiltonians (Hubbard, jellium) and derives explicit $W$ norms; the extensive-vs-intensive error framing there is the condensed-matter analogue of the basis/state-dependence insight here
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — the split-operator Trotter step for plane-wave Hamiltonians, introduced by several of the same authors; the translation-invariant structure of the potential energy coefficients produces the same kind of systematic cancellation in the Trotter error that this paper identifies for molecular systems
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — fermionic swap network Trotter step; the ordering-dependent Trotter error (left unanalysed in that paper) is exactly the kind of problem this paper's techniques address

### Trick cards
- [[Nuclear Charge Scaling of Trotter Error]]
- [[Classical Ansatz Error Subtraction for Quantum Simulation]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Pauli-String Exponential via Parity-CNOT Ladder]]
- [[Finite Nested-Commutator Expansion]]
