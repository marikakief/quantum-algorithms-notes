> **Source:** Andrew M. Childs and Nathan Wiebe, *Product Formulas for Exponentials of Commutators*, arXiv:1211.4945, J. Math. Phys. **54**, 062202 (2013)
> **Links:** [arXiv](https://arxiv.org/abs/1211.4945) · [Journal](https://doi.org/10.1063/1.4811386)
> **Tags:** #hamiltonian-simulation #product-formula #commutator #quantum-control #lower-bounds

---

## The computational problem

Given anti-Hermitian operators $A$ and $B$ (and generalizations to nested commutators), and a target total evolution time $T$, implement $e^{[A,B]T}$ to error $\varepsilon$ using only exponentials of $A$ and $B$. Count the number of such elementary exponentials as the cost.

The problem generalizes to: given a sequence $A_0, \ldots, A_k$, implement $e^{Z_k T^{k+1}}$ where $Z_k = [A_k, [A_{k-1}, \cdots [A_1, A_0]\cdots]]$ is a nested commutator.

## What the paper does

Provides a recursive construction — the first systematic one — for building product formula approximations to exponentials of commutators (and nested commutators) to arbitrary order. The cost scales as $(\Lambda T)^{k+1+o(1)}$ in total evolution time, where $\Lambda \geq 2\max_j \|A_j\|$ and $k$ is the nesting depth of the commutator. A quantum search reduction shows this is nearly optimal: any such product formula requires $\Omega(T)$ exponentials.

Applications include quantum control (arbitrary unitary through commutator evolution) and simulating many-body Hamiltonians with higher-order coupling terms (products of commuting operators).

## The algorithm / construction

There are two main recursion tracks depending on the parity of $k$ (the commutator degree minus 1), plus a generalization to nested commutators.

### Odd-$k$ recursion: the $V_{p,k}$ family

**Base case (Lemma 1).** The group commutator gives
$$
V_{1,k}(At, Bt^k) := e^{At} e^{Bt^k} e^{-At} e^{-Bt^k} = e^{[A,B]t^{k+1}} + O(t^{k+2}).
$$
This is the 4-exponential starting point, accurate to order $k+1$.

**Recursion (Theorem 2).** Given $V_{p,k}$ satisfying $V_{p,k}(At, Bt^k) = e^{[A,B]t^{k+1}} + O(t^{2p+k})$, define the 6-fold symmetric composition:
$$
V_{p+1,k}(At, Bt^k) := V_{p,k}(\gamma_p t) \, V_{p,k}(-\gamma_p t) \cdot V_{p,k}(\beta_p t)^{-1} V_{p,k}(-\beta_p t)^{-1} \cdot V_{p,k}(\gamma_p t) \, V_{p,k}(-\gamma_p t),
$$
where $\beta_p = (2r_p)^{1/(k+1)}$, $\gamma_p = (\tfrac{1}{4} + r_p)^{1/(k+1)}$, and $r_p$ is chosen to cancel the leading error term. Then
$$
V_{p+1,k}(At, Bt^k) = e^{[A,B]t^{k+1}} + O(t^{2(p+1)+k}).
$$
Each recursion multiplies the exponential count by 6, giving $6^{p-1} \cdot 4$ exponentials for $V_{p,k}$ (rough count).

**Symmetrization (Corollary 3, $k=1$ only).** Since $V_{p,1}$ has odd error terms, compose:
$$
V'_{p,1}(A, B; t) := V_{p,1}(At/\sqrt{2}, Bt/\sqrt{2}) \, V_{p,1}(-At/\sqrt{2}, -Bt/\sqrt{2}),
$$
which kills the next odd-order error term and doubles the exponential count. This gives $8 \cdot 6^{p-1}$ exponentials for $V'_{p,1}$.

### Even-$k$ recursion: the $W_{p,k}$ family

**Base case (Lemma 4).** For even $k$, a 5-term Strang-like formula:
$$
W_{1,k}(At, Bt^k) := e^{At\xi_k} e^{B(\xi_k t)^k} e^{-2At\xi_k} e^{-B(\xi_k t)^k} e^{At\xi_k},
$$
where $\xi_k = 2^{-1/(1+k)}$. This achieves error $O(t^{k+3})$ (one order better than the odd-$k$ base, which starts at $O(t^{k+2})$).

**Suzuki-like recursion (Theorem 5).** Given $W_{p,k}$ with the symmetry $W_{p,k}(-At, Bt^k) = W_{p,k}(At, Bt^k)^{-1}$, define
$$
W_{p+1,k}(At, Bt^k) := W_{p,k}(\nu_p t)^2 \, W_{p,k}(-\mu_p t) \, W_{p,k}(\nu_p t)^2,
$$
where $\mu_p, \nu_p$ cancel the leading error (explicit values in Eq. (24) of the paper). Cost: $5^p$ exponentials for $W_{p,k}$.

### Nested commutators (Lemma 6)

To implement $e^{Z_k T^{k+1}}$ for $Z_k = [A_k, [A_{k-1}, \cdots [A_1, A_0]\cdots]]$, recursively replace each inner commutator exponential in the outer formula with the appropriate product formula approximation of the inner commutator. This yields $U_p(A_k t, \ldots, A_0 t)$ satisfying
$$
U_p(A_k t, \ldots, A_0 t) = e^{Z_k t^{k+1}} + O(t^{2p+k+1}).
$$
Exponential count scales as $O(6^{pk})$ (rough; see Section III A of the paper).

### Alternative scheme (Lemma 7, Definition 2)

A Jean–Koseleff-inspired scheme builds formulas $G_p$ that raise order by $\tfrac{1}{2}$ per step (via a 5-term symmetric composition) rather than by 1. This gives different tradeoffs — sometimes fewer exponentials in certain regimes — and may be preferred when the nesting depth $k$ is large.

### Segmented simulation and overall complexity (Corollary 11)

To simulate total evolution time $T$, divide into $r$ segments each of step size $t = T/r^{1/(k+1)}$ and apply $U_p$ or $V_{p,k}$ to each. Optimizing $p$ and $r$ jointly gives total exponential count:
$$
N_{\rm exp} \in (\Lambda T)^{k+1} \cdot (\Lambda T / \varepsilon)^{o(1)}.
$$
The cost is near-linear in $(\Lambda T)^{k+1}$ and only sub-polynomial in $1/\varepsilon$.

## Key results

**Theorem 2 (odd-$k$ recursion):** $V_{p+1,k}$ approximates $e^{[A,B]t^{k+1}}$ with error $O(t^{2(p+1)+k})$, using a 6-fold composition of $V_{p,k}$.

**Theorem 5 (even-$k$ recursion):** $W_{p+1,k}$ approximates $e^{[A,B]t^{k+1}}$ with error $O(t^{2(p+1)+k+1})$, using a 5-fold Suzuki composition.

**Corollary 11 (overall scaling):**
$$
N_{\rm exp} = (\Lambda T)^{k+1} \cdot (\Lambda T / \varepsilon)^{O(1/p)} \xrightarrow{p\to\infty} (\Lambda T)^{k+1+o(1)}.
$$

**Theorem 12 (lower bound — near-optimality):** Any product formula approximation to $e^{[A,B]T}$ for generic $A, B$ requires $\Omega(T)$ elementary exponentials. The lower bound is established via reduction to the $\Omega(\sqrt{n})$ quantum search lower bound.

**Theorem 13 (many-body couplings):** For commuting Hermitian operators $\{A_\ell\}$ with multiplicities $\alpha_\ell$, the operator $\exp(-i \, 2^{-k} A_1^{\alpha_1} \cdots A_m^{\alpha_m} t^{k+1})$ (where $k = \sum_\ell \alpha_\ell - 1$) can be simulated using $O(6^{pk} (6^{pk} \Lambda t)^{k+1+(k+1)^2/(2p)} \varepsilon^{-(k+1)/(2p)})$ exponentials.

## Comparison with prior work

| What | Prior | This paper |
|---|---|---|
| Simulating $e^{[A,B]T}$ | Ad hoc; no systematic approach | Recursive, arbitrary-order, nearly-optimal |
| Order of approximation | Limited (no recursion) | Arbitrary via $p\to\infty$ |
| Complexity scaling | Unknown (first systematic treatment) | $(\Lambda T)^{k+1+o(1)}$ |
| Optimality | No lower bound | $\Omega(T)$ lower bound; near-optimal |
| Many-body $k$-body terms | Not addressed | Handled via qubit dilation trick |
| Quantum control | No product formula approach | New family of evolutions for control |

The [[Balanced Group Commutator Decomposition]] trick from Solovay–Kitaev uses group commutators for gate synthesis, but only one level deep. This paper's recursion goes to arbitrary order.

## Limits / caveats

The complexity $O(6^{pk})$ grows exponentially in both the recursion depth $p$ and the nesting depth $k$. For $k \geq 2$ the practical constant factors are large. The near-linearity in $T^{k+1}$ only emerges after optimizing $p$ in the large-$T$ regime.

The lower bound (Theorem 12) shows the $(\Lambda T)^{k+1}$ factor is genuine. The $o(1)$ precision exponent is nearly but not fully optimal — there is a polylog$(1/\varepsilon)$ gap versus the $O(1)$ precision dependence achieved by LCU/[[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] methods for direct Hamiltonian simulation.

This is a 2013 paper predating the [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP/qubitization]] era; the methods here are product-formula-only. For simulating $e^{HT}$ directly, [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Taylor series / LCU]] achieves polylog$(1/\varepsilon)$. The commutator-simulation setting is distinct: when you only have access to $e^{At}$ and $e^{Bt}$ but want $e^{[A,B]T}$, this paper gives the right tool.

