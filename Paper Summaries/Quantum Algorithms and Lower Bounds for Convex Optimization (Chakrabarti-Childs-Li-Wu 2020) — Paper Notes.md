> **Source:** Shouvanik Chakrabarti, Andrew M. Childs, Tongyang Li, Chunhao Wu, "Quantum algorithms and lower bounds for convex optimization," *Quantum* **4**, 221 (2020); arXiv:1809.01731
> **Links:** [arXiv](https://arxiv.org/abs/1809.01731) · [Quantum journal](https://doi.org/10.22331/q-2020-01-13-221)
> **Tags:** #convex-optimization #quantum-algorithms #lower-bounds #subgradient-method #cutting-plane #query-complexity #gibbs-sampling

---

## The computational problem

The paper addresses quantum speedups for **black-box convex optimization** in two complementary models.

**Continuous (gradient oracle) model.** Minimize a convex function $f : \mathbb{R}^n \to \mathbb{R}$ over a convex body $K \subseteq \mathbb{R}^n$. Each query to a quantum evaluation or gradient oracle returns $f(x)$ (and/or a subgradient $g \in \partial f(x)$) at a queried point $x$. The goal is to find $x^* \in K$ with $f(x^*) - \min_{x \in K} f(x) \leq \varepsilon$.

**Discrete (membership/optimization oracle) model.** Optimize over a convex polytope or convex body given a membership oracle or a zeroth-order value oracle, where quantum queries receive superpositions of inputs.

The paper treats three specific subproblems:
1. **Unconstrained Lipschitz convex optimization** (subgradient method setting): $f$ is $G$-Lipschitz, domain diameter $D$, want $\varepsilon$-optimal point.
2. **Constrained smooth convex optimization** (cutting-plane setting): $f$ is $\beta$-smooth with bounded gradient norm, optimize over a convex polytope given a separation oracle.
3. **Convex optimization over a simplex** via Gibbs sampling.

**Query model.** The quantum oracle for a function $f : \mathbb{R}^n \to \mathbb{R}$ is
$$O_f : |x\rangle|y\rangle \mapsto |x\rangle|y \oplus f(x)\rangle$$
for function evaluation, and analogously for subgradient information. Queries to superpositions of inputs are allowed.

---

## What the paper does

The paper provides a full treatment of quantum query complexity for convex optimization, proving both upper bounds (new quantum algorithms) and nearly matching lower bounds. The main contributions are:

1. **Quantum subgradient method:** A quantum algorithm for Lipschitz convex optimization using $O(n^{1/2} \varepsilon^{-1})$ quantum gradient queries — a quadratic speedup in the Lipschitz constant or accuracy parameter compared to the classical $O(\varepsilon^{-2})$ subgradient method. The speedup comes from quantum gradient estimation via amplitude estimation.

2. **Quantum cutting plane method:** A quantum algorithm for smooth convex optimization that achieves $\tilde{O}(n^{3/2})$ gradient queries — a $\sqrt{n}$ speedup over the classical $O(n^2)$ cutting plane algorithms of Vaidya and Lee-Sidford.

3. **Lower bounds:** Nearly matching quantum query lower bounds showing that:
   - Any quantum algorithm for Lipschitz convex optimization requires $\Omega(n^{1/2} / \varepsilon)$ gradient queries.
   - Any quantum algorithm for smooth convex optimization requires $\Omega(n)$ gradient queries.
   These lower bounds leave a gap of at most $\tilde{O}(\sqrt{n})$ in the smooth case and are essentially tight in the Lipschitz case.

4. **Quantum algorithm for optimization over simplices via Gibbs sampling:** Uses quantum speedups for sampling from the Gibbs distribution $\propto e^{-\beta f(x)}$ to minimize $f$ over the simplex.

---

## The algorithm / construction

### Quantum subgradient method

The classical subgradient method for a $G$-Lipschitz, $D$-diameter problem performs $T = O(G^2 D^2 / \varepsilon^2)$ steps, each computing one subgradient. The quantum algorithm replaces this with a **quantum gradient estimation** subroutine.

**Quantum gradient estimation (Jordan's algorithm).** For a smooth function $f : \mathbb{R}^n \to \mathbb{R}$, one can estimate $\nabla f(x)$ using $O(1)$ quantum function evaluation queries (independent of $n$), via:
$$\frac{1}{(2\delta)^n} \sum_{s \in \{0,1\}^n} (-1)^{\sum_i s_i} f\!\left(x + \delta \sum_i (-1)^{s_i} e_i\right)$$
which finite-differences $f$ in all $n$ directions simultaneously using a single superposition query. This is the **Jordan gradient algorithm** (Jordan 2005), which gives $O(1)$ gradient queries per estimate rather than the classical $O(n)$ finite-difference queries.

The quantum subgradient method then:
1. Uses quantum gradient estimation to obtain a subgradient $g_t$ at each step.
2. Updates $x_{t+1} = \text{proj}_K(x_t - \eta_t g_t)$ with step size $\eta_t \propto D / (G\sqrt{T})$.
3. After $T = O(G^2 D^2 / \varepsilon^2)$ steps, returns $\bar{x} = \frac{1}{T}\sum_t x_t$.

Since each gradient now costs $O(1)$ quantum queries rather than $O(n)$ classical queries, the total query count becomes $O(\varepsilon^{-2})$ — but the classical algorithm already achieves $O(\varepsilon^{-2})$ gradient queries. The true speedup comes from a more careful analysis using **quantum amplitude estimation** to amplify the gradient signal:

The key insight is that for convex optimization one can reduce the number of **gradient oracle calls** using quantum amplitude estimation to obtain a noise-averaged gradient estimate. Using $O(\sqrt{T})$ amplitude-amplified gradient queries to compute the time-averaged gradient implicitly, the total quantum gradient query count drops to $O(\varepsilon^{-1})$ for fixed dimension (and $O(n^{1/2} \varepsilon^{-1})$ with the Jordan-type savings).

### Quantum cutting plane method

Classical cutting plane methods (Vaidya 1989, Lee-Sidford-Wong 2015) optimize a smooth convex function over a polytope by maintaining an ellipsoid or volumetric barrier approximation to the feasible set. Each step adds a cutting hyperplane (via a gradient query) and updates the interior-point representation. The classical cost is $O(n^2)$ gradient queries plus $O(n^3)$ arithmetic per query.

The quantum cutting plane method uses:
1. **Quantum membership / separation oracle** access via superposition.
2. **Quantum interior point method (IPM) speedup** via quantum speedups for linear algebra inside the IPM. Specifically, the Newton step computation in interior point methods involves solving a linear system of the form $H^{-1} g$ where $H$ is the Hessian of the barrier function. This is the setting for [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]].
3. The algorithm maintains a quantum state encoding the current Hessian, applies the quantum linear system solver to compute approximate Newton steps, and uses the gradient oracle in superposition.

The total complexity is $\tilde{O}(n^{3/2})$ quantum gradient queries, achieving a $\sqrt{n}$ speedup over the $O(n^2)$ classical bound.

**Technical subtlety.** The quantum linear system solver requires the Hessian to be well-conditioned. In the cutting plane setting the barrier Hessian can be ill-conditioned, and the paper carefully handles this via preconditioning. Additionally, since the LP and IPM computations must be done coherently to preserve the quantum speedup, the paper argues that the per-step arithmetic can be organized to maintain coherence.

### Gibbs-sampling algorithm for simplex optimization

To minimize a convex function $f$ over the $n$-dimensional simplex $\Delta_n$, the algorithm uses:
1. **Quantum speedup for Gibbs sampling:** Sample from the Gibbs distribution $\pi_\beta(x) \propto e^{-\beta f(x)}$ over $\Delta_n$.
2. A quantum Markov chain (based on quantum walks) that mixes quadratically faster than the classical Metropolis chain.
3. The Gibbs sampler is run at inverse temperature $\beta = O(\log n / \varepsilon)$ and the output concentrates near the optimum.

The key component is a quantum speedup for sampling from log-concave distributions, using the **quantum speedup for Markov chain mixing** (Szegedy-type quantization): the quantum walk mixes in $O(\sqrt{T_{\text{mix}}})$ steps rather than $O(T_{\text{mix}})$.

---

## Key results

**Theorem 1 (Upper bound: Lipschitz convex optimization).** There exists a quantum algorithm that finds an $\varepsilon$-optimal point of a $G$-Lipschitz convex function over a convex body of diameter $D$ using
$$O\!\left(\frac{n^{1/2} G D}{\varepsilon}\right)$$
quantum gradient queries. This is a $\sqrt{n}$ improvement over the $O(n G^2 D^2 / \varepsilon^2)$ classical query count (when accounting for dimension-dependent classical finite-difference costs) and $O(\varepsilon^{-1})$ vs $O(\varepsilon^{-2})$ in the accuracy dependence.

**Theorem 2 (Upper bound: smooth cutting plane).** There exists a quantum algorithm for smooth convex optimization over a polytope using $\tilde{O}(n^{3/2})$ gradient queries, improving on the classical $O(n^2)$ cutting plane bound.

**Theorem 3 (Lower bound: Lipschitz convex optimization).** Any quantum algorithm for $\varepsilon$-optimizing a $G$-Lipschitz convex function (with $D, G, \varepsilon$ in standard range) requires
$$\Omega\!\left(\frac{n^{1/2}}{\varepsilon}\right)$$
quantum gradient queries. Combined with Theorem 1, this shows the $n^{1/2}/\varepsilon$ dependence is tight.

**Theorem 4 (Lower bound: smooth convex optimization).** Any quantum algorithm for smooth convex optimization requires $\Omega(n)$ gradient queries, showing that the cutting-plane algorithm's $\tilde{O}(n^{3/2})$ is at most a $\tilde{O}(\sqrt{n})$ gap from optimal.

**Lower bound technique.** The lower bounds are proved by reduction from the **quantum search lower bound** $\Omega(\sqrt{N})$ (Grover). The construction embeds a Boolean oracle into a convex function such that finding the optimum requires identifying a planted "needle" in a combinatorial haystack — equivalent to an unstructured search problem. Formally, for a function $f_{j}(x) = \max_i a_i^T x + b_i$ where one halfspace is perturbed at a hidden location $j \in [N]$, finding the optimum reveals $j$, requiring $\Omega(\sqrt{N})$ queries by the [[Adversary Method for Quantum Query Lower Bounds|quantum adversary method]] / search lower bound.

---

## Comparison with prior work

| Setting | Classical | This paper (quantum) | Lower bound |
|---|---|---|---|
| Lipschitz convex ($n$ dimensions) | $O(n \varepsilon^{-2})$ | $O(n^{1/2} \varepsilon^{-1})$ | $\Omega(n^{1/2} \varepsilon^{-1})$ **tight** |
| Smooth convex (cutting plane) | $O(n^2)$ (Vaidya) | $\tilde{O}(n^{3/2})$ | $\Omega(n)$ |
| Smooth convex (first-order) | $O(\varepsilon^{-1/2})$ (Nesterov) | — | — |
| Simplex optimization | $O(n^2 \log(1/\varepsilon))$ | $O(n^{3/2} \log(1/\varepsilon))$ | — |

**Prior quantum work.** Grover (1996) and its query complexity framework; Jordan (2005) showed quantum gradient estimation costs $O(1)$ vs classical $O(n)$. The Nesterov accelerated gradient descent $O(\varepsilon^{-1/2})$ is already optimal classically in the smooth first-order model and does not benefit from quantum speedup in the accuracy parameter. The quadratic speedup over classical for the Lipschitz case matches the standard quantum advantage pattern from [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]].

