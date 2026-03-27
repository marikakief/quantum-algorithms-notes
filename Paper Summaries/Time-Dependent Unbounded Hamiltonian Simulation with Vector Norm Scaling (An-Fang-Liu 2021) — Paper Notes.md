> **Source:** Dong An, Di Fang, and Jin-Peng Liu, *Time-dependent unbounded [[Hamiltonian simulation]] with vector norm scaling*, arXiv:2012.13105, Quantum **5**, 459 (2021)
> **Links:** [arXiv](https://arxiv.org/abs/2012.13105) · [Quantum](https://quantum-journal.org/papers/q-2021-05-26-459/)
> **Tags:** #hamiltonian-simulation #trotter #product-formulas #time-dependent #unbounded-operators #vector-norm #commutator-scaling #Schrödinger #PDE

---

## The computational problem

Simulate the time-dependent Schrödinger equation

$$i\partial_t |\psi(t)\rangle = H(t)|\psi(t)\rangle, \quad t \in [0,T],$$

where $H(t) = f_1(t)H_1 + f_2(t)H_2$ with $H_1, H_2$ time-independent Hermitian, $f_1, f_2$ smooth scalar functions. The setting of interest: $H_1$ approximates an unbounded operator (like $-\Delta$ on a grid), so $\|H_1\| \to \infty$ as the discretisation refines. $H_2$ stays bounded (e.g., a potential $V(x)$).

The usual operator-norm error $\|\hat{U}(T) - U(T)\|$ scales badly with $\|H_1\|$ for Trotter methods. But operator-norm error is worst-case over *all* input vectors. The question: can we bound the vector-norm error $\|\hat{U}(T)|\psi_0\rangle - U(T)|\psi_0\rangle\|$ for a *specific* initial state, and does this remove the $\|H_1\|$ dependence?

## What the paper does

Proves that under suitable regularity assumptions on $|\psi_0\rangle$ (specifically, that $\|H_1|\psi(t)\rangle\|$ stays bounded along the exact solution trajectory), Trotter methods for time-dependent Hamiltonians have vector-norm error bounds where $\|H_1\|$ is replaced by $\|H_1|\psi(t)\rangle\|$ — which can be much smaller. The Trotter step count to achieve error $\varepsilon$ may not grow at all as $\|H_1\| \to \infty$.

This extends a classical numerical-analysis result of Jahnke and Lubich (BIT Numer. Math. 2000) from the time-independent to the time-dependent setting. The paper also provides the first rigorous [[Trotter Commutator-Scaling Bound|commutator-scaling]] analysis for time-dependent Trotter and generalised (Huyghebaert-De Raedt) Trotter methods.

## The two Trotter variants

### Standard Trotter

On each time segment $[t_l, t_{l+1}]$ of length $h$, approximate:

$$U_{\text{s},p}(t_{l+1}, t_l) = \prod_{\text{ordered}} e^{-i\alpha_j f_{\sigma(j)}(t_l) H_{\sigma(j)} h}$$

where $\alpha_j, \sigma(j)$ encode the $p$-th order Suzuki formula, and $f(t)$ is frozen at $t = t_l$.

### Generalised Trotter (Huyghebaert-De Raedt)

Instead of freezing $f(t)$, each factor uses the *integrated* coefficient:

$$U_{\text{g},p}(t_{l+1}, t_l) = \prod_{\text{ordered}} \exp\!\left(-iH_{\sigma(j)} \int_{t_l}^{t_{l+1}} \alpha_j f_{\sigma(j)}(\tau)\,d\tau\right)$$

This absorbs time-variation into the integration, so the error from $f'(t)$ disappears at leading order.

## Key results

### Operator-norm bounds (Theorems 1 and 2)

Local (single-step) error for first-order:

| Method | Leading local error |
|---|---|
| Standard Trotter | $\alpha_{\text{s},1} h^2 + \beta_{\text{s},1} h^3$, with $\alpha_{\text{s},1} \sim \|f_1'\|_\infty\|H_1\| + \|f_2'\|_\infty\|H_2\| + \|f_1\|_\infty\|f_2\|_\infty\|[H_1,H_2]\|$ |
| Generalised Trotter | $\alpha_{\text{g},1} h^2$, with $\alpha_{\text{g},1} = \tfrac{1}{2}\|f_1\|_\infty\|f_2\|_\infty\|[H_1,H_2]\|$ |

The generalised Trotter is strictly better: its leading error depends only on the commutator, not on derivatives of $f$ or $\|H_1\|$ individually. This is the commutator scaling.

For second order, the pattern continues: generalised Trotter has error $\alpha_{\text{g},2} h^3$ with $\alpha_{\text{g},2}$ depending on nested commutators $\|[H_1,[H_1,H_2]]\|$ and $\|[H_2,[H_1,H_2]]\|$.

Global bounds follow by standard stability accumulation over $L = T/h$ steps.

### Vector-norm bounds (Theorems 3 and 4) — the main contribution

Under **Assumption 1** (see below), for any vector $v$:

| Method | Order | Local vector-norm error |
|---|---|---|
| Standard Trotter | 1st | $\leq \tilde{C} h^2 (\|H_1 v\| + \|v\|)$ |
| Generalised Trotter | 1st | $\leq \tilde{C} h^2 (\sqrt{\|v\|\|H_1 v\|} + \|v\|)$ |
| Standard Trotter | 2nd | $\leq \tilde{C} h^3 (\|H_1 v\| + \|v\|)$ |
| Generalised Trotter | 2nd | $\leq \tilde{C} h^3 (\|H_1 v\| + \|v\|)$ |

Here $\tilde{C}$ depends on $\|H_2\|$, $\|[H_1,H_2]\|$, derivatives of $f$, and Assumption 1 constants — but **not on $\|H_1\|$**.

The key shift: $\|H_1\|$ (the operator norm, scaling as $O(n^2)$ for a Laplacian on $n$ grid points) is replaced by $\|H_1 v\|$ (the vector norm, which is $O(1)$ for smooth initial states since $\|(-\Delta)\psi\| = \|\psi''\|_{L^2}$ depends on regularity, not grid size).

**Global vector-norm bounds** (Theorem 4): for $L$ Trotter steps over $[0,T]$,

$$\left\|\prod_{l=1}^L U_{\text{s},1} |\psi_0\rangle - U(T,0)|\psi_0\rangle\right\| \leq \tilde{C}\frac{T^2}{L}\left(\sup_{t \in [0,T]}\|H_1|\psi(t)\rangle\| + \|\psi_0\|\right).$$

So the number of Trotter steps scales as $L = O(T^2 \sup_t \|H_1|\psi(t)\rangle\| / \varepsilon)$, **independent of $\|H_1\|$**.

### The Schrödinger example (Section 6)

For $H_1 = -\Delta$ (Laplacian on $n$ grid points) and $H_2 = V(x)$ (bounded potential):

- Operator norm: $\|H_1\| = O(n^2)$
- Commutator: $\|[H_1, H_2]\| = O(n^2)$ (same order — commutator scaling doesn't help by itself here)
- But $\|H_1|\psi(t)\rangle\| = O(1)$ for smooth solutions, by Sobolev regularity

So the operator-norm bound says $L = O(n^2 T^2/\varepsilon)$ for first-order Trotter, while the vector-norm bound says $L = O(T^2/\varepsilon)$ — a factor of $n^2$ improvement, and the cost becomes *independent of grid resolution*.

The generalised first-order Trotter does even better via the geometric mean: $\sqrt{\|\psi\|\|H_1\psi\|}$ instead of $\|H_1\psi\|$.

## Assumption 1 — when vector-norm scaling works

The paper requires (**Assumption 1**):

For an operator $A$ and a vector $v$, there exist constants $C_1, C_2$ such that for all $s$ sufficiently small:

$$\|A e^{-isA} v\| \leq C_1 \|Av\| + C_2 \|v\|.$$

This says: applying $A$ after a short propagation by $e^{-isA}$ doesn't blow up the $A$-norm of $v$ beyond what it was initially. For self-adjoint $A$, spectral theory gives $\|Ae^{-isA}v\| = \|Av\|$ (exact equality, since $e^{-isA}$ commutes with $A$), so Assumption 1 holds trivially with $C_1 = 1, C_2 = 0$.

For the generalised Trotter (where you need $\|A e^{-isB}v\|$ with $B \neq A$), the condition is harder to check and depends on the structure of $A$ and $B$. The paper verifies it for the Schrödinger case $A = -\Delta, B = V(x)$ under smoothness conditions on $V$.

## Comparison with prior work

| Method | Grid dependence | Error type | Notes |
|---|---|---|---|
| Standard Trotter (operator norm) | $O(n^2)$ in step count | Worst-case | Classical bound |
| [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes\|Childs et al. (2021)]] commutator scaling | Commutator $\|[H_1,H_2]\|$ | Worst-case | For $-\Delta + V$, commutator is still $O(n^2)$, so no win here |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Berry et al. (2020)]] L1-norm rescaled Dyson | $O(n^2)$ via $\|H\|_{\max}$ | Worst-case | Optimal time-dependence but still norm-dependent |
| [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes\|Low-Wiebe (2019)]] interaction picture | Can reduce if $\|B\| \ll \|A\|$ | Worst-case | Different mechanism: norm splitting |
| **This paper** (vector norm) | $O(1)$ for smooth initial states | State-dependent | Requires regularity; not worst-case |
| Jahnke-Lubich (2000) | $O(1)$ for smooth states | State-dependent | Time-independent only |
| [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes\|An-Fang-Lin (2022)]] qHOP superconvergence | $O(\log n)$ | Worst-case (!) | Different mechanism: pseudo-differential cancellation |

