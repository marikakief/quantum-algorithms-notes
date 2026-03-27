# Order-Optimized Multiproduct Simulation

> **Source:** Low, Kliuchnikov, Wiebe, arXiv:1907.11679 (2019)
> **Tags:** #trick #multi-product #hamiltonian-simulation #order-optimization

## What it does

Converts the polynomial-in-$m$ cost of a single multiproduct step into a polylogarithmic overall cost by choosing the order $2m$ to scale sub-logarithmically with $t\lambda/\epsilon$.

## The trick

An order-$2m$ multiproduct formula has per-step error bounded by (using Stirling):
$$\epsilon_\Delta \leq \frac{2\|\vec{a}\|_1 |\Delta\lambda|^{2m+1}}{(2m+1)!}\,e^{|\Delta\lambda|}$$

After $r = t/\Delta$ steps, the total error $\leq \epsilon$ requires
$$r = t\lambda \cdot \max\!\left\{\left(\frac{8t\lambda\|\vec{a}\|_1}{\epsilon(2m+1)!}\right)^{\!1/(2m)},\;\frac{1}{\log 2}\right\}$$

With [[Chebyshev-Node Conditioning for Multiproduct Formulas|well-conditioned]] multiproduct formulas ($\|\vec{a}\|_1 \in O(\log m)$, $\|\vec{k}\|_1 \in O(m^2\log m)$), the total query cost is $r\|\vec{a}\|_1\|\vec{k}\|_1$.

**The optimisation:** Set $2m = e^z$ where $z = W(\log(t\lambda/\epsilon))$ is the Lambert-$W$ function satisfying $ze^z = \log(t\lambda/\epsilon)$.

Then:
- $(t\lambda/\epsilon)^{1/(2m)} = e^{ze^z/e^z} = e^z$ — the factorial denominator from Stirling ($1/(2m)! \sim e^{-2m\log(2m)}$) cancels the exponential numerator.
- $r \in \Theta(t\lambda)$ — the number of steps becomes independent of $\epsilon$.
- Per-step cost: $\|\vec{a}\|_1\|\vec{k}\|_1 \in O(z^2 e^{2z}) \subseteq O(\log^2(t\lambda/\epsilon))$.
- Total: $O(t\lambda \log^2(t\lambda/\epsilon))$ queries to $U_2$.

The Lambert-$W$ function grows very slowly: $W(x) \sim \log x - \log\log x$, so the order $2m \approx e^{W(\log(t\lambda/\epsilon))}$ is sub-logarithmic in $t\lambda/\epsilon$. This is why the multiproduct method achieves polylogarithmic $\epsilon$-scaling despite each individual multiproduct formula having polynomial cost in its order.

## When to reach for it

- Any approximation scheme parametrised by an order $m$ where: (a) the per-step cost is polynomial in $m$ (here $O(m^2\log m)$), and (b) the per-step error decays as $O(\alpha^{2m+1}/(2m+1)!)$ (factorial suppression). The factorial denominator is what makes the Lambert-$W$ trick work.
- Similar order-optimisation appears in truncated Taylor series simulation (Berry et al. 2015) and Dyson series methods ([[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe 2018]]).

## Complexity

Total: $O(t\lambda\log^2(t\lambda/\epsilon))$ controlled-$U_2$ queries. An additional sub-logarithmic factor can be shaved using the programmable rotation model (Appendix B of the paper), reducing the per-step cost from $\|\vec{k}\|_1$ to $\max_j k_j$.

## Caveat

The Lambert-$W$ optimisation gives the best asymptotic cost but requires choosing $m$ as a function of $t$, $\lambda$, and $\epsilon$ — all of which must be known (or bounded) in advance. In practice (as in the paper's benchmarks), one typically scans over tabulated multiproduct formulas of different orders and picks the cheapest for the given parameters.

Also, the $\log^2$ overhead is worse than [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]'s $O(t\|H\| + \log(1/\epsilon)/\log\log(1/\epsilon))$. The multiproduct formula trades a logarithmic factor for commutativity sensitivity — a good trade when $H$ has significant commutator structure.

## Related notes

- [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes]] — source paper
- [[Chebyshev-Node Conditioning for Multiproduct Formulas]] — the conditioning result that makes polynomial per-step cost possible
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — analogous order optimisation for Taylor series truncation
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — analogous order optimisation in the interaction picture
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — the asymptotically better competitor
