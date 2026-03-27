
> **Source:** Kivlichan, Wiebe, Babbush, Aspuru-Guzik, arXiv:1608.05696 (2017)
> **Tags:** #trick #finite-difference #kinetic-energy #LCU #real-space #bounded-LCU-norm

## What it does

Approximates the Laplacian $\nabla^2$ using a $(2a+1)$-point centered finite difference formula of order $2a$, with the key property that the sum of the absolute values of the finite difference coefficients is bounded by $2\pi^2/3$ *independent of the order $a$*. This makes the kinetic energy LCU 1-norm bounded even as $a \to \infty$, enabling arbitrarily precise kinetic energy simulation at constant query overhead.

## The trick

The $(2a+1)$-point centered finite difference formula for a second derivative:

$$\frac{\partial^2 \psi}{\partial x^2} \approx h^{-2} \sum_{j=-a}^{a} d_{2a+1,j}\, \psi(x + jh)$$

where the coefficients are:

$$d_{2a+1, j \neq 0} = \frac{2(-1)^{a+j+1}(a!)^2}{(a+j)!(a-j)!j^2}, \qquad d_{2a+1, j=0} = -\sum_{j\neq 0} d_{2a+1,j}$$

**Lemma 6 (Kivlichan et al.):** For any $a \geq 1$:

$$\sum_{j=-a, j\neq 0}^{a} |d_{2a+1,j}| < \frac{2\pi^2}{3} \approx 6.58$$

*Proof sketch:* $(a+j)!(a-j)! \geq (a!)^2$ for $|j| \leq a$, so $|d_{2a+1,j}| \leq 2/j^2$, and $\sum_{j\neq 0}^{\infty} 2/j^2 = 2\pi^2/3$.

This means the kinetic energy, written as a linear combination of unitary adder operators $A_j$ (addition by $j$ modulo the grid):

$$M\tilde{T} = Mh^{-2} \sum_{i,n} \sum_{j=-a, j\neq 0}^{a} \frac{d_{2a+1,j}}{2m_i} A_j$$

has a total LCU coefficient sum bounded by $\pi^2 \eta D / (3mh^2)$, regardless of how large $a$ is chosen.

**Error control (Theorem 11):** The truncation error in approximating the true kinetic energy is:

$$|(T - \tilde{T})\psi(x)| \leq \frac{\pi^{3/2} e^{2a[1-\ln 2]}}{18m\sqrt{4a+3}} \eta D k_\text{max}^{2a+1} \left(\frac{k_\text{max}}{\pi}\right)^{\eta D/2} h^{2a-1}$$

Since $e^{2a(1-\ln 2)} \sim e^{-0.614 a}$, this decays exponentially in $a$ for fixed $h$ and bounded $k_\text{max}$. Setting $a \in O(\log \frac{\eta^2 D^2 t k_\text{max}}{m\varepsilon^2})$ suffices for any target error $\varepsilon$.

**In practice:** The key practical point is that increasing $a$ makes the approximation more accurate exponentially fast, without increasing the query complexity (the 1-norm is constant-bounded). The complexity is $O(h^{-2})$ regardless of $a$.

## When to reach for it

- Any real-space simulation where the kinetic energy operator must be decomposed into an LCU of unitaries for the [[Truncated Taylor Series Simulation]] method
- When the wave function has a maximum momentum $k_\text{max}$ (compact support in momentum space) — this is exactly the regime where the error bound applies
- For lattice models or first-principles simulations where position-basis registers are natural
- As an alternative to the quantum Fourier transform (QFT) approach to the kinetic energy (used by Kassal et al.); the QFT-based approach has different complexity characteristics and may struggle with the LCU framework

## Complexity

- **LCU 1-norm of kinetic term:** $\pi^2 \eta D / (3mh^2)$ — independent of order $a$
- **Number of distinct LCU terms:** $2\eta Da$ (one adder per particle, dimension, shift) — but implemented with a single adder query via [[Controlled Swap Network for Select Oracle]]
- **Required order** for error $\leq \varepsilon$ at time $t$: $a \in O\!\left(\log \frac{\eta^2 D^2 t k_\text{max}}{m\varepsilon^2}\right)$
- **Gate count for swap network:** $O(\eta D \log(L/h))$ gates per adder application

## Caveat

- The error bound assumes a momentum cutoff: $\hat{\psi}(k) = 0$ for $\|k\|_\infty > k_\text{max}$. This is violated near Coulomb singularities (nuclei), where the cusp condition forces large-$k$ components. Plane waves and free-particle wave functions satisfy it naturally.
- The diagonal identity term $d_{2a+1,j=0} \cdot \mathbf{1}$ is dropped from the simulation — it only contributes a global phase (or a classical energy offset). If absolute energies (not just time evolution) matter, add it back classically.
- The formula assumes periodic boundary conditions (coordinates evaluated modulo the grid length $L$). For non-periodic problems, enlarge the simulation box so the wave function has negligible support near the boundary.

## Related notes
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]
- [[Real-Space Grid Encoding for Many-Body Simulation]]
- [[Controlled Swap Network for Select Oracle]]
- [[Truncated Taylor Series Simulation]]
- [[Regularized Coulomb Potential (Delta Cutoff)]]
