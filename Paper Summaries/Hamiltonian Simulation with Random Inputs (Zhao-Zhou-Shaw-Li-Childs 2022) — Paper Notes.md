> **Source:** Qi Zhao, You Zhou, Alexander F. Shaw, Tongyang Li, and Andrew M. Childs, *Hamiltonian Simulation with Random Inputs*, arXiv:2111.04773, PRL **129**, 270502 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2111.04773) · [PRL](https://doi.org/10.1103/PhysRevLett.129.270502)
> **Tags:** #hamiltonian-simulation #trotter #product-formulas #average-case #frobenius-norm #random-states

---

## The computational problem

Same setup as standard [[Hamiltonian simulation]]: approximate $e^{-iHt}$ for $H = \sum_{l=1}^L H_l$ via [[Product Formulas]] or truncated Taylor series. But instead of bounding the worst-case error $\|U - U_0\|$ (spectral norm), bound the *average-case* error when the input state is drawn from a 1-design ensemble.

## What the paper does

Develops a theory of average-case Trotter error by connecting the average error to the Frobenius norm of the multiplicative error $M$ (where $U = U_0(I + M)$). The central result:

$$
R(U, U_0) \le \frac{1}{\sqrt{d}} \|M\|_F
$$

where $d = 2^n$ is the Hilbert space dimension and the average is over any 1-design input ensemble (including the computational basis).

Since $\|M\|_F \le \sqrt{d} \|M\|$, the Frobenius-norm bound can never be worse than the spectral-norm bound. But for structured Hamiltonians it can be *much* better: for an $n$-spin nearest-neighbor Heisenberg chain, the average error is $O(\sqrt{n})$ compared to the worst-case $O(n)$. The improvement is quadratic in system size.

My assessment: this is a natural and well-executed idea. The $1/\sqrt{d}$ factor from the Frobenius norm is the core observation — the rest is careful commutator bookkeeping applied to both PF and Taylor series. The OTOC application is a nice bonus. The main limitation is that the improvement is in the $n$-dependence, not the $t$ or $\varepsilon$ dependence, so the gate count savings are polynomial but moderate.

## The average-case framework

**Theorem 1.** For input drawn from a 1-design ensemble on $d$-dimensional Hilbert space:

$$
R(U, U_0) \le [E_\psi S(\psi)]^{1/2} = \frac{1}{\sqrt{d}} \|M\|_F
$$

where $S(\psi) = \|U|\psi\rangle - U_0|\psi\rangle\|_2^2$ and $M$ is the multiplicative error. The variance satisfies $\text{Var}(S(\psi)) \le \frac{4\|M\|_F^2}{d(d+1)}$ for 2-design inputs.

The framework extends to subsystem inputs: if the initial state is random only on a subsystem of dimension $d_1$ (e.g., one clean qubit + maximally mixed rest), the bound becomes $\frac{1}{\sqrt{d_1}} \|M\Pi\|_F$ where $\Pi$ projects onto the subsystem.

The 1-design assumption is weak — it's satisfied by the computational basis, random product states, stabilizer states, and many other easily prepared ensembles.

## Average-case bounds for product formulas

**Theorem 2 (general PFp).** For the $p$-th order product formula with $r$ steps:

$$
R = O\!\left(\frac{T_p \, t^{p+1}}{r^p}\right)
$$

where the average-case commutator parameter is

$$
T_p := \sum_{l_1, \ldots, l_{p+1}=1}^{L} \frac{1}{\sqrt{d}} \|[H_{l_1}, [H_{l_2}, \ldots, [H_{l_p}, H_{l_{p+1}}]\cdots]]\|_F.
$$

Compare with the worst-case parameter from [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs et al. (2019)]]:

$$
\alpha_{\text{comm},p} := \sum_{l_1, \ldots, l_{p+1}=1}^{L} \|[H_{l_1}, [H_{l_2}, \ldots, [H_{l_p}, H_{l_{p+1}}]\cdots]]\|.
$$

Since $\frac{1}{\sqrt{d}}\|A\|_F \le \|A\|$ always, we have $T_p \le \alpha_{\text{comm},p}$. The gap depends on the eigenvalue distribution of the nested commutators.

**Theorem 3 (triangle bounds for PF1 and PF2).** Tighter bounds with explicit prefactors, moving some summations inside the Frobenius norm:

$$
R(U_1^r(t/r), U_0(t)) \le \frac{t^2}{r} T_1', \quad R(U_2^r(t/r), U_0(t)) \le \frac{t^3}{r^2} T_2'
$$

where

$$
T_1' = \frac{1}{2\sqrt{d}} \sum_{l_1=1}^{L-1} \left\|\left[H_{l_1}, \sum_{l_2 > l_1} H_{l_2}\right]\right\|_F.
$$

**Theorem 4 (interference bound for PF1).** For nearest-neighbor Hamiltonians with the even-odd decomposition, destructive error interference carries over to the average case:

$$
R(U^r(t), U_0(t)) = O\!\left(\sqrt{n}\left(\frac{t}{r} + \frac{t^3}{r^2}\right)\right)
$$

provided $nt^2/r$ is small. This mirrors the worst-case interference result from [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes|Tran et al. (2020)]], but with $\sqrt{n}$ replacing $n$.

## Average-case bound for truncated Taylor series

The average error for the $K$-th order truncated Taylor series is reduced from

$$
O\!\left(\alpha t \cdot \frac{(\ln 2)^{K+1}}{(K+1)!}\right) \quad \text{(worst case)}
$$

to

