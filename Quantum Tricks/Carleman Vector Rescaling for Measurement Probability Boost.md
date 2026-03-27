
> **Source:** Costa, Schleich, Morales, Berry, arXiv:2312.09518 (Definition 1, Lemma 2)
> **Tags:** #trick #Carleman-linearisation #rescaling #amplitude-amplification #measurement-probability

## What it does
Eliminates an exponential-in-$N$ penalty on the measurement probability when extracting the solution component from a Carleman-linearised nonlinear ODE.

## The trick

The Carleman vector is $\mathbf{y} = [\mathbf{u}, \mathbf{u}^{\otimes 2}, \ldots, \mathbf{u}^{\otimes N}]^T$. Its norm is $\|\mathbf{y}\|^2 = \sum_{\ell=1}^{N}\|\mathbf{u}\|^{2\ell}$. When $\|\mathbf{u}\| > 1$, the highest-order component dominates and the probability of measuring $y_1 = \mathbf{u}$ is $O(\|\mathbf{u}\|^{-2N})$ — exponentially suppressed.

**Fix:** Substitute $\tilde{\mathbf{u}} = \mathbf{u}/\gamma$ with $\gamma \geq \|\mathbf{u}_{\mathrm{in}}\|$. For dissipative dynamics ($\|\mathbf{u}(t)\| \leq \|\mathbf{u}_{\mathrm{in}}\|$), this gives $\|\tilde{\mathbf{u}}(t)\| \leq 1$ for all $t$, so $\|\tilde{\mathbf{y}}\|^2 \leq N$ and

$$P(\tilde{y}_1) = \frac{1 - \|\tilde{\mathbf{u}}\|^2}{1 - \|\tilde{\mathbf{u}}\|^{2N}} \geq \frac{1}{N}.$$

The amplitude amplification cost drops from $O(\|\mathbf{u}_{\mathrm{in}}\|^N)$ to $O(\sqrt{N})$.

The rescaling modifies the Carleman matrix: off-diagonal blocks pick up a factor $\gamma^{M-1}$, so $\tilde{A}_{j+M-1}^{(M)} = \gamma^{M-1}A_{j+M-1}^{(M)}$. For the ODE solver to remain stable, we need $\gamma^{M-1} \leq |\lambda_0|/\|F_M\|$, which is equivalent to $\gamma \leq \|\mathbf{u}_{\mathrm{in}}\|/R^{1/(M-1)}$ where $R$ is the dissipativity ratio. The optimal choice $\gamma^{M-1} = |\lambda_0|/\|F_M\|$ gives a combined amplitude amplification cost of

$$\frac{1}{\sqrt{1 - R^{2/(M-1)}}} \cdot \frac{\|\mathbf{u}_{\mathrm{in}}\|}{\|\mathbf{u}(T)\|}.$$

## When to reach for it

- Any quantum algorithm using [[Carleman Linearisation for Quantum Nonlinear ODE Solvers|Carleman linearisation]] — rescaling is essentially mandatory
- Especially for PDE applications where $\|\mathbf{u}_{\mathrm{in}}\|_2 \propto \sqrt{n}$ (number of grid points), since without rescaling the cost is $n^N$, killing quantum speedup entirely

## Complexity

The rescaling itself is free (just a redefinition of variables). The cost is implicit: it changes the block-encoding normalisation $\lambda_{\tilde{A}_N}$, but under the stability condition this is at most a constant factor worse than without rescaling.

## Caveat

- Requires $R < 1$. If $R \geq 1$, the stability condition $\gamma^{M-1} \leq |\lambda_0|/\|F_M\|$ cannot be satisfied with $\gamma \geq \|\mathbf{u}_{\mathrm{in}}\|$.
- For PDEs, the rescaling is by $\|\mathbf{u}\|_2$ but PDE stability is in $\|\mathbf{u}\|_{\max}$. This [[Norm Tension Between PDE Stability and Quantum Rescaling|norm mismatch]] can make the rescaled system unstable even when the original PDE is stable.

## Related notes
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes]]
- [[Carleman Linearisation for Quantum Nonlinear ODE Solvers]]
- [[Norm Tension Between PDE Stability and Quantum Rescaling]]
- [[Postselection-Cost Parameterization via g-Factor]] — analogous idea for linear ODEs: the g-factor $\|\mathbf{u}\|_{\max}/\|\mathbf{u}(T)\|$ plays a similar role to the amplitude amplification cost here
