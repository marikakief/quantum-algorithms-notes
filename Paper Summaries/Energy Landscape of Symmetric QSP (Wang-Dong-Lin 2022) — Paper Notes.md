> **Source:** Jiasu Wang, Yulong Dong, and Lin Lin, *On the energy landscape of symmetric quantum signal processing*, arXiv:2110.04993, Quantum **6**, 850 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2110.04993) · [Quantum](https://quantum-journal.org/papers/q-2022-11-03-850/)
> **Tags:** #QSP #phase-factors #optimization #nonlinear-optimization #convexity #Chebyshev

---

## The computational problem

Given a target real polynomial $f \in \mathbb{R}[x]$ with degree $d$, definite parity $(d \bmod 2)$, and $\|f\|_\infty < 1$, find **symmetric phase factors** $\Phi = (\phi_0, \phi_1, \ldots, \phi_1, \phi_0) \in [-\pi,\pi)^{d+1}$ such that

$$f(x) = \operatorname{Re}\bigl[\langle 0 | U(x,\Phi) | 0 \rangle\bigr], \quad x \in [-1,1],$$

where $U(x,\Phi) = e^{i\phi_0 Z} e^{i\arccos(x)X} e^{i\phi_1 Z} \cdots e^{i\phi_d Z}$ is the standard [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] unitary.

This is equivalent to minimising the cost function

$$F(\Phi) = \frac{1}{e_d} \sum_{k=1}^{e_d} |g(x_k, \Phi) - f(x_k)|^2$$

over symmetric phase factors, where $e_d = \lceil (d+1)/2 \rceil$ and $x_k$ are positive Chebyshev nodes of $T_{2e_d}$.

The practical importance: phase factor computation is the classical preprocessing bottleneck for all [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]-family quantum algorithms. This paper explains why a naive-looking optimization approach works so well.

---

## What the paper does

Provides the first theoretical analysis of **why optimization-based QSP phase-factor computation succeeds**. The cost function $F(\Phi)$ is non-convex with combinatorially many global minima and many local minima — yet starting from the fixed initial guess $\Phi_0 = (\pi/4, 0, \ldots, 0, \pi/4)$, standard optimisers reliably converge to a global minimum even for $d \sim 10{,}000$.

The paper identifies a special global minimum (the **maximal solution**), proves it lives near $\Phi_0$, and shows the cost function is strongly convex in that neighbourhood — when $\|f\|_\infty = O(d^{-1})$.

**My assessment:** This is a clean piece of nonlinear optimisation theory applied to a quantum computing problem. The core contribution is structural — it explains empirical success that had been unexplained since [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|Dong, Meng, Whaley & Lin (2021)]] introduced the optimisation approach in QSPPACK. The $\|f\|_\infty = O(d^{-1})$ condition is restrictive and doesn't match practice (which works up to $\|f\|_\infty$ close to 1), so the explanation is partial. But the techniques are solid and the structural insight — especially the maximal solution concept — is genuinely useful.

---

## The algorithm / construction

### Symmetric phase factors (Theorem 1)

Given polynomials $(P, Q)$ satisfying the [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] feasibility conditions — $\deg(P) = d$, $\deg(Q) = d-1$, definite parities, and $|P(x)|^2 + (1-x^2)|Q(x)|^2 = 1$ — there exist **unique** symmetric phase factors $\Phi \in \mathcal{D}_d$ producing these polynomials.

This strengthens [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019), Theorem 4]], which proves existence without symmetry. With symmetry imposed, the phase factors become unique. The proof is constructive: recursively peel off pairs of outer phases using a degree-reduction lemma (Lemma 13), extracting $\phi_\ell$ from the ratio of leading coefficients at each step.

### Characterising all global minima (Theorem 4 / Theorem 16)

All admissible complementary polynomial pairs $(P_{\mathrm{Im}}, Q)$ for a target $f$ are constructed via [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials|Laurent polynomial root grouping]]:

1. Form $F(z) = 1 - \bigl[f\bigl(\frac{z+z^{-1}}{2}\bigr)\bigr]^2$, which has $4d$ roots.
2. Select a multiset $D$ of $2d$ roots satisfying: (a) $D \cup D^{-1}$ gives all roots with multiplicity, (b) $D$ is closed under conjugation and negation.
3. Construct $P_{\mathrm{Im}}$ and $Q$ from $e(z) = z^{-d}\prod_{r \in D}(z - r)$.

