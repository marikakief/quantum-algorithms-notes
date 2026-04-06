> **Source:** Yu-An Chen, Andrew M. Childs, Mohammad Hafezi, Zhang Jiang, Hwanmun Kim, Yijia Xu, *Efficient Product Formulas for Commutators and Applications to Quantum Simulation*, arXiv:2111.12177, Phys. Rev. Research **4**, 013191 (2022)
> **Links:** [arXiv](https://arxiv.org/abs/2111.12177) · [Journal](https://doi.org/10.1103/PhysRevResearch.4.013191)
> **Tags:** #hamiltonian-simulation #product-formula #commutator #lattice-gauge-theory #counterdiabatic-driving #quantum-simulation

---

## The computational problem

Same setting as [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs-Wiebe 2013]]: given anti-Hermitian operators $A$ and $B$, implement $e^{x^2[A,B]}$ to error $\varepsilon$ using only exponentials $e^{\alpha_i x A}$ and $e^{\alpha_i x B}$. Count the number of elementary exponentials.

Additionally: implement $e^{x(A+B) + Rx^2[A,B]}$ — a *combined* sum-and-commutator formula — which turns out to be achievable at no extra cost over the pure commutator case.

## What the paper does

Constructs a **third-order** (error $O(x^4)$) product formula for commutator exponentials using only 6 exponentials, directly solving the polynomial equations via the operator differential method. This is one order better than the standard group commutator $S_2(x) = e^{xA}e^{xB}e^{-xA}e^{-xB}$ (which is second-order, error $O(x^3)$), and it's the starting point for improved recursive constructions.

The paper then proposes four new recursive formulas ($\sqrt{4}$-copy, $\sqrt{5}$-copy, $\sqrt{6}$-copy, $\sqrt{10}$-copy) that raise the approximation order by 2 per recursion step. The $\sqrt{10}$-copy formula performs best numerically. Combined with the better $S_3$ base formula, this significantly reduces gate counts compared to the [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs-Wiebe 2013]] constructions.

Applications to counterdiabatic driving, 1D fermion chains with next-nearest-neighbour hopping, and fractional quantum Hall (Kapit-Mueller) models demonstrate the practical utility.

## The construction

### The third-order base formula $S_3(x)$

Using the operator differential method (a systematic BCH-like expansion), any 6-gate product formula can be written as:

$$e^{p_1 xA} e^{p_2 xB} e^{p_3 xA} e^{p_4 xB} e^{p_5 xA} e^{p_6 xB} = e^{\Phi(x)}$$

where

$$\Phi(x) = x(lA + mB) + \frac{x^2}{2}(lm - 2q)[A,B] + \frac{x^3}{6}\left(\left(\frac{l^2 m}{2} - 3r\right)[A,[A,B]] + \left(\frac{m^2 l}{2} - 3s\right)[B,[B,A]]\right) + O(x^4)$$

with $l = p_1 + p_3 + p_5$, $m = p_2 + p_4 + p_6$, and $q, r, s$ defined as specific symmetric polynomials in the $p_i$.

For a pure commutator formula, require $l = m = 0$ (kills linear terms), $q = -1$ (normalises the commutator coefficient), and $r = s = 0$ (kills third-order terms). The solution:

$$S_3(x) = e^{\frac{\sqrt{5}-1}{2}xA} \, e^{\frac{\sqrt{5}-1}{2}xB} \, e^{-xA} \, e^{-\frac{\sqrt{5}+1}{2}xB} \, e^{\frac{3-\sqrt{5}}{2}xA} \, e^{xB} = e^{x^2[A,B] + O(x^4)}.$$

The golden ratio $\varphi = \frac{\sqrt{5}+1}{2}$ appears naturally in the solution — the coefficients involve $\varphi - 1$, $-1$, $-\varphi$, $2-\varphi$, $1$.

### Sum-and-commutator formula

For arbitrary $R \in \mathbb{R}$, there exists a 6-gate formula implementing:

$$f_R(x) = e^{x(A+B) + Rx^2[A,B] + O(x^4)}.$$

In the large-$R$ limit (i.e., small step size), approximate solutions have coefficients $p_i \sim O(\sqrt{R})$. The combined formula is:

$$\left(f_{\beta n/\alpha^2}(\alpha/n)\right)^n = e^{\alpha(A+B) + \beta[A,B] + O((\alpha\beta + \beta^2)/n)}.$$

This uses $6n$ gates. The sum $A + B$ comes for free alongside the commutator — no extra gates needed.

### Improved recursive constructions

Starting from an $m$-th order formula $f_m(x)$ with $f_m(x) = e^{x^2[A,B]} + C_m x^{m+1} + D_m x^{m+2} + O(x^{m+3})$, the paper constructs four recursions that raise the order by 2:

| Recursion | Copies | Gate count | Formula |
|---|---|---|---|
| $\sqrt{4}$-copy ($Q$) | 4 | $N_{m+2} = 4N_m - 3$ | $Q_n(ax)Q_n^{-1}(bx)Q_n(cx)Q_n^{-1}(dx)$, solving a 2-equation system |
| $\sqrt{5}$-copy ($W$) | 5 | $N_{m+2} = 5N_m - 4$ | $W_n(-s'x)W_n^{-1}(x)W_n(sx)W_n^{-1}(-x)W_n(-s'x)$ |
| $\sqrt{6}$-copy ($V$) | 6 | $N_{2k+1} = 6N_{2k-1} - 4$ | Modified [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes\|Childs-Wiebe]] $\sqrt{6}$-copy, reordered for odd-order base |
| $\sqrt{10}$-copy ($G$) | 10 | $N_{m+2} = 10N_m - 4$ | Childs-Wiebe 5-copy (order +1) then 2-copy symmetrisation (order +1) |

Starting from $S_3$ (6 gates, 3rd order), the closed-form gate counts for order $2k+1$ are:

| Formula | $N_{2k+1}$ |
|---|---|
| $Q$ ($\sqrt{4}$) | $5 \cdot 4^{k-1} + 1$ |
| $W$ ($\sqrt{5}$) | $5^k + 1$ |
| $V$ ($\sqrt{6}$) | $\frac{1}{15}(13 \cdot 6^k + 12)$ |
| $G$ ($\sqrt{10}$) | $\frac{1}{9}(5 \cdot 10^k + 4)$ |

### Why $\sqrt{10}$-copy wins despite more copies

Numerically, $G_5$ (the $\sqrt{10}$-copy at 5th order, using 56 gates per step) outperforms all others in total gate count to fixed accuracy — the larger number of exponentials per step is offset by much smaller error constants. The paper attributes this to the "time evolution trajectory" of the coefficients staying in a narrower range.

## Key results

**Equation (14) — Third-order commutator formula:** $S_3(x) = e^{x^2[A,B] + O(x^4)}$ using 6 exponentials — one order better than the group commutator $S_2(x)$ (4 exponentials, error $O(x^3)$).

**Equation (26) — Sum + commutator at no extra cost:**
$$f_{R}(\alpha/n)^n = e^{\alpha(A+B) + \beta[A,B] + O((\alpha\beta + \beta^2)/n)}$$
using $6n$ gates. The linear terms ride along for free.

**Table II — Recursive formula comparison:** Four new recursive constructions improving over [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs-Wiebe 2013]]. Best performer ($\sqrt{10}$-copy $G$): roughly $10\times$ fewer gates than the previous best ($\tilde{V}_4$) at accuracy $10^{-4}$ for the single-qubit benchmark.

**Existence result (Appendix B):** The $\sqrt{4}$-copy recursive formula exists for all odd orders $n = 2k-1$ — proven via monotonicity of solution curves in the $(c,d)$ parameter space.

## Comparison with prior work

| Aspect | [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes\|Childs-Wiebe 2013]] | This paper |
|---|---|---|
| Base formula | $S_2(x)$: 4 gates, 2nd order | $S_3(x)$: 6 gates, 3rd order |
| Best recursion | $\sqrt{6}$-copy (Theorem 2): $6N_{2k} - 2$ | $\sqrt{10}$-copy ($G$): $10N_m - 4$; best numerics |
| Sum + commutator | Not addressed | Free: include $A+B$ at no extra gate cost |
| Applications | Abstract (quantum control, search lower bound) | Concrete: counterdiabatic driving, lattice models, FQH |
| Gate count to $10^{-4}$ | $\sim 200$ gates ($\tilde{V}_4$, numerics) | $\sim 56$ gates ($G_5$, numerics for $x \in [0.1, 0.3]$) |

## Applications

### Counterdiabatic driving

For adiabatic evolution $H(\lambda(t))$, the counterdiabatic correction is $H_{\text{CD}} = H(\lambda) + \dot{\lambda} C_\lambda$ where $C_\lambda \approx i c_1(\lambda)[H, \partial_\lambda H]$. The commutator structure means the sum-and-commutator formula implements counterdiabatic driving *at the same gate count as standard Trotterization*. Numerical example: 2-qubit model with $J = -1$, $h_z = 5$; the CD protocol maintains ground-state fidelity $> 0.99$ while standard Trotter deviates after $t \approx 0.6\tau$.

### 1D fermion chain — NNN hopping from NN terms