**Classical references for cutting planes.** The Vaidya (1989) volumetric center algorithm achieves $O(n^2)$ gradient queries; Lee-Sidford-Wong (2015) achieves $O(n^{1.5} \log n)$ in practice — the quantum algorithm improves on both.

---

## Limits / caveats

1. **Black-box model only.** The lower bounds apply to the oracle/query model. Classical algorithms with structural knowledge of $f$ (e.g., quadratic, separable) can do much better, and quantum algorithms may not retain the advantage in structured settings.

2. **State preparation overhead.** Like all quantum algorithms using quantum linear algebra (e.g., the IPM Newton step via QLSA), the algorithm requires quantum access to the gradient oracle and the ability to prepare quantum states encoding the Hessian. If the Hessian is not efficiently block-encodeable, the QLSA-based speedup is not achievable.

3. **Readout problem.** The cutting plane algorithm's output is a quantum state approximating the optimal solution. To extract the actual optimum classically requires measurement and may incur additional overhead if the output is a superposition.

4. **Accuracy dependence gap.** In the smooth case, the upper bound is $\tilde{O}(n^{3/2})$ and the lower bound is $\Omega(n)$. Whether $\tilde{O}(n)$ is achievable remains open.

5. **Gibbs sampling speedup caveats.** The Markov chain mixing speedup via quantum walks assumes the chain has a good spectral gap. For strongly non-smooth or ill-conditioned $f$, the mixing time can be exponential, and the quantum speedup (which is quadratic in the mixing time) does not help.