The relationship with qHOP (the same group's later paper) is worth noting: qHOP achieves $\log n$ dependence *in operator norm* for Schrödinger, while this paper achieves $O(1)$ dependence in *vector norm*. The mechanisms are different. Vector-norm scaling is about the state being smooth; qHOP's superconvergence is about the operator structure (differential vs. multiplication).

## Limits / caveats

1. **State-dependent bounds.** The bound depends on $\sup_t \|H_1|\psi(t)\rangle\|$ — a property of the *exact* solution. If the exact solution develops sharp features (high-frequency content), $\|H_1|\psi(t)\rangle\|$ grows and the advantage shrinks. The bound is only useful when the solution stays regular.

2. **Not a free lunch for all problems.** For problems where the solution genuinely develops fine-scale structure (e.g., quantum quenches that generate entanglement at all scales), the vector norm can grow with $n$ and the bound reverts to the operator-norm one.

3. **Smoothness of $f_1, f_2$ required.** The time-dependent coefficients need sufficient regularity for the error bounds. Discontinuous switching would break the analysis.

4. **Only first and second order.** The paper proves results for $p = 1, 2$ Trotter. Higher-order Suzuki formulas are left open. The proof technique (variation-of-constants + stability) should extend but the algebra gets heavy.

5. **Assumption 1 must be verified case-by-case.** For $A = -\Delta, B = V(x)$ with smooth $V$, it holds. For more exotic Hamiltonians, checking Assumption 1 is nontrivial.

6. **Two terms only.** The Hamiltonian is restricted to $H(t) = f_1(t)H_1 + f_2(t)H_2$. Multi-term extensions are not treated.

## Reusable ideas

1. [[Vector Norm Error Analysis for Product Formulas]] — the key technique: bound Trotter error per-vector rather than in operator norm, replacing $\|H\|$ with $\|H|\psi\rangle\|$ via variation-of-constants and stability arguments. Applicable whenever the initial state has better regularity than the worst case.

2. [[Generalised Trotter for Time-Dependent Hamiltonians]] — integrate the time-dependent coefficients into each exponential factor instead of freezing them. Eliminates the $f'(t)$ dependence at leading order and achieves commutator scaling automatically for time-dependent simulations.

## References within this paper

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2021)]] — commutator-scaling Trotter bounds (time-independent). This paper extends and complements their results.
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes|Berry-Childs-Su-Wang-Wiebe (2020)]] — L1-norm scaling for time-dependent simulation via rescaled Dyson series
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2019)]] — truncated Dyson series for time-dependent Hamiltonians
- Jahnke and Lubich (2000) — the time-independent vector-norm error analysis that this paper generalises. *BIT Numer. Math.* 40(4), 735–744.
- Huyghebaert and De Raedt (1990) — introduced generalised Trotter for time-dependent systems. *J. Phys. A* 23(24), 5777–5793.
- Descombes and Thalhammer (2010) — prior time-dependent Trotter error bounds in the semiclassical regime
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2007)]] — sparse [[Hamiltonian simulation]] foundations
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — QSVT framework
- Şahinoğlu and Somma (2020) — related low-energy subspace simulation, arXiv:2006.02660

## Cross-links

### Paper notes
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]] — first application of the [[Generalised Trotter for Time-Dependent Hamiltonians|generalised Trotter]] to time-dependent simulation; this paper extends it with vector-norm error analysis
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]]
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes]]
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]]
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]]

### Trick cards
- [[Vector Norm Error Analysis for Product Formulas]]
- [[Generalised Trotter for Time-Dependent Hamiltonians]]
- [[Trotter Commutator-Scaling Bound]]
- [[Finite Nested-Commutator Expansion]]
- [[Superconvergence via Pseudo-Differential Commutator Cancellation]]