$$
O\!\left(\max_i \sqrt{\alpha_i \alpha} \cdot t \cdot \frac{(\ln 2)^{K+1}}{(K+1)!}\right) \quad \text{(average case)}
$$

where $H = \sum_l \alpha_l H_l$ and $\alpha = \sum_l \alpha_l$. Since the Taylor series gate complexity is logarithmic in $1/\varepsilon$, the gate savings from the average-case analysis are mild — the improvement is subsumed in the constant.

## Applications

### Lattice Hamiltonians

For an $n$-spin nearest-neighbor Heisenberg chain:

| Quantity | Worst case | Average case |
|---|---|---|
| $\alpha_{\text{comm},1}$ / $T_1$ | $O(n)$ | $O(\sqrt{n})$ |
| PF1 gate count | $O(n^2 t^2/\varepsilon)$ | $O(n^{1.5} t^2/\varepsilon)$ |
| PF2 gate count | $O(n^{1.5} t^{1.5}/\varepsilon^{0.5})$ | $O(n^{1.25} t^{1.5}/\varepsilon^{0.5})$ |

### $k$-local Hamiltonians

For $k$-local Hamiltonians on $n$ qubits:

| Order | Worst-case error | Average-case error |
|---|---|---|
| PF1 | $O(t^2 n^{2k-1}/r)$ | $O(t^2 n^{(3k-1)/2}/r)$ |
| PF2 | $O(t^3 n^{3k-2}/r^2)$ | $O(t^3 n^{(5k-3)/2}/r^2)$ |

### Power-law interactions ($1/r^\alpha$)

For $\alpha > D$: worst-case and average-case errors have the same $n$-scaling (both $O(n)$). For $0 \le \alpha < D$: average-case error scaling improves by $n^{1/2}$ compared to worst case.

### Out-of-time-order correlators (OTOCs)

In OTOC measurements, the initial state is $\rho \otimes I_{d_1}/d_1$ — one qubit in a chosen state, the rest maximally mixed. The Trotter error in the OTOC is bounded by $\frac{1}{\sqrt{d_1}} \|M\|_F$ using the subsystem variant. For a nearest-neighbor Hamiltonian with PF2, this reduces the gate complexity from $O(n^{1.5} t^{1.5})$ to $O(n^{1.25} t^{1.5})$.

## Limits / caveats

- The improvement is in the *system-size* dependence, not in $t$ or $\varepsilon$. For a fixed-size system, average-case analysis gives no benefit.
- The Cauchy-Schwarz inequality introduces a gap between the true average error and the Frobenius bound. Appendix E of the paper explores tighter bounds without Cauchy-Schwarz, but the gap appears small in practice (factor ~2–3).
- The 1-design requirement is mild but non-trivial: it excludes adversarial input states designed to hit worst-case directions. For most physical simulation scenarios this is fine.
- For the Taylor series / [[Linear Combination of Unitaries (LCU)|LCU]] method, the improvement is mild because the gate count is logarithmic in $1/\varepsilon$.
- The interference bound (Theorem 4) inherits the condition $nt^2/r < \text{const}$ from [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes|Tran et al. (2020)]].

## Reusable ideas

1. [[Frobenius-Norm Average-Case Error Bound]] — replace worst-case spectral-norm error with $\frac{1}{\sqrt{d}} \|M\|_F$ for 1-design inputs; quadratic system-size improvement for local Hamiltonians
2. [[Frobenius-Norm Commutator Sum for Product Formulas]] — average-case analogue of the [[Trotter Commutator-Scaling Bound|commutator-scaling bound]]: replace $\sum \|[\cdots]\|$ with $\sum \frac{1}{\sqrt{d}} \|[\cdots]\|_F$

## References within this paper

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2021)]] — worst-case [[Trotter Commutator-Scaling Bound|commutator-scaling bounds]] that this paper generalises to average case
- [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes|Tran, Chu, Su, Childs & Gorshkov (2020)]] — worst-case lattice interference bounds; this paper extends the same interference analysis to average-case Frobenius norms
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry, Childs, Cleve, Kothari & Somma (2015)]] — truncated Taylor series method whose average-case error is also bounded
- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes|Su, Huang & Campbell (2021)]] — fermionic seminorm approach; complementary restriction to a subspace (particle-number sector vs random states)
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes|An, Fang & Lin (2021)]] — vector-norm bounds for unbounded Hamiltonians; prior work on state-dependent error
- Heyl, Hauke & Zoller (2019) — local-observable error bounds for PF1
- Chen, Huang, Kueng & Tropp (2021) — concentration bounds for observable errors
- Şahinoğlu & Somma (2021) — low-energy subspace Trotter bounds

---

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — worst-case commutator-scaling framework that this paper generalises
- [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes]] — worst-case interference analysis; this paper extends it to average case
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — complementary error-reduction strategy via symmetry protection
- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]] — fermionic seminorm restricts to physical subspace; analogous to average-case restriction here
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]] — randomised Trotter ordering; different way to exploit cancellation
- [[Faster Quantum Simulation by Randomization (Berry-Childs-Su-Wang-Wiebe 2020) — Paper Notes]] — randomized approaches to simulation (being created in parallel)
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] — different dimension of improvement (norm scaling rather than input-state averaging)

### Trick cards
- [[Frobenius-Norm Average-Case Error Bound]]
- [[Frobenius-Norm Commutator Sum for Product Formulas]]
- [[Trotter Commutator-Scaling Bound]]
- [[Commutator-to-Integral Telescoping for Trotter Error Cancellation]]

### Concept hubs
- [[Product Formulas]]
- [[Hamiltonian Simulation — Comparison Tables]]
