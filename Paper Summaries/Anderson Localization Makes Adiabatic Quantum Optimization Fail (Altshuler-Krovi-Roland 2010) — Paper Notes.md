> **Source:** Boris Altshuler, Hari Krovi, Jérémie Roland, *Anderson localization makes adiabatic quantum optimization fail*, Proceedings of the National Academy of Sciences **107**(28):12446–12450 (2010); arXiv:0912.0746
> **Links:** [arXiv](https://arxiv.org/abs/0912.0746) · [PNAS](https://doi.org/10.1073/pnas.1002116107)
> **Tags:** #adiabatic #NP-complete #spectral-gap #Anderson-localization #random-satisfiability #negative-result

---

## The computational problem

**Exact Cover 3 (EC3):** Given $N$ bits $x_1, \ldots, x_N$ and $M$ clauses, each a triplet $(i_c, j_c, k_c)$, find an assignment where exactly one bit per clause is 1. Cost function:

$$f(x) = \sum_c (x_{i_c} + x_{j_c} + x_{k_c} - 1)^2$$

Solutions have $f(x) = 0$. Random instances are drawn by picking $M$ clauses uniformly at random. Hardness is controlled by the clause-to-variable ratio $\alpha = M/N$, with satisfiability threshold $\alpha_s \approx 0.6263$. Near $\alpha_s$, instances have few isolated solutions separated by Hamming distance $\Theta(N)$ — the hard regime.

## What the paper does

Shows that the standard [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|adiabatic quantum optimisation]] algorithm encounters exponentially small spectral gaps for typical random instances of NP-complete problems near the satisfiability threshold. The mechanism is Anderson localisation of eigenstates on the $N$-dimensional hypercube of assignments.

This is a *typical-case* negative result, not a worst-case construction like [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes|van Dam-Mosca-Vazirani (2001)]], which planted specific hard instances. The paper argues that for *randomly generated* hard instances, AQO is doomed to exponential runtime with probability tending to 1.

## The construction

### Mapping AQO to Anderson's model

The adiabatic Hamiltonian for EC3 uses:
- **Problem Hamiltonian:** $\hat{H}_P = M\hat{I} - \frac{1}{2}\sum_i B_i \hat{\sigma}_z^{(i)} + \frac{1}{4}\sum_{i,j} J_{ij} \hat{\sigma}_z^{(i)}\hat{\sigma}_z^{(j)}$, where $B_i$ counts clauses involving bit $i$ and $J_{ij}$ counts clauses involving both $i$ and $j$.
- **Initial Hamiltonian:** $\hat{H}_0 = -\sum_i \hat{\sigma}_x^{(i)}$ (transverse field).
- **Interpolation:** $\hat{H}_{QC}(\lambda) = \hat{H}_P + \lambda \hat{H}_0$, with $\lambda$ decreasing from $+\infty$ to 0.

The key observation: rewriting in the computational basis gives

$$\hat{H}_{QC}(\lambda) = \underbrace{\sum_\sigma E_P(\sigma)|\sigma\rangle\langle\sigma|}_{\text{disorder}} + \lambda \underbrace{\sum_{\sigma, \sigma' \text{ n.n.}} |\sigma\rangle\langle\sigma'|}_{\text{hopping}}$$

This is exactly the [[Anderson Localisation on the Hypercube as AQO Obstruction|Anderson model]] on the $N$-dimensional hypercube ($2^N$ vertices), where $E_P(\sigma)$ is the random on-site energy (cost function) and $\lambda$ controls nearest-neighbour hopping (single bit flips).

### Why anti-crossings appear

For hard random instances near $\alpha_s$, there are few solutions $\sigma_1, \sigma_2$ separated by Hamming distance $n \sim v(\alpha) N$, where $v(\alpha) \approx \frac{4}{9}(1 - e^{-3\alpha})$. The argument proceeds:

1. **Energy splitting scales as $\sqrt{N}$:** Using cluster expansion, the perturbative correction to each solution's energy is a sum of $\sim N$ independent $O(1)$ random terms. The *difference* between two solutions' energies at order $m$ satisfies $|F_{1,2}^{(m)}|^2 = O(N)$, so:
$$|E_1(\lambda) - E_2(\lambda)| \sim \sqrt{N} \sum_m \lambda^{2m} f^{(m)}$$

2. **Anti-crossing is probable:** Adding a single clause to an $(M-1)$-clause instance can swap which solution is the ground state at $\lambda = 0$, creating a level anti-crossing. This happens when the splitting exceeds 4 (the maximum energy shift from one clause), which occurs for $\lambda \geq \lambda^* \sim N^{-1/8}$.

3. **Gap is exponentially small:** The tunnelling matrix element between two solutions at Hamming distance $n$ appears only at $n$-th order perturbation theory:
$$V_{12} = \lambda^n \sum_{\text{trajectories}} \prod_{k=1}^n E_P(\sigma_{\text{tr}}^{(k)})^{-1} + O(\lambda^{n+1})$$
The factorial growth of trajectories ($\sim n!$) cancels with the factorial growth of the energy product, giving $V_{12} < (A\lambda)^n$ for some $A = O(1)$.

## Key results

**Main estimate (Eq. 9):**

$$\Delta_{\min} \sim \exp\!\left[-\frac{v(\alpha)N}{8} \ln\!\left(\frac{N}{N_0}\right)\right]$$

where $N_0 = O(1)$ depends on the constant $A$ and the cluster expansion coefficient $f^{(2)}$.

This is *super-exponentially* small in $N$ — the gap decreases faster than any exponential $e^{-cN}$, because of the additional $\ln N$ factor. The adiabatic runtime $T \sim 1/\Delta_{\min}^2$ is therefore catastrophically large.

**Why you can't dodge it:** The number of low-energy states grows exponentially with the energy window $\epsilon \sim \sqrt{N}\lambda^4$, so the ground state participates in $\nu(\epsilon)$ anti-crossings. The probability of avoiding *all* of them is exponentially suppressed.

## Comparison with prior work

| Paper | Type of hard instance | Gap scaling | Nature of result |
|---|---|---|---|
| [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes\|Farhi et al. (2000)]] | Small random instances | Polynomial (numerical) | Optimistic numerics |
| [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes\|van Dam-Mosca-Vazirani (2001)]] | Planted Hamming weight trap | $\exp(-\Omega(N))$ | Worst-case construction |
| [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes\|Roland-Cerf (2002)]] | Unstructured search | $1/\sqrt{N}$ | Optimal for unstructured case |
| [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes\|Aharonov et al. (2004)]] | General | Polynomial (by construction) | Equivalence proof |
| **This paper** | Random EC3 near $\alpha_s$ | $\exp(-\Omega(N \ln N))$ | Typical-case failure |

