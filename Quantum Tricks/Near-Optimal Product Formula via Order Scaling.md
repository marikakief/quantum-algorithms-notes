# Near-Optimal Product Formula via Order Scaling

> **Source:** Childs, Leng, Li, Liu, Zhang, arXiv:2112.05027 (Quantum 2022); also Childs, Su, arXiv:1901.00564 (PRL 2019)
> **Tags:** #trick #product-formulas #trotter #order-selection #near-optimal #real-space

## What it does

Achieves near-optimal Hamiltonian simulation complexity $(Nt)^{1+\delta}$ (for any $\delta > 0$) by choosing the Suzuki product formula order $p$ as a function of $N$, $t$, and $\varepsilon$, rather than fixing $p$ upfront.

## The trick

For a $p$-th order Suzuki formula applied to a Hamiltonian whose Trotter error scales with some commutator norm $\alpha$, the gate complexity is roughly

$$\text{Gates} = \tilde{O}\!\left(\alpha^{1/p} t (t/\varepsilon)^{1/p} \cdot 5^{p}\right)$$

(the $5^p$ comes from the number of Suzuki exponentials at order $2p$).

To minimize over $p$ for given $\alpha, t, \varepsilon$: set $p^* \approx \frac{1}{2} \log_5(\alpha t / \varepsilon)$. Substituting back gives

$$\text{Gates} = \tilde{O}\!\left(\alpha t \cdot (\alpha t / \varepsilon)^{o(1)}\right) = \tilde{O}(\alpha t) \cdot \text{subpoly}(\alpha t / \varepsilon)$$

For real-space simulation with $\alpha \sim N$ (from the [[Trotter Error via Nested Commutators for Real-Space Hamiltonians]] bound), this gives $\tilde{O}(Nt) \cdot \text{subpoly}(Nt/\varepsilon)$, i.e., $(Nt)^{1+\delta}$ for any $\delta > 0$.

The concrete statement: for any $\delta > 0$, set $p = \lceil 1/(2\delta) \rceil$. Then

$$r = \tilde{O}\!\left(N t (Nt/\varepsilon)^{1/p}\right) = \tilde{O}\!\left(Nt \cdot (Nt/\varepsilon)^{2\delta}\right)$$

Trotter segments suffice, with total gate complexity $\tilde{O}((Nt)^{1+2\delta})$. Since $\delta$ is arbitrary, $(Nt)^{1+o(1)}$ follows.

## When to reach for it

- Any simulation where the Trotter error scales as $\alpha_\text{comm}^{(p+1)} / r^p$ and $\alpha$ is large but the lower bound on queries is $\Omega(\alpha t / \text{poly}(\log))$.
- When you need to argue that product formulas are near-optimal for a class of Hamiltonians: show $\alpha_\text{comm} \sim \alpha$ and then apply this order-tuning argument.
- Lattice simulations ([[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]]) and real-space simulations ([[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]]) both use this argument.

## Complexity

Given commutator norm $\alpha$ at level 1 (first commutator), time $t$, error $\varepsilon$:
- Optimal order: $p^* \sim \frac{\log(\alpha t/\varepsilon)}{\log 5}$
- Gate complexity: $O(\alpha t \cdot e^{c \sqrt{\log(\alpha t / \varepsilon)}})$ — technically superlinear in $\alpha t$ but subpolynomial
- For any fixed $\delta > 0$: gates $= O((\alpha t)^{1+\delta} \varepsilon^{-O(\delta)})$ by choosing $p = \lceil 1/\delta \rceil$

## Caveat

- The $5^p$ (or $5^{p/2}$) factor from Suzuki recursion grows rapidly with $p$. For moderate $\alpha t/\varepsilon$, low-order formulas with larger $r$ may actually be cheaper than high-order formulas with fewer segments — the crossover depends on constants.
- "Near-optimal" means the exponent on $Nt$ approaches 1 as $p \to \infty$, not that the gate count approaches the lower bound in absolute terms. The subpolynomial factor $(\alpha t/\varepsilon)^{o(1)}$ can be large.
- Requires the Trotter error bound to be tight in $\alpha$ (i.e., the commutator norm actually controls the error, not just upper-bounds it). For some Hamiltonians, empirical Trotter performance is much better than the commutator bound.

## Related notes
- [[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]]
- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Trotter Error via Nested Commutators for Real-Space Hamiltonians]]
- [[Subnormalization Gap Between Product Formulas and Qubitization for Real-Space Hamiltonians]]
- [[Local Error Representation for Lattice Product Formulas]]
