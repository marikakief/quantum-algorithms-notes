
> **Source:** Childs & Wiebe, arXiv:1211.4945, J. Math. Phys. 54, 062202 (2013)
> **Tags:** #trick #product-formula #commutator #suzuki #recursion

## What it does

Provides a product-formula recursion for approximating $e^{[A,B]t^{k+1}}$ when $k$ is even. Unlike the [[Recursive Group-Commutator Product Formula|odd-$k$ recursion]] which uses a 6-fold group commutator, this uses a Suzuki-style 5-fold composition from a symmetric base formula. It achieves the same order-raising property with 5-fold (not 6-fold) growth in exponential count.

## The trick

**Base (Lemma 4):** For even $k$, define $\xi_k = 2^{-1/(1+k)}$ and
$$
W_{1,k}(At, Bt^k) := e^{At\xi_k}\, e^{B(\xi_k t)^k}\, e^{-2At\xi_k}\, e^{-B(\xi_k t)^k}\, e^{At\xi_k}.
$$
This is a 5-exponential formula satisfying
$$
W_{1,k} = e^{[A,B]t^{k+1}} + O(t^{k+3}).
$$
The base formula is already more accurate (by one order: $O(t^{k+3})$ vs $O(t^{k+2})$) than the [[Recursive Group-Commutator Product Formula|odd-$k$ base]] because the symmetry of even-$k$ commutators allows a Strang-splitting-style construction that cancels the $O(t^{k+2})$ term.

**Key symmetry:** $W_{1,k}(-At, Bt^k) = W_{1,k}(At, Bt^k)^{-1}$. This antisymmetry in $A$ is what makes the Suzuki recursion work.

**Recursion (Theorem 5):** Given $W_{p,k}$ satisfying the antisymmetry condition and error $O(t^{2p+k+1})$, define
$$
W_{p+1,k}(At, Bt^k) := W_{p,k}(\nu_p t)^2\, W_{p,k}(-\mu_p t)\, W_{p,k}(\nu_p t)^2,
$$
where $\mu_p$ and $\nu_p$ satisfy $2\nu_p - \mu_p = 1$ and cancel the leading error term (see Eq. (24) in the paper). Then
$$
W_{p+1,k} = e^{[A,B]t^{k+1}} + O(t^{2(p+1)+k+1}).
$$
Exponential count: $5^p$ (since the base has 5 and each recursion is 5-fold).

## When to reach for it

- Same setup as [[Recursive Group-Commutator Product Formula]], but the nesting degree $k$ of the commutator is even.
- Slightly more efficient than the odd-$k$ path (5-fold vs 6-fold growth) when applicable.
- The better base accuracy ($O(t^{k+3})$ vs $O(t^{k+2})$) means fewer recursion levels may be needed for the same target order.

## Complexity

$5^p$ exponentials for $W_{p,k}$. Overall simulation cost (after segment optimization) is the same $(\Lambda T)^{k+1+o(1)}$ as the odd-$k$ path.

## Caveat

Only applies when $k$ is even. The antisymmetry condition $W(-At, Bt^k) = W(At, Bt^k)^{-1}$ is preserved by the recursion (provable by induction) but must be verified for the base.

## Related notes

- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]
- [[Recursive Group-Commutator Product Formula]]
- [[Anticommutator Simulation via Qubit Dilation]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
