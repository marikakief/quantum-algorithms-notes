
> **Source:** Wiebe, Berry, Høyer & Sanders, arXiv:1011.3489 (2011)
> **Tags:** #trick #oracle-model #precision #sparse #hamiltonian-simulation

## What it does

Cleanly separates the total simulation error $\varepsilon$ into two independent halves — integrator approximation error and finite-precision oracle round-off error — and derives the oracle precision requirements (bits for time mesh and matrix values) needed to keep each half $\leq \varepsilon/2$.

## The trick

Set $\varepsilon = \varepsilon_{\text{int}} + \varepsilon_{\text{disc}}$ with $\varepsilon_{\text{int}} = \varepsilon_{\text{disc}} = \varepsilon/2$. This decouples the two analyses:

**Integrator error** (Lemma 4): controlled by the Suzuki order $k$ and step size. Standard [[Smoothness-Order Saturation for Product Formulas|smoothness-order]] analysis.

**Discretization error** (Lemma 1): controlled by the number of bits $n_t$ (time mesh) and $n_H$ (matrix-value encoding). The key bounds are:

$$n_t \geq \left\lceil\log_2\!\left(\frac{\max_\mu\|\partial_t H_\mu\|_{\max}\cdot 32kMd^2(5/3)^{k-1}\Delta t^2}{\varepsilon}\right)\right\rceil$$

$$n_H \geq 2\left\lceil\log_2\!\left(\frac{32kMd^2(5/3)^{k-1}\Lambda_{\max}\Delta t}{\varepsilon}\right)\right\rceil + 6$$

Both grow as $O(\log(kMd^2\Lambda\Delta t/\varepsilon))$ — logarithmic in all simulation parameters. In particular, $n_t$ depends on the time-derivative of the Hamiltonian; if $H$ varies quickly, you need a finer time mesh to accurately interpolate matrix elements.

## When to reach for it

Any oracle-based simulation where you must explicitly account for finite-precision arithmetic. Especially when:
- Designing the oracle (deciding how many output bits it needs)
- Proving end-to-end complexity including implementation overhead
- The Hamiltonian has large time-derivatives that could make naive time-mesh choices expensive

## Complexity

Both $n_t$ and $n_H$ are $O(\text{poly-log})$ in problem parameters — they add a logarithmic overhead to gate counts but don't change the asymptotic scaling.

## Caveat

The $n_t$ bound depends on $\max_\mu\|\partial_t H_\mu\|$. If the Hamiltonian changes very rapidly (large derivatives), the time mesh needs to be fine even if the magnitudes $\|H_\mu\|$ are small. This is the same "badly-behaved derivatives" issue flagged in the abstract.

## Related notes
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes]]
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]]
- [[Smoothness-Order Saturation for Product Formulas]]
- [[Λ-Smoothness Parametrization]]
