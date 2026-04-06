> **Source:** Minh C. Tran, Su-Kuan Chu, Yuan Su, Andrew M. Childs, and Alexey V. Gorshkov, *Destructive Error Interference in Product-Formula Lattice Simulation*, arXiv:1912.11047, PRL **124**, 220502 (2020)
> **Links:** [arXiv](https://arxiv.org/abs/1912.11047) · [PRL](https://doi.org/10.1103/PhysRevLett.124.220502)
> **Tags:** #hamiltonian-simulation #trotter #product-formulas #lattice #error-cancellation

---

## The computational problem

Simulate the time evolution $U_t = e^{-iHt}$ of a nearest-neighbor interacting lattice Hamiltonian $H = \sum_{i=1}^{n-1} h_i$ on $n$ sites using the first-order [[Product Formulas|product formula]] (PF1). Each $h_i$ acts on sites $i, i+1$ with $\|h_i\| \le J$ (set $J=1$). The Hamiltonian splits as $H = H_1 + H_2$ where $H_1 = \sum_{\text{odd } j} h_j$ and $H_2 = \sum_{\text{even } j} h_j$, with terms within each part mutually commuting.

The question: why does PF1 perform so much better on lattice systems than existing error bounds predict?

## What the paper does

Answers the question by proving that Trotter errors from different time steps interfere destructively on lattices, yielding a total error much smaller than the sum of per-step errors. The standard analysis bounds the total error via the triangle inequality — summing $r$ per-step errors of size $O(nt^2/r^2)$ to get $O(nt^2/r)$. This paper shows the actual total error is $O(nt/r + nt^3/r^2)$, a factor of $t$ tighter in the leading term.

For a 1D nearest-neighbor system at $t = n$ and fixed $\varepsilon$, the gate count improves from $O(n^4/\varepsilon)$ (old bound) to $O(n^3/\varepsilon)$ (this paper), closely matching the empirical scaling $O(n^{2.964})$ observed by Childs, Maslov, Nam, Ross & Su (PNAS 2018).

My assessment: this is a clean, satisfying result. The mechanism is simple and physical — rotated error terms average out around the circle — and the proof is rigorous. It explains a long-standing gap between theory and numerics for PF1 on lattices.

## The mechanism: how destructive interference works

The per-step error is $\delta = U_{t/r} - U^{(1)}_{t/r} U^{(2)}_{t/r}$. To leading order,

$$
\delta \approx \frac{t^2}{2r^2} [H_1, H_2] = \frac{t^2}{2r^2} [H, H_2].
$$

The total error (to first order in $\delta$) is

$$
\Delta \approx \sum_{j=0}^{r-1} U_{t/r}^j \, \delta \, U_{t/r}^{-(j)}.
$$

The standard approach applies the triangle inequality: $\|\Delta\| \le r\|\delta\| = O(nt^2/r)$. But notice what the sum actually does — it conjugates $\delta$ by the evolution $U_{t/r}^j$ for $j = 0, 1, \ldots, r-1$, rotating the error operator through evenly spaced "angles." These rotations cause cancellation.

The key identity: for any operator $A$,

$$
U_t A U_{-t} - A = -i \int_0^t dx \, U_x [H, A] U_{-x}.
$$

Since $\delta \approx \frac{t^2}{2r^2} [H, H_2]$, the sum can be approximated by an integral, giving

$$
\|\Delta\| \approx \frac{r}{t} \left\| \int_0^t dx \, U_x \left[H, \frac{t^2}{2r^2} H_2\right] U_{-x} \right\| \le \frac{t}{2r^2} \|H_2\| = O\!\left(\frac{nt}{r}\right).
$$

The integral evaluates to a telescoped expression $U_{-t}(iA)U_t - iA$ with $A = \frac{t^2}{2r^2} H_2$, whose norm is at most $2\|A\|$. The result is a factor of $t$ tighter than the triangle inequality.

## Structure of the error: Lemma 1

The paper decomposes each order of the per-step error. Define $\delta = \sum_{k=2}^{\infty} \frac{(-it)^k}{k! r^k} \delta_k$. Then for all $k \ge 2$:

$$
\delta_k = [H, S_k] + V_k
$$

where $\|S_k\| = O(k^2 n^{k-1})$, $\|[H, S_k]\| = O(k^3 n^{k-1})$, and $\|V_k\| = O(e^{k-2} n^{k-2})$.

The $[H, S_k]$ part is the "good" component — it admits the [[Commutator-to-Integral Telescoping for Trotter Error Cancellation|telescoping trick]] via conjugation, so it contributes $O(nt/r)$ to the total error. The $V_k$ remainder is one order smaller in $n$ ($n^{k-2}$ vs $n^{k-1}$) and contributes $O(nt^3/r^2)$. For $k = 2$, $V_2 = 0$ exactly.

## Key results

**Theorem (main error bound).** For a 1D nearest-neighbor Hamiltonian with $n$ sites and $\|h_i\| \le 1$, the total error of PF1 with $r$ time steps satisfies

$$
\|\Delta\| = O\!\left(\frac{nt}{r} + \frac{nt^3}{r^2}\right)
$$

provided $nt^2/r$ is less than a small constant.

**Gate count.** Given error tolerance $\varepsilon$:
- **Small time** ($\varepsilon t \le 1$): gate count $O(n^2 t / \varepsilon)$. This has optimal scaling in $t$.
- **Large time** ($\varepsilon t > 1$): divide into $m = \lceil \sqrt{\varepsilon t} \rceil$ stages, giving gate count $O(n^2 t^{3/2} / \varepsilon^{1/2})$.
- **Combined:** $\max\{O(n^2 t/\varepsilon), O(n^2 t^{3/2}/\varepsilon^{1/2})\}$.

## Comparison with prior work

| Analysis | Error bound | Gate count ($t = n$, fixed $\varepsilon$) |
|---|---|---|
| Triangle inequality ([[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes\|Childs-Su 2019]]) | $O(nt^2/r)$ | $O(n^4/\varepsilon)$ |
| **This paper** | $O(nt/r + nt^3/r^2)$ | $O(n^3/\varepsilon)$ |
| Empirical (Childs-Maslov-Nam-Ross-Su 2018) | — | $O(n^{2.964})$ |

The match between $O(n^3)$ and $O(n^{2.964})$ is striking.

## Connection to higher-order formulas

Numerical evidence (Fig. 2 in the paper) shows that PF2 and PF4 do *not* exhibit the same destructive interference — their errors scale as $O(t^3)$ and $O(t^5)$ respectively, matching the triangle-inequality bounds. The authors argue this is because the leading error for PF1 has the special structure $\delta_2 = [H, H_2]$ (a pure commutator with no remainder), which enables the telescoping trick. Higher-order formulas have more complex error structures where this cancellation doesn't apply as cleanly.

This observation is picked up by [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes|Zhao, Zhou, Shaw, Li & Childs (2022)]], who show the same interference pattern persists in the average-case analysis.

