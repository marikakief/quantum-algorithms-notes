> **Source:** Jan Lukas Bosse, Andrew M. Childs, Charles Derby, Filippo Maria Gambetta, Ashley Montanaro, Raul A. Santos, *Efficient and Practical Hamiltonian Simulation from Time-Dependent Product Formulas*, arXiv:2403.08729, Nature Communications **16**, 2673 (2025)
> **Links:** [arXiv](https://arxiv.org/abs/2403.08729) · [Journal](https://doi.org/10.1038/s41467-025-57580-5)
> **Tags:** #hamiltonian-simulation #product-formula #interaction-picture #time-dependent #practical-algorithms #THRIFT

---

## The computational problem

Simulate $e^{-it(H_0 + \alpha H_1)}$ where $\alpha \ll 1$ and $e^{-itH_0}$ can be implemented exactly (or very cheaply) for any $t$. The goal: exploit the energy-scale separation to get better error scaling in $\alpha$ than standard [[Product Formulas|Trotter formulas]], while keeping the implementation simple (no ancillae, no block encodings, no LCU).

This covers the practically common case of a dominant "easy" part (e.g., a diagonal or single-qubit Hamiltonian) plus a weaker perturbation (e.g., interactions).

## What the paper does

Introduces **THRIFT** (Trotter Heuristic Resource Improved Formulas for Time-dynamics): a family of [[Product Formulas|product formula]] algorithms that move to the interaction picture with respect to $H_0$, then apply Trotter decompositions to the interaction-picture Hamiltonian $H_1(t) = e^{itH_0} H_1 e^{-itH_0}$, which has effective norm $\alpha \|H_1\|$ instead of $\|H_0 + \alpha H_1\|$.

The base result (**THRIFT**): a first-order product formula with error $O(\alpha^2 t^2)$ instead of $O(\alpha t^2)$ — a factor $\alpha$ improvement. Higher-order THRIFT via Suzuki recursion gives $O(\alpha^2 t^{k+1})$.

Two extensions achieve $O(\alpha^{k+1} t^{k+1})$ for any $k$:
- **Magnus-THRIFT**: approximate the time-ordered exponential via the Magnus expansion truncated at order $k$, then Trotterise the Magnus terms
- **Fer-THRIFT**: decompose the time-ordered exponential as a product of ordinary exponentials via the Fer expansion

The paper proves $\alpha^2$ is **optimal** for any product formula built from time-ordered evolutions of Hamiltonian terms — you can't beat THRIFT's $\alpha$-scaling within this framework.

Extensive numerics on transverse-field Ising (1D and 2D), Heisenberg with random fields (1D), and Fermi-Hubbard (1D) show THRIFT substantially outperforms standard Trotter for spin models, but the advantage shrinks for Fermi-Hubbard because the interaction-picture terms become more non-local.

## The algorithm / construction

### THRIFT (first order)

Start from the interaction-picture identity:
$$e^{-it(H_0 + \alpha H_1)} = e^{-itH_0} \, \mathcal{T} e^{-i\alpha \int_0^t H_1(\tau) d\tau}$$

where $H_1(\tau) = e^{i\tau H_0} H_1 e^{-i\tau H_0}$.

Split $H_1 = \sum_\gamma H_1^\gamma$ and apply a first-order time-dependent Trotter formula to the time-ordered exponential:
$$U_{\text{apx}}(t) = e^{-itH_0} \prod_\gamma \left( e^{itH_0} e^{-it(H_0 + \alpha H_1^\gamma)} \right).$$

Each factor $e^{itH_0} e^{-it(H_0 + \alpha H_1^\gamma)}$ implements the time-ordered evolution under $\alpha H_1^\gamma(\tau)$ exactly. The formula is just a standard first-order Trotter splitting of $H = H_0 + \alpha \sum_\gamma H_1^\gamma$ using the unusual summands $\{H_0 + \alpha H_1^\gamma\}$ and $\{-H_0\}$.

**Error (Theorem 1):**
$$\|U(t) - U_{\text{apx}}(t)\| \leq \alpha^2 \int_0^t dv \int_0^v ds \sum_{\gamma_1 < \gamma_2} \|[H_1^{\gamma_1}(s), H_1^{\gamma_2}(v)]\| = O(\alpha^2 t^2).$$

### Higher-order THRIFT (Proposition 2)

Any $k$-th order Suzuki formula $S_k(t) = \prod_j S_2(u_j t)$ induces a $k$-th order THRIFT formula via:
$$\mathcal{S}_k(t) = \prod_j U_{\text{apx}}(u_j t / 2) \, U_{\text{apx}}^\dagger(-u_j t / 2)$$

with error $O(\alpha^2 t^{k+1})$. The $\alpha^2$ factor is inherited from the interaction-picture structure.

### Magnus-THRIFT (Theorem 4)

Approximate $\mathcal{T} e^{-i\alpha \int_0^t H_1(\tau) d\tau} \approx e^{\Omega^{[k]}(\alpha, t)}$ where $\Omega^{[k]} = \sum_{j=1}^k \alpha^j \tilde{\Omega}_j(t)$ is the truncated Magnus expansion. Then implement $e^{\Omega^{[k]}}$ via a $k$-th order product formula. Error: $O((\alpha t)^{k+1})$.

The Magnus terms involve nested integrals of commutators. For the second-order Magnus, the effective Hamiltonian at each time step is:
$$\Omega^{[2]}(t, \delta t) = -i\alpha \delta t \left( \sum_q A_q(t, \delta t) O_q + \sum_{q>p} B_{qp}(t, \delta t) [O_q, O_p] \right)$$

where $A_q$ and $B_{qp}$ are time-averaged coefficients computed classically.

### Fer-THRIFT (Corollary 7)

The Fer expansion factors the time-ordered exponential as a *product* of ordinary exponentials:
$$\mathcal{T} e^{-i\alpha \int_0^t H_1(\tau) d\tau} = \prod_{j=0}^{k-1} e^{-i \int_0^t A_j(s) ds} \cdot V_k$$

where $A_0 = \alpha H_1$ and $A_{j+1}$ is defined recursively from commutators of $A_j$. Error: $O(\alpha^{2^k} t^{2^{k+1}-1})$ if the exponentials are implemented exactly; in practice, same scaling as Magnus-THRIFT.

### Optimality of $\alpha^2$ (Appendix A.3)

Any product formula built from time-ordered evolutions $\mathcal{T} e^{-i \int_a^b H_\gamma(\tau) d\tau}$ for subsets of Hamiltonian terms has error $\Omega(\alpha^2)$. This is a structural limitation of the product-formula-in-interaction-picture approach. Magnus/Fer bypass this by stepping outside the product-of-time-ordered-exponentials framework.

## Key results

**Theorem 1 (THRIFT):** Error $O(\alpha^2 t^2)$ for first-order, factor $\alpha$ better than standard Trotter.

**Proposition 2 (Higher-order THRIFT):** Error $O(\alpha^2 t^{k+1})$ for $k$-th order Suzuki recursion.

**Theorem 4 (Magnus-THRIFT):** Error $O((\alpha t)^{k+1})$ for $k$-th order Magnus approximation.

**Theorem 10 (Optimality):** $\alpha^2$ scaling is optimal among all product formulas of time-ordered evolutions.

**Numerical headlines:**
- 1D transverse-field Ising ($L = 100$, $T = L$, $\alpha = 0.125$): THRIFT 4 needs $\sim 5\times$ fewer 2-qubit gates than Trotter 4
- 1D Heisenberg ($L = 100$): similar advantage
- Fermi-Hubbard: THRIFT advantage disappears because $H_1(t)$ becomes 4-local (vs 2-local for spin models), tripling the per-step gate cost

## Comparison with prior work

| Method | Error scaling | Ancillae? | Practical regime |
|---|---|---|---|
| Standard Trotter ($k$-th order) | $O(\alpha t^{k+1})$ | No | General purpose |
| **THRIFT ($k$-th order)** | $O(\alpha^2 t^{k+1})$ | No | $\alpha \lesssim 1$, spin models |
| **Magnus-THRIFT** | $O((\alpha t)^{k+1})$ | No | $\alpha \ll 1$, small systems |
| LCU / interaction picture ([[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes\|Low-Wiebe 2018]]) | $O(\alpha T \cdot \text{polylog}(\alpha T/\varepsilon))$ | Yes | Asymptotically optimal |
| Corrected product formulas ([[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes\|Bagherimehrab et al. 2024]]) | $O(\alpha^2 t^{2k+1})$ | No | Same $\alpha^2$ scaling; uses $e^{At}, e^{Bt}$ separately |
| [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes\|Haah-Hastings-Kothari-Low 2021]] | $\tilde{O}(nT)$ | Yes | Geometrically local, any $\alpha$ |

