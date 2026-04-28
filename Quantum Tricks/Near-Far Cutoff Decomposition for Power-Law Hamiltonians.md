# Near-Far Cutoff Decomposition for Power-Law Hamiltonians

> **Tags:** #trick #trotter #power-law #locality #Lieb-Robinson #hamiltonian-simulation
> **Source:** Tran, Guo, Su, Garrison, Eldredge, Foss-Rasmussen, Childs, Gorshkov, arXiv:1808.05225, PRX **9**, 031006 (2019)

## What it does

Splits a power-law Hamiltonian $H = \sum_{i<j} J_{ij}/r_{ij}^\alpha \, h_{ij}$ into a "near" part (pairs within distance $\ell$) and a "far" part (pairs at distance $> \ell$), then simulates them differently. The cutoff $\ell$ is tuned to match the $\alpha$-dependent Lieb-Robinson light cone, yielding gate counts that interpolate smoothly between the nearest-neighbour regime ($\alpha \to \infty$) and the all-to-all regime ($\alpha \le D$).

## The trick

**Decompose:**

$$H = H_{\mathrm{near}}(\ell) + H_{\mathrm{far}}(\ell), \qquad H_{\mathrm{near}} = \sum_{r_{ij} \le \ell} \frac{J_{ij}}{r_{ij}^\alpha} h_{ij}$$

**Simulate each part:**

- $H_{\mathrm{near}}$: Trotterise. It has $O(n \ell^{D-1})$ terms in $D$ dimensions; Trotter error is controlled by commutator norms that only involve pairs within $\ell$ of each other.

- $H_{\mathrm{far}}$: The tail is small in norm:
$$\|H_{\mathrm{far}}\| \le C \, n \, \sum_{r > \ell} r^{D-1-\alpha} \le C' \, n \, \ell^{D-\alpha}$$
(converges for $\alpha > D$). Simulate exactly or via LCU; the small norm makes the cost cheap per step.

**Tune $\ell$:** The total gate count has the form:

$$\text{gates} \sim n \ell^{D-1} \cdot r_{\mathrm{Trotter}} + \text{(far-part cost)}$$

where $r_{\mathrm{Trotter}} \propto (n \ell^{D-\alpha} t^2 / \epsilon)^{1/p}$ from the commutator bound on the near part. Optimising $\ell$ gives the $\alpha$-dependent gate count:
- $\alpha > 2D$: choose $\ell = O(1)$ → NN-equivalent cost.
- $D < \alpha \le 2D$: choose $\ell \sim n^{D/(\alpha - D)}$ → intermediate cost.
- $\alpha \le D$: the decomposition gives no gain; simulate all-to-all.

**The Lieb-Robinson link:** The optimal cutoff $\ell$ equals the Lieb-Robinson light-cone radius at time $t/r$ (one Trotter step duration). Terms outside the light cone cannot contribute appreciably to the simulation error in a single step; grouping them into the far part is exactly what the LR bound permits.

## When to reach for it

- Simulating any $1/r^\alpha$-decaying Hamiltonian (trapped ions, Rydberg arrays, dipolar molecules, van der Waals interactions) by product formulas.
- Deriving $\alpha$-dependent gate count bounds: the near/far split gives the right $n$-scaling for all $\alpha > D$.
- Arguing that long-range interactions with $\alpha > 2D$ cost no more than nearest-neighbour: choose $\ell = O(1)$, the far part is negligible, and the simulation is purely local.
- Setting up an interaction-picture simulation where the far part provides the free evolution and the near part is Trotterised.

## Complexity

For a 1D ($D=1$) system, first-order product formula:

| $\alpha$ | Optimal $\ell$ | Gate count |
|---|---|---|
| $\alpha > 2$ | $O(1)$ | $\tilde{O}(n t^2/\epsilon)$ |
| $1 < \alpha \le 2$ | $O(n^{1/(\alpha-1)})$ | $\tilde{O}(n^{1+1/\alpha} t^2/\epsilon)$ |
| $\alpha \le 1$ | No gain | $\tilde{O}(n^2 t^2/\epsilon)$ |

## Caveat

- **Requires $\alpha > D$** for the far-part norm to converge. For $\alpha \le D$ the split provides no advantage.
- **Assumes isotropic decay.** Anisotropic power laws (different exponents in different spatial directions) require separate analysis.
- **Far-part simulation cost must be accounted for.** The standard argument treats the far part as "free," but in a real implementation the far-part gates (especially for intermediate $\alpha$) can dominate.
- **Not tight for high-order formulas.** The near/far split is most natural for first- and second-order product formulas; higher-order formulas may benefit from a more recursive decomposition (see [[Recursive Interval Decomposition for Power-Law Hamiltonians]]).

## Related notes

- [[Locality and Digital Quantum Simulation of Power-Law Interactions (Tran-Guo-Su-Childs-Gorshkov 2019) — Paper Notes]]
- [[Lieb-Robinson Decomposition for Lattice Simulation]]
- [[Commutator-Sensitive Lieb-Robinson Bound]]
- [[Recursive Interval Decomposition for Power-Law Hamiltonians]]
- [[Interaction-Picture Split for Product Formulas]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes]]
