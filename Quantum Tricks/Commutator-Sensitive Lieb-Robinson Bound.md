# Commutator-Sensitive Lieb-Robinson Bound

> **Source:** Haah, Hastings, Kothari, Low, arXiv:1801.03922 (Appendix C)
> **Tags:** #trick #Lieb-Robinson #commutator #lattice #information-propagation #near-commuting

## What it does

Provides a Lieb-Robinson bound where the effective velocity scales as $\sqrt{\eta}$ when the Hamiltonian terms have small pairwise commutators $\|[h_X, h_Y]\| \leq 2\eta \|h_X\|\|h_Y\|$. In the exactly commuting limit ($\eta \to 0$), the Lieb-Robinson velocity goes to zero.

## The trick

For a Hamiltonian $H = \sum_X h_X$ with commutator bound $\|[h_X, h_Y]\| \leq 2\eta\|h_X\|\|h_Y\|$ and exponential decay condition $\sum_{X \ni x} \|h_X\||X|^2 e^{\mu\,\mathrm{diam}(X)} \leq \zeta$, the propagation bound is:

$$\|[A(t), B]\| \leq \frac{2}{\sqrt{\eta}}\|A\|\|B\|\left(\exp\left(\zeta|t|\sqrt{8\eta}\right) - 1\right)\sum_{x \in X} e^{-\mu\,\mathrm{dist}(x,Y)}$$

The Lieb-Robinson velocity is $v_{\mathrm{LR}} \propto \zeta\sqrt{8\eta}/\mu$.

**Why $\sqrt{\eta}$ and not $\eta$?** Consider $h_X = 1 + \sqrt{\eta}\,h_X^0$ where $h_X^0$ has $O(1)$ norm and $O(1)$ Lieb-Robinson velocity. Then $\|[h_X, h_Y]\| = O(\eta)$ but the actual propagation speed is $O(\sqrt{\eta})$ — so the bound is tight.

The proof works by tracking two coupled quantities: $C_B(X,t) = \sup_{\|A\|\leq 1} \|[A(t), B]\|$ and $D_B(X,t) = \|[h_X(t), B]\|$. The commutator bound couples $D_B$ back to $C_B$ with a factor of $\eta$, giving an effective "doubled step" in the iterative expansion that produces the $\sqrt{\eta}$ factor.

**Strictly local case:** For terms with $\mathrm{diam}(X) \leq 1$ and commutator bound $\|[h_X, h_Y]\| \leq 2K$, the velocity is $O(d\sqrt{K})$ where $d$ is the graph degree — without any bound on individual $\|h_X\|$ required.

## When to reach for it

- Simulating near-commuting lattice Hamiltonians (e.g., perturbations of exactly solvable models, weakly interacting systems).
- Combined with the [[Lieb-Robinson Decomposition for Lattice Simulation]]: smaller $v_{\mathrm{LR}}$ means smaller overlap $\ell$ and fewer blocks, directly reducing gate count.
- Analysing information spreading in systems close to integrable points.
- Any Lieb-Robinson argument where you want tighter bounds than the standard $v_{\mathrm{LR}} = O(\zeta_0)$ for structured Hamiltonians.

## Complexity

Reduces the overlap size from $\ell = O(v_{\mathrm{LR}} t + \log(1/\epsilon))$ with standard Lieb-Robinson to $\ell = O(\sqrt{\eta}\,\zeta t / \mu + \log(1/\epsilon))$. For the lattice simulation algorithm, this gives improved gate counts proportional to $nT/\ell^D$ when $\eta \ll 1$.

## Caveat

- The $\sqrt{\eta}$ scaling is a worst-case bound over all Hamiltonians satisfying the commutator condition. Specific Hamiltonians may propagate even slower.
- Unlike standard Lieb-Robinson bounds which only need $\|h_X\|$ bounds, the commutator-sensitive version for exponentially decaying interactions requires the stronger condition $\sum_{X \ni x} \|h_X\||X|^2 e^{\mu\,\mathrm{diam}(X)} \leq \zeta$ (note the $|X|^2$ factor).
- No bound on higher-order commutators is assumed, but if available ($\|[[h_X, h_Y], h_Z]\| \leq \eta'$), even tighter bounds follow by tracking additional coupled quantities.

## Related notes
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes]]
- [[Lieb-Robinson Decomposition for Lattice Simulation]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — Trotter methods also benefit from small commutators
- [[Order-Condition Cancellation in Product Formulas]]