Different root groupings give different global minima. The number of global minima grows combinatorially with $d$.

### The maximal solution

Choose $D$ to contain all roots of $F(z)$ **inside** the unit disc. The resulting complementary polynomials have the property that $Q$ has maximal leading coefficient magnitude among all choices (Lemma 18) — hence "maximal solution."

Key property (Remark 20): when $f = 0$, the maximal solution is exactly $\Phi_0 = (\pi/4, 0, \ldots, 0, \pi/4)$, which generates $(P, Q) = (iT_d, U_{d-1})$. So the fixed initial guess already sits at the right minimum for the trivial target.

### Distance bound (Theorem 5)

If $\|f\|_\infty \leq 1/2$, the maximal solution $\tilde\Phi^*$ satisfies

$$\|\tilde\Phi^* - \tilde\Phi_0\|_2 \leq \frac{\pi}{\sqrt{3}}\|f\|_\infty.$$

This is **independent of $d$**. The maximal solution stays close to $\Phi_0$ proportionally to the target's sup-norm, regardless of polynomial degree.

The proof uses a telescoping argument through the recursive phase extraction sequence, bounding each $|\sin(2\phi_\ell)|^2$ by the change in leading Chebyshev coefficients of $Q^{(\ell)}$, then summing the telescope.

### Local strong convexity (Theorem 6)

If $\|f\|_\infty \leq \frac{\sqrt{3}}{20\pi e_d}$, then for all symmetric $\Phi$ with $\|\tilde\Phi - \tilde\Phi_0\|_2 \leq \frac{1}{20e_d}$:

$$\frac{1}{4} \leq \lambda_{\min}\bigl(\mathrm{Hess}(\tilde\Phi)\bigr) \leq \lambda_{\max}\bigl(\mathrm{Hess}(\tilde\Phi)\bigr) \leq \frac{25}{4}.$$

The Hessian decomposes as $\frac{2}{e_d}A^\top A + R$, where $A$ is the Jacobian. At $\Phi_0$, the Hessian is exactly $4I$ (odd $d$) or $\mathrm{diag}(4,\ldots,4,2)$ (even $d$). The perturbation analysis bounds $\|R\|_F$ using a lower bound on $\|\tilde\Phi - \tilde\Phi_0\|_2$ in terms of the Chebyshev norm of $g(x,\tilde\Phi)$ (Theorem 31).

### Convergence guarantee (Corollary 7)

Under the $\|f\|_\infty = O(d^{-1})$ condition, projected gradient descent with step size $t = 4/25$ converges exponentially:

$$\|\tilde\Phi_\ell - \tilde\Phi^*\|_2^2 \leq e^{-\ell/25}\|\tilde\Phi_0 - \tilde\Phi^*\|_2^2.$$

This needs $O(\log(1/\epsilon))$ iterations, each numerically stable in standard double precision. No extended-precision arithmetic required.

---

## Key results

| Result | Statement |
|---|---|
| Uniqueness of symmetric phases (Thm 1) | For admissible $(P,Q)$, symmetric phases in $\mathcal{D}_d$ are unique |
| Bijection (Cor 3) | Global minima $\leftrightarrow$ admissible pairs (up to $\pm Q$ for even $d$) |
| All admissible pairs (Thm 4/16) | Constructed via Laurent root grouping from $F(z) = 1 - f(\frac{z+z^{-1}}{2})^2$ |
| Maximal solution distance (Thm 5) | $\|\tilde\Phi^* - \tilde\Phi_0\|_2 \leq \frac{\pi}{\sqrt{3}}\|f\|_\infty$ (degree-independent) |
| Strong convexity (Thm 6) | $\lambda_{\min}(\mathrm{Hess}) \geq 1/4$ near $\Phi_0$ when $\|f\|_\infty = O(d^{-1})$ |
| Convergence (Cor 7) | Projected GD converges in $O(\log(1/\epsilon))$ iterations, double precision |

---

## Comparison with prior work

| Method | Precision requirement | Stability | Degree limit | Provable convergence |
|---|---|---|---|---|
| [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes\|GSLW (2019)]] factorisation | $O(d\log(d/\epsilon))$ bits | Unstable | None | N/A (direct) |
| Haah (2019) product decomposition | $O(d\log(d/\epsilon))$ bits | Unstable | None | N/A (direct) |
| Capitalisation (Chao et al. 2020) | Standard (empirical) | Stable (empirical) | $d \sim 10^4$ | No |
| Prony (Ying 2022) | Standard (empirical) | Stable (empirical) | Large | No |
| QSPPACK optimisation (Dong et al. 2021) | Standard double | Stable | $d \sim 10^4$ | No (empirical) |
| **This paper** | Standard double | Stable | $\|f\|_\infty = O(d^{-1})$ | **Yes** (Cor 7) |