For a chain with NN hopping split as $H_0$ (even bonds) and $H_1$ (odd bonds), the commutator $[H_0, H_1]$ generates NNN hopping $c_j^\dagger c_{j+2}$ with alternating $\pm i$ phases. This corresponds to $\pi/2$ flux insertion per triangle. In qubit language, NNN hopping maps to 3-qubit interactions $\sigma_j^+ \sigma_{j+1}^z \sigma_{j+2}^-$ — relevant to [[Hamiltonian simulation|lattice gauge theories]]. Gate cost: $3nL$ (same scaling as NN-only Trotter).

### Fractional quantum Hall — Kapit-Mueller model

The Kapit-Mueller Hamiltonian on a square lattice with magnetic flux $\phi$ requires NNN hopping with specific phase factors. The paper shows these can be generated from commutators of NN hopping terms: $-i[H_1 - H_2, H_3 - H_4]$ produces the correct NNN phase pattern, with $J'$ tuned to flatten the lowest band. This is a concrete quantum simulation protocol for topological phases using only NN gates.

## Limits / caveats

- The $S_3$ formula involves irrational coefficients ($\sqrt{5}$), which in a digital quantum circuit must be approximated to finite precision. The paper doesn't quantify the impact of this rounding.
- Numerical comparisons are on a single-qubit benchmark ($A = -i\sigma_x$, $B = -i\sigma_z$). Scaling to many-body systems is discussed qualitatively but not benchmarked numerically.
- The $\sqrt{10}$-copy formula uses 56 exponentials per step, which is significant circuit depth. For near-term hardware, the $\sqrt{4}$-copy (21 per step) may be more practical despite worse asymptotics.
- The sum-and-commutator formula requires $R \gg 1$ (equivalently, small step size) for the approximate solution Eq. (19) to be accurate. For moderate step sizes, one must solve the polynomial equations numerically.
- No rigorous error analysis for the applications — the counterdiabatic and FQH examples are purely numerical demonstrations.

## Reusable ideas

1. **[[Operator Differential Method for Commutator Product Formulas]]** — systematic derivation of product formula coefficients by expanding $e^{\Phi(x)}$ via the operator differential $\delta_A O = [A, O]$. Produces polynomial equations in the coefficients $p_i$ that can be solved to any desired order.

2. **[[Free Sum-and-Commutator Simulation]]** — when simulating $e^{\alpha(A+B) + \beta[A,B]}$, the linear terms $A + B$ can be included at no extra gate cost beyond the commutator formula. The key: reparameterise via $R = \beta n / \alpha^2$ and take $n$ steps.

3. **[[NNN Hopping from NN Commutators]]** — generate next-nearest-neighbour hopping terms from commutators of nearest-neighbour hopping operators: $[c_i^\dagger c_j + \text{h.c.}, c_j^\dagger c_k + \text{h.c.}] = c_i^\dagger c_k - c_k^\dagger c_i$. Extends to 2D lattices for producing specific flux patterns.

## References within this paper

- **[[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs-Wiebe (2013)]]** — direct predecessor; the recursive constructions in Sec. III all build on or improve the $\sqrt{6}$-copy and 5-copy recursions from this paper
- **Jean-Koseleff (1997)** — the 3-copy recursive formula (Eq. 36); one of two prior recursive approaches
- **Hatano-Suzuki (2005)** — the operator differential method used to derive the base formula; also the foundation for Suzuki's recursion for sum formulas
- **[[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]]** — original quantum simulation proposal via first-order Trotter
- **[[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]]** — tight commutator-scaling Trotter error bounds; cited for the error analysis framework
- **Sels-Polkovnikov (2017)** — variational approach to counterdiabatic driving via nested commutators; the CD application builds on this
- **Kapit-Mueller (2010)** — the lattice FQH model simulated in Sec. V C

## Cross-links

### Paper notes
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]] — direct predecessor; this paper improves the base formula and all recursive constructions
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) �� Paper Notes]] — commutator-scaling error analysis framework
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — uses commutator correctors at boundaries; related approach to error reduction
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — Suzuki recursion for ordered exponentials
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]] — adiabatic simulation; the counterdiabatic application connects to this

### Trick cards
- [[Operator Differential Method for Commutator Product Formulas]]
- [[Free Sum-and-Commutator Simulation]]
- [[NNN Hopping from NN Commutators]]
- [[Recursive Group-Commutator Product Formula]] — the Childs-Wiebe 2013 recursion that this paper improves upon
- [[Anticommutator Simulation via Qubit Dilation]] — related technique from Childs-Wiebe 2013 for many-body terms
- [[Order-Condition Cancellation in Product Formulas]] — the polynomial equation approach used here is a specific instance
