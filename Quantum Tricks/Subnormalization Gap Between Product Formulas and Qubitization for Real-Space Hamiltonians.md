# Subnormalization Gap Between Product Formulas and Qubitization for Real-Space Hamiltonians

> **Source:** Childs, Leng, Li, Liu, Zhang, arXiv:2112.05027 (Quantum 2022)
> **Tags:** #trick #qubitization #product-formulas #real-space #subnormalization #block-encoding

## What it does

Explains why [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]]/QSP does **not** improve over high-order product formulas for real-space Hamiltonians, despite generally being the better method for Hamiltonian simulation — the subnormalization factor for the kinetic energy block-encoding scales as $\lambda_T \sim N^2$, which overwhelms the log-precision gain.

## The trick

Qubitization achieves gate complexity $O(\lambda t + \log(1/\varepsilon))$ where $\lambda$ is the block-encoding subnormalization (the LCU 1-norm of $H$). For product formulas, precision $\varepsilon$ enters as $\varepsilon^{-1/p}$ at order $p$, which for large $p$ is nearly 1.

For $H = T + V$ on a grid with $N$ points per dimension and grid spacing $h = L/N$:

- The maximum kinetic energy on the grid is $E_\text{max} \sim \frac{k_\text{max}^2}{2m} \sim \frac{\pi^2 N^2}{2m L^2}$. The block-encoding of $T$ requires subnormalization $\lambda_T \geq E_\text{max} \sim N^2$.
- The potential contributes $\lambda_V \sim \|V\|_\infty$, typically $O(1)$ or $O(\eta^2)$ for Coulomb.

So qubitization pays $\lambda \sim N^2$, giving gate complexity $O(N^2 t)$.

Product formulas, applied with QFT basis switching, directly exponentiate $e^{-iT\delta t}$ as a diagonal unitary in momentum space — they never construct a block-encoding of $T$. The kinetic energy contributes to the Trotter error only through commutators with $V$, not through the full spectrum width $\lambda_T$. This is why high-order product formulas scale as $O(N^{1+1/p})$ rather than $O(N^2)$.

The formal statement: for any $\delta > 0$, there exists a product formula order $p$ such that gates $= \tilde{O}((Nt)^{1+\delta})$, while qubitization costs $\Omega(N^2 t)$. The crossover is at $N \gg 1$, which is exactly the regime of interest for real-space simulation.

## When to reach for it

- Deciding whether to use qubitization or product formulas for a real-space (first-quantized, position-basis) simulation.
- Any setting where $H = T + V$ with $T$ diagonal in a QFT basis: the subnormalization gap argument applies whenever $\lambda_T \gg \alpha_\text{comm}^{1/p}$ at finite product formula order.
- Justifying why the general "qubitization is better" intuition breaks down for structured Hamiltonians with large spectrum width but small commutator norms.

## Complexity

| Method | Gate complexity | Dominant term |
|---|---|---|
| 2nd order Trotter | $\tilde{O}(N^{3/2} t^{3/2} \varepsilon^{-1/2})$ | $N$-scaling |
| $p$-th order Trotter | $\tilde{O}(N^{1+1/p} t^{1+1/p} \varepsilon^{-1/p})$ | decreasing in $p$ |
| $p \to \infty$ | $\tilde{O}((Nt)^{1+\delta})$ | Near-optimal |
| LCU / Taylor series | $\tilde{O}(N^2 t)$ | $\lambda_T = O(N^2)$ |
| Qubitization / QSP | $O(N^2 t + \log(1/\varepsilon))$ | $\lambda_T = O(N^2)$ |

## Caveat

- The comparison is asymptotic in $N$. For small $N$ (coarse grids) and high precision, qubitization's log-precision advantage may dominate.
- Product formulas with $p \gg 1$ have large constant prefactors from the Suzuki recursion ($5^{k-1}$ stages at order $2k$). The actual crossover gate count requires careful constant tracking.
- The argument is specific to the case where $T$ is diagonal in an efficiently implementable transformed basis (QFT). If $T$ has a different structure (e.g., non-uniform kinetic term), the analysis changes.
- In the non-oracle model, FMM-based methods ([[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]]) can do better than the $\Omega(Nt)$ oracle lower bound by exploiting Coulomb structure.

## Related notes
- [[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Trotter Error via Nested Commutators for Real-Space Hamiltonians]]
- [[Near-Optimal Product Formula via Order Scaling]]
- [[Basis Switching via QFT for Kinetic-Potential Splitting]]
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]