The key advance: first provable algorithm for QSP phase factors using standard precision. The catch: the $\|f\|_\infty = O(d^{-1})$ condition means you need to scale down $f$ for large-degree polynomials, potentially degrading query complexity of the downstream quantum algorithm.

---

## Limits / caveats

- The $\|f\|_\infty = O(d^{-1})$ condition is much more restrictive than what works empirically. In practice, optimisation converges for $\|f\|_\infty$ close to 1 (independent of $d$). The gap comes from bounding the Hessian entry-by-entry, which introduces a factor of $d$.
- The convergence guarantee requires **projected** gradient descent (projecting back onto a ball around $\Phi_0$). Empirically, unconstrained quasi-Newton methods work fine, but the paper can't prove this.
- Analysis is restricted to symmetric phase factors. The non-symmetric case has a singular Hessian at all global minima (because there are more variables than equations), making the analysis fundamentally harder.
- The paper doesn't address the complexity of the initial polynomial approximation step (finding good $f$ for a given target function). That's a separate approximation theory problem.
- Non-maximal solutions can have much slower convergence — the numerical experiments in Section 7.3 show this clearly.

---

## Reusable ideas

1. [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]] — constructing all admissible complementary polynomials by choosing which roots of $F(z)$ go inside/outside the unit disc. The inside-unit-disc choice gives the maximal solution with the best optimisation properties.

2. [[Maximal Solution Selection via Mahler Measure]] — using the Mahler measure (geometric mean on the unit circle) to prove the leading coefficient of $Q$ is lower-bounded by $\sqrt{1 - \|f\|_\infty^2}$, which keeps the maximal solution near the initial guess.

3. [[Symmetry Reduction for Phase Factor Uniqueness]] — imposing symmetric phase factors halves the degrees of freedom and makes the global minimum unique (for each admissible pair). This converts a singular optimisation problem into a non-degenerate one.

---

## References within this paper

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — original QSP framework ([14])
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén, Su, Low & Wiebe (2019)]] — QSVT existence theorem for phase factors ([6])
- Haah (2019), *Product decomposition of periodic functions in quantum signal processing*, Quantum **3**, 190 — direct (unstable) factorisation method ([8])
- Dong, Meng, Whaley & Lin (2021), *Efficient phase factor evaluation in quantum signal processing*, Phys. Rev. A **103**, 042419 — QSPPACK, optimisation-based approach, introduced $\Phi_0$ initial guess ([5])
- Chao, Ding, Gilyén, Huang & Szegedy (2020), *Finding angles for quantum signal processing with machine precision*, arXiv:2003.02831 — capitalisation method ([3])
- Ying (2022), *Stable factorization for phase factors of quantum signal processing*, Quantum **6**, 842 — Prony-based method ([19])
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn, Rossi, Tan & Chuang (2021)]] — pedagogical QSP → QET → QSVT chain ([16])
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin & Tong (2020)]] — eigenvalue filtering via QSP ([12])

---

## Cross-links

### Paper notes
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — QSP origin
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — existence theorem this paper refines
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] — introduced the optimisation approach (QSPPACK) that this paper explains
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]] — QSP tutorial
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes]] — precursor: recursive phase extraction, completion
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]] — same group (Dong, Lin); uses QSPPACK
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] — fully-coherent QSP simulation

### Trick cards
- [[Recursive Phase Extraction from Polynomial Coefficients]] — the recursive peeling used in the uniqueness proof
- [[Partial-Quadrature Completion via Root Grouping]] — related root-factorisation approach from Low-Yoder-Chuang
- [[QSP-to-QSVT Lifting Chain]] — conceptual chain that this paper's phase-factor analysis feeds into
- [[Jacobi-Anger Truncation for QSP Simulation]] — polynomial approximation that generates the target $f$
- [[Laurent Polynomial Root Grouping for QSP Complementary Polynomials]] — new trick card
- [[Maximal Solution Selection via Mahler Measure]] — new trick card
- [[Symmetry Reduction for Phase Factor Uniqueness]] — new trick card