## Reusable ideas

1. **[[Recursive Group-Commutator Product Formula]]** — build high-order approximations to $e^{[A,B]T}$ by recursively composing group-commutator building blocks, raising the approximation order by 2 per recursion step (odd-$k$ case).

2. **[[Suzuki-Like Recursion for Even-k Commutator Exponentials]]** — for even nesting degree, use a 5-term symmetric Suzuki-style composition starting from a 5-term base formula; achieves the same order-raising with better initial accuracy.

3. **[[Anticommutator Simulation via Qubit Dilation]]** — embed commuting operators $A_\ell$ in a larger Hilbert space ($H \otimes \mathbb{C}^2$) so that the commutator of the embedded operators reproduces the product $A_1^{\alpha_1} \cdots A_m^{\alpha_m}$; this converts many-body interaction simulation into nested commutator simulation.

4. **[[Quantum Search Lower Bound for Commutator Simulation]]** — prove near-optimality of commutator simulation by reducing to quantum search: evolving under $[|w\rangle\langle w|, |+\rangle\langle +|]$ for time $T = O(\sqrt{n})$ solves the $n$-item search problem, so any product formula using $o(T)$ exponentials gives fewer than $\Omega(\sqrt{n})$ oracle queries — a contradiction.

## References within this paper

- **Suzuki (1990, 1991)** — higher-order Trotter–Suzuki product formulas; basis of the even-$k$ recursion in this paper. Vault note: [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] has context.
- **Solovay–Kitaev** — uses one level of group commutator for gate approximation. Vault: [[Balanced Group Commutator Decomposition]], [[Group Commutator Error Cancellation]].
- **Grover / quantum search lower bound** — the $\Omega(\sqrt{n})$ query lower bound (Bennett et al.) is the basis for Theorem 12.
- **Jean and Koseleff (2003)** — alternative product formula recursion that raises order by $1/2$ per step, which inspires the $G_p / F_p$ family in Section III B.
- **Berry–Ahokas–Cleve–Sanders (2005)** — Suzuki product formulas for sparse Hamiltonian simulation; [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] is the evolution.
- **Childs–Wiebe (2012)** — precursor paper on LCU; vault: [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]].

## Cross-links

### Paper notes
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]] — same authors, contemporary work; the LCU paper goes in a different direction (coherent sums), while this paper stays with product formulas
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — later work making Trotter error bounds tight via nested commutator structure; the [[Finite Nested-Commutator Expansion]] trick is conceptually related
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]] — uses product formula ideas including multi-product formulas; Marika is a co-author
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — product formula era successor that eventually got superseded by Taylor/QSP
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — the method that eventually beats product formula precision scaling
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — higher-order Suzuki for time-dependent Hamiltonians; methodologically adjacent

### Trick cards
- [[Recursive Group-Commutator Product Formula]]
- [[Suzuki-Like Recursion for Even-k Commutator Exponentials]]
- [[Anticommutator Simulation via Qubit Dilation]]
- [[Quantum Search Lower Bound for Commutator Simulation]]
- [[Balanced Group Commutator Decomposition]]
- [[Group Commutator Error Cancellation]]
- [[Finite Nested-Commutator Expansion]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Vandermonde Compilation of Nested Commutator Exponentials]]

### Subsequent work
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes]] — extends the commutator product-formula compilation to systematically build correctors for Suzuki/Yoshida product formulas; uses Vandermonde systems to determine compilation coefficients