The key practical insight: THRIFT has the *same circuit depth per step* as standard Trotter when $H_0$ is 1-local (so $H_1(t)$ preserves locality), but with better error. For models where the interaction picture increases locality (Fermi-Hubbard), the advantage is lost to higher per-step costs.

## Limits / caveats

- The advantage requires $e^{-itH_0}$ to be implementable *exactly* at no cost scaling with $t$. This is satisfied when $H_0$ is diagonal or single-qubit, but not for general $H_0$.
- For Fermi-Hubbard, the interaction-picture Hamiltonian $H_1(t)$ has 4-local terms even though $H_1$ is 2-local, because $H_0$ (the on-site interaction) doesn't preserve the hopping terms' locality. This triples the per-step gate cost and wipes out the $\alpha$ advantage.
- Magnus-THRIFT 2 is only competitive for $\alpha < 10^{-2}$ — a pretty small perturbation. And the Magnus terms involve commutators that may require more complex circuits.
- The optimality proof (Theorem 10) applies only to product formulas built from time-ordered evolutions of Hamiltonian terms. Non-product-formula methods (LCU, QSP) aren't bound by this.
- All numerical benchmarks are for worst-case operator norm error or average fidelity on random states — performance on physically relevant states could be different.

## Reusable ideas

1. **[[Interaction-Picture Product Formula (THRIFT)]]** — move to the interaction picture w.r.t. the "easy" part of the Hamiltonian, then Trotterise the interaction-picture Hamiltonian. Gets a free factor of $\alpha$ in the error because the effective norm is $\alpha \|H_1\|$ instead of $\|H_0 + \alpha H_1\|$.

