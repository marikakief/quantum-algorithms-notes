
> **Source:** Childs & Wiebe, arXiv:1211.4945, J. Math. Phys. 54, 062202 (2013)
> **Tags:** #trick #product-formula #commutator #hamiltonian-simulation #recursion

## What it does

Builds a [[product formula]] of arbitrary approximation order for $e^{[A,B]T}$ using only elementary exponentials $e^{\pm At}$ and $e^{\pm Bt^k}$. At each recursion step the approximation order increases by 2, so you can get as high an order as desired by iterating.

## The trick

**Base (Lemma 1):** The group commutator
$$
V_{1,k}(At, Bt^k) := e^{At}\, e^{Bt^k}\, e^{-At}\, e^{-Bt^k} = e^{[A,B]t^{k+1}} + O(t^{k+2})
$$
is a 4-exponential formula accurate to leading order.

**Recursion (Theorem 2):** Given $V_{p,k}$ with error $O(t^{2p+k})$, construct
$$
V_{p+1,k}(t) := V_{p,k}(\gamma_p t)\, V_{p,k}(-\gamma_p t)\cdot V_{p,k}(\beta_p t)^{-1} V_{p,k}(-\beta_p t)^{-1}\cdot V_{p,k}(\gamma_p t)\, V_{p,k}(-\gamma_p t),
$$
where $\beta_p = (2r_p)^{1/(k+1)}$, $\gamma_p = (\tfrac{1}{4}+r_p)^{1/(k+1)}$, and $r_p$ is the unique solution that cancels the leading error term. Then
$$
V_{p+1,k}(t) = e^{[A,B]t^{k+1}} + O(t^{2(p+1)+k}).
$$

Each level multiplies the exponential count by 6; after $p$ levels from the base, the formula uses $4 \cdot 6^{p-1}$ exponentials (for $k > 1$; for $k=1$ an additional symmetrization step per Corollary 3 gives $8 \cdot 6^{p-1}$).

**For $k=1$ only (Corollary 3):** Compose $V_{p,1}(t/\sqrt{2})\, V_{p,1}(-t/\sqrt{2})$ to cancel the odd-order error term, raising from $O(t^{2p+1})$ to $O(t^{2p+2})$.

**Nested commutators (Lemma 6):** For $Z_k = [A_k, [\cdots[A_1,A_0]\cdots]]$, build $U_p$ by recursively replacing exponentials of inner commutators in the outer formula with their [[product formula]] approximations. The result satisfies $U_p = e^{Z_k t^{k+1}} + O(t^{2p+k+1})$.

## When to reach for it

- You have access to $e^{\pm At}$ and $e^{\pm Bt}$ but need $e^{[A,B]T}$ — e.g., you can directly implement each term of a commutator but not the commutator's exponential.
- Quantum control: any target Hamiltonian can in principle be reached through iterated commutators (if the Lie algebra closes), so this gives a constructive simulation route.
- Simulating $k$-body interactions that arise as products of simple terms; the [[Anticommutator Simulation via Qubit Dilation]] trick converts these to commutator form.

## Complexity

Total cost for simulating $e^{Z_k T^{k+1}}$ to error $\varepsilon$ using segmented simulation:
$$
N_{\rm exp} = (\Lambda T)^{k+1} \cdot (\Lambda T / \varepsilon)^{o(1)},
$$
where $\Lambda \geq 2\max_j \|A_j\|$. Near-linear in $(\Lambda T)^{k+1}$; sub-polynomial in $1/\varepsilon$.

## Caveat

The $6^{p-1}$ exponential count grows fast with recursion depth $p$, and optimizing the tradeoff between $p$ and the number of segments requires solving a system of equations. For $k \geq 2$ the constants become large. The method does not achieve polylog$(1/\varepsilon)$ — that requires LCU or [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]; this paper's lower bound (Theorem 12) shows the $(\Lambda T)^{k+1}$ factor is unavoidable.

## Related notes

- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]
- [[Suzuki-Like Recursion for Even-k Commutator Exponentials]]
- [[Anticommutator Simulation via Qubit Dilation]]
- [[Quantum Search Lower Bound for Commutator Simulation]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Balanced Group Commutator Decomposition]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