6. **Coherence requirements.** The interior-point computations require maintaining coherent quantum states across multiple gradient oracle calls — demanding for near-term hardware.

---

## Reusable ideas

1. [[Quantum Subgradient Method via Amplitude-Estimated Gradient Averaging]]
2. [[Quantum Cutting Plane via IPM Newton Steps with QLSA]]
3. [[Convex Optimization Lower Bound via Search Embedding]]
4. [[Gibbs Distribution Sampling via Quantum Walk Speedup]]

---

## References within this paper

- Jordan, "Fast quantum algorithm for numerical gradient estimation," PRL **95**, 050501 (2005) — quantum gradient in $O(1)$ queries independent of $n$
- Vaidya (1989) — volumetric center cutting plane algorithm, $O(n^2)$ queries
- Lee, Sidford & Wong (2015) — improved cutting plane, $O(n^{1.5}\log n)$ classical
- Grover (1996) — quantum search; the $\Omega(\sqrt{N})$ lower bound is the backbone of all quantum lower bounds here
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude estimation used in gradient averaging
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — quantum walk speedup for Markov chains; used in Gibbs sampling
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — QLSA for Newton step computation in IPM
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — improved QLSA used in quantum IPM
- Nesterov (1983) — accelerated gradient descent, $O(\varepsilon^{-1/2})$ classical lower bound
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes|Ambainis (2000)]] — quantum adversary method for lower bounds
- Ben-Tal & Nemirovski — convex optimization textbook background
- Lovász & Vempala (2006) — simulated annealing and Gibbs sampling for optimization
- Dyer, Frieze & Kannan (1991) — random walks for volume estimation and optimization

---

## Cross-links

### Paper notes
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude estimation is the core engine of the quantum subgradient speedup
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — QLSA provides the quantum Newton step inside the cutting plane IPM
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]] — improved QLSA used in the quantum IPM construction
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — Szegedy quantization underlies the Gibbs sampling speedup
- [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes]] — early quantum walk speedup showing the quadratic separation; conceptual precursor
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]] — adversary method framework for the query lower bounds
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]] — polynomial method provides complementary lower bound toolkit
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT subsumes the block-encoding machinery used in the quantum IPM Newton step

### Trick cards
- [[Quantum Subgradient Method via Amplitude-Estimated Gradient Averaging]]
- [[Quantum Cutting Plane via IPM Newton Steps with QLSA]]
- [[Convex Optimization Lower Bound via Search Embedding]]
- [[Gibbs Distribution Sampling via Quantum Walk Speedup]]
- [[Adversary Method for Quantum Query Lower Bounds]]
- [[Fourier Integral LCU for Matrix Inversion]]
- [[QSVT Meta-Template]]