2. **[[Magnus Expansion for Time-Ordered Product Formulas]]** — approximate the time-ordered exponential via the Magnus expansion $e^{\Omega^{[k]}}$, where the Magnus terms are nested commutator integrals. Achieves $O((\alpha t)^{k+1})$ error, breaking the $\alpha^2$ barrier of plain THRIFT.

3. **[[Locality Preservation under Interaction Picture]]** — when $H_0$ is 1-local (diagonal, single-qubit), the interaction-picture Hamiltonian $H_1(t) = e^{iH_0 t} H_1 e^{-iH_0 t}$ preserves the locality of $H_1$. This means THRIFT circuits have the same depth per step as standard Trotter. When $H_0$ is $k$-local with $k > 1$, locality can increase and the advantage is lost.

## References within this paper

- **[[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]]** — the LCU-based interaction-picture approach; THRIFT's product-formula analogue for the same setting
- **[[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]]** — commutator-scaling Trotter error bounds; the commutator analysis here builds on this framework
- **[[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. (2025)]]** — optimised 8th-order formula used as a baseline in the numerics
- **[[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes|Haah-Hastings-Kothari-Low (2021)]]** — cited for the locality-based simulation approach
- **Omelyan et al. (2003)** — optimised 4th-order formulas for perturbative Hamiltonians ("small A" formula); outperforms THRIFT for Fermi-Hubbard
- **Fer (1958)** — the Fer expansion for time-ordered exponentials as products of ordinary exponentials
- **Magnus (1954)** — the Magnus expansion relating time-ordered to ordinary exponentials

## Cross-links

### Paper notes
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — LCU approach to the same interaction-picture setting; asymptotically better but uses ancillae
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — commutator-scaling framework that underlies THRIFT's error analysis
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — same $\alpha^2$ error scaling via a different mechanism (boundary-injected commutator correctors)
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]] — the optimised 8th-order formula used as baseline
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — time-dependent product formulas; THRIFT extends this to the interaction-picture setting
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]] — commutator product formulas; the Magnus terms in Magnus-THRIFT involve similar commutator structures
- [[Efficient Product Formulas for Commutators and Applications to Quantum Simulation (Chen-Childs-Hafezi-Jiang-Kim-Xu 2022) — Paper Notes]] — related: exploiting commutator structure in product formulas

### Trick cards
- [[Interaction-Picture Product Formula (THRIFT)]]
- [[Magnus Expansion for Time-Ordered Product Formulas]]
- [[Locality Preservation under Interaction Picture]]
- [[Trotter Commutator-Scaling Bound]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