The super-exponential gap is worse than anything in prior constructions. The van Dam-Mosca-Vazirani result was a planted worst case; this paper shows the problem is *generic* for random instances.

## Limits / caveats

- The analysis assumes the standard linear interpolation path. Modified adiabatic schedules (e.g. [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] local scheduling, or [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes|Kieferová-Wiebe (2014)]] coherent control) could help navigate known gap structures, but here the gaps are at *unknown* locations, so adaptive scheduling doesn't obviously help.
- The perturbative analysis requires $\lambda < \lambda_{\text{cr}} \sim 1/\log N$. The authors argue localisation persists well beyond this regime for low-energy states, but this isn't rigorously proven.
- The result is specific to EC3 but the authors argue the mechanism applies to any NP-complete problem with similar random-instance structure (clustered solutions at large Hamming distance). The extension to, say, 3-SAT is plausible but not formally shown.
- Does *not* rule out quantum advantage via non-adiabatic quantum algorithms (e.g. QAOA, quantum walks, or circuit-based approaches) for these problems.
- The cluster expansion coefficient $f^{(2)} \approx 0.18$ is extracted numerically from 2500 random instances, not derived analytically.

## Reusable ideas

1. [[Anderson Localisation on the Hypercube as AQO Obstruction]] — Mapping a combinatorial optimisation Hamiltonian to the Anderson model on a high-dimensional graph, where the cost function provides the random on-site potential and the driver Hamiltonian provides hopping. Localisation then implies exponentially small tunnelling matrix elements and spectral gaps.

2. [[Cluster Expansion for Perturbative Energy Statistics]] — Using the cluster expansion (from statistical mechanics) to show that perturbative energy corrections decompose into sums of $O(N)$ independent $O(1)$ random terms, giving $\sqrt{N}$ scaling of energy differences between localised states.

## References within this paper

- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi-Goldstone-Gutmann-Sipser (2000)]] — The original AQO proposal; this paper shows its failure on random instances.
- Farhi, Goldstone, Gutmann, Lapan, Lundgren, Preda (2001) — Early numerical simulations suggesting polynomial gaps for small instances (up to $N=20$).
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov-van Dam-Kempe-Landau-Lloyd-Regev (2004)]] — Equivalence of AQC and circuit model.
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes|van Dam-Mosca-Vazirani (2001)]] — Planted worst-case exponential gap.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — Motivation: factoring is in BQP but AQO hasn't cracked NP-complete problems.
- Young, Knysh, Smelyanskiy (2008) — Latest numerical simulations (up to $N=128$) showing polynomial gap; this paper explains why the polynomial trend reverses at larger $N$.
- Farhi, Goldstone, Gosset, Gutmann, Meyer, Shor (2009) — First-order phase transition inducing exponential gap for planted 3-SAT instances.
- P. W. Anderson (1958) — Original localisation paper; the physical mechanism underlying the result.

## Cross-links

### Paper notes
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]]
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]]

### Trick cards
- [[Anderson Localisation on the Hypercube as AQO Obstruction]]
- [[Cluster Expansion for Perturbative Energy Statistics]]
- [[Local Adiabatic Schedule for Gap Navigation]]
- [[Gap-Adapted Adiabatic Scheduling]]