## Scope and limitations

**Where it applies:**
- 1D chains with nearest-neighbor interactions (Heisenberg, transverse field Ising, disordered chains, MBL systems)
- Some higher-dimensional models where $H$ splits into exactly two mutually commuting parts (e.g., transverse field Ising in any dimension)
- Plane-wave dual basis electronic structure Hamiltonians (kinetic + Coulomb split)

**Where it doesn't:**
- Hamiltonians requiring three or more mutually commuting parts (general 2D lattice models with non-Ising interactions)
- The condition $nt^2/r < \text{const}$ is required for the proof. Numerics suggest the bound holds more broadly, but this remains a conjecture.
- Higher-order product formulas (PF2, PF4, etc.) — the interference mechanism appears specific to PF1.

**Conjecture:** If the error bound $O(nt/r + nt^3/r^2)$ holds without the condition $nt^2/r < \text{const}$, then PF1 would match PF2's gate count $O(\sqrt{n^3 t^3/\varepsilon})$ in the large-time limit, making the simplest product formula as efficient as the symmetric one for lattice problems. This remains open.

## Reusable ideas

1. [[Commutator-to-Integral Telescoping for Trotter Error Cancellation]] — decompose per-step error as $[H, S] + V$; the commutator part telescopes under time evolution, cancelling across steps
2. [[Inductive Commutator-Remainder Decomposition]] — recursive construction showing each order $\delta_k$ of the Trotter step error splits into a commutator $[H, S_k]$ (which cancels) and a remainder $V_k$ (which is one order smaller in system size)

## References within this paper

- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — original PF1 simulation proposal
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs & Su (2019)]] — prior best bounds using [[Trotter Commutator-Scaling Bound|commutator scaling]]; this paper improves on their $O(nt^2/r)$ estimate
- Childs, Maslov, Nam, Ross & Su (PNAS 2018) — empirical benchmarks showing $O(n^{2.964})$ scaling that this paper explains
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2007)]] — higher-order Suzuki formula bounds
- Heyl, Hauke & Zoller (2019) — observed that local-observable PF1 error doesn't grow with simulation time, consistent with this paper's $O(nt/r)$ bound at fixed step size
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — plane-wave dual basis Hamiltonian where this analysis applies

---

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — provides the commutator-scaling framework; this paper goes further for lattice systems
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — same Tran-Su-Childs team; uses symmetries to *engineer* error cancellation between steps, whereas this paper identifies *natural* cancellation on lattices
- [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]] — extends the interference analysis to average-case errors with Frobenius-norm bounds
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]] — different improvement direction (norm scaling) for the same underlying Dyson/Trotter machinery
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — identified the theory-vs-practice gap that this paper partly explains
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]] — randomized ordering as an alternative route to error reduction in [[Product Formulas]]
- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]] — restricts error to physical subspace; complementary to this paper's geometry-based cancellation

### Trick cards
- [[Commutator-to-Integral Telescoping for Trotter Error Cancellation]]
- [[Inductive Commutator-Remainder Decomposition]]
- [[Trotter Commutator-Scaling Bound]]
- [[Product-Formula Error Representation Switching]]

### Concept hubs
- [[Product Formulas]]
- [[Hamiltonian Simulation — Comparison Tables]]
