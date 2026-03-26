# Regularized Coulomb Potential (Delta Cutoff)

> **Source:** Kivlichan, Wiebe, Babbush, Aspuru-Guzik, arXiv:1608.05696 (2017)
> **Tags:** #trick #coulomb #regularization #LCU #bounded-potential #real-space

## What it does

Replaces the singular Coulomb potential $1/\|x_i - x_j\|$ with the regularized form $1/\sqrt{\|x_i - x_j\|^2 + \Delta^2}$ to make the potential energy operator bounded. This is a prerequisite for any LCU decomposition of a real-space Coulomb Hamiltonian, where the unbounded singularity would otherwise require infinitely many LCU terms.

## The trick

For $\eta$ particles with charges $q_1, \ldots, q_\eta$ (maximum absolute charge $q$), replace:

$$V_\text{Coulomb}(x) = \sum_{i<j} \frac{q_i q_j}{\|x_i - x_j\|} \longrightarrow V_\Delta(x) = \sum_{i<j} \frac{q_i q_j}{\sqrt{\|x_i - x_j\|^2 + \Delta^2}}$$

This is bounded:

$$\|V_\Delta(x)\|_\infty \leq \frac{\eta(\eta-1)q^2}{2\Delta}$$

and smooth everywhere (no singularity at $x_i = x_j$).

The gradient bound, needed for discretization error analysis (Lemma 12), is:

$$\left|\frac{\partial V_\Delta}{\partial x_{i,n}}\right| \leq \frac{\eta^2 q^2 \sqrt{3}}{9\Delta^2}$$

**LCU decomposition:** With the bounded potential, the scaled potential $M V_\Delta$ can be approximated to precision $\delta$ by a sum of $M \geq \|V_\Delta\|_\infty / \delta$ signature matrices (diagonal $\pm 1$ unitaries). For a target simulation error $\varepsilon$, choose $\delta \in O(\varepsilon/t)$, giving $M = O(\eta^2 q^2 t / (\Delta\varepsilon))$ signature matrices.

**Physical interpretation:** $\Delta$ is the closest distance between two particles that is resolved. For $\Delta > 0$, the potential is well-behaved but deviates from the true Coulomb by $O(q^2/\Delta)$ at short distances. For bound-state energy estimation, $\Delta$ must be small enough that the energy error from regularization is within the target precision.

## When to reach for it

- Any quantum simulation that uses the Coulomb interaction in real space (chemistry, nuclear physics, plasma physics, cold atoms with $1/r$ interactions)
- As a prerequisite before applying [[Truncated Taylor Series Simulation]] or any LCU-based method to a real-space Coulomb Hamiltonian
- When decomposing the potential energy into signature matrices for the BCCKS/LCU simulation framework
- Also useful for classical algorithms: the regularized Coulomb is standard in density functional theory (exchange-correlation functionals), plasma simulations, and $N$-body codes

## Complexity

- **Potential norm:** $\|V_\Delta\|_\infty = O(\eta^2 q^2 / \Delta)$
- **Number of signature matrices needed:** $M = O(\eta^2 q^2 / (\Delta \delta))$ for approximation error $\delta$
- **Contribution to total LCU 1-norm:** $O(\eta^2 q^2 / \Delta)$, which feeds into the number of segments $r \propto V_\text{max} t$
- **Energy approximation error:** $O(q^2/\Delta)$ per pair; to get within total error $\varepsilon$, need $\Delta \ll q^2/\varepsilon$ (which can be small, requiring large $M$)

## Caveat

- The regularization error is not zero — it physically removes the short-range Coulomb cusp. For ground-state energy calculations, the cusp condition matters (the wave function has a kink at $x_i = x_j$, contributing significantly to the kinetic energy expectation value). With $\Delta > 0$ this cusp is smoothed, potentially biasing the energy estimate.
- The trade-off between $\Delta$ (regularization, controlling potential norm) and simulation precision $\varepsilon$ is nontrivial: smaller $\Delta$ gives more accurate physics but larger $\|V_\Delta\|$ and thus higher simulation cost.
- This approach is specific to real-space/position-basis simulation. Second-quantized and CI methods ([[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. 2018]]) use orbital integrals that are already finite (the singularity is absorbed into the integrals analytically via the chosen orbital basis).

## Related notes
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]
- [[Real-Space Grid Encoding for Many-Body Simulation]]
- [[High-Order Finite Difference Kinetic Energy]]
- [[Truncated Taylor Series Simulation]]
