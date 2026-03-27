
> **Source:** O'Brien, Streif, Rubin, Santagati, Su, Huggins et al., arXiv:2111.12437
> **Tags:** #trick #gradient-estimation #finite-difference #phase-estimation #fault-tolerant

## What it does

Jointly optimises the step size $dR$ and order $m$ of a higher-order central finite difference scheme when the function values come from quantum phase estimation, balancing discretisation error (shrinks with larger $dR$ and higher $m$) against QPE error (grows with smaller $dR$).

## The trick

Estimate $dE/dR_i$ via degree-$2m$ central finite differences:

$$\frac{dE}{dR_i} = \frac{1}{dR}\sum_{l=-m}^{m} a_l^{(m)} E(R + l\,dR\cdot v_i) + \varepsilon_{\text{fd}}^{(m)}$$

Two competing error sources:
- **Discretisation error:** $\varepsilon_{\text{fd}}^{(m)} \leq \frac{e}{dR}\frac{(8\,dR\,c\,m)^{2m+1}}{1 - 8\,dR\,c\,m}$, where $|d^k E / dR_i^k| \leq ec^k k^{k/2}$
- **Phase estimation error:** $\varepsilon_{\text{PE}} \leq \frac{\pi \lambda_H 6^{3/2} m^{1/2}}{2\,dR\cdot T}$

Discretisation error drops *exponentially* with $m$, so higher-order formulas allow *larger* $dR$, which *reduces* the QPE cost (since $T \propto 1/dR$). Optimal $m$ satisfies a Lambert-W equation:

$$m \in \tilde{O}\left(\frac{\log(ec/\varepsilon)}{\log\left(\frac{9}{2}(1 + N_a \max\|dH/dR_i\| / (\gamma c))\right)}\right)$$

The resulting total query complexity for all $3N_a$ gradient components (2-norm error $\varepsilon$) is:

$$T_{\text{FD}} \in \tilde{O}\left(\frac{N_a^{3/2} \lambda_H c \log^{5/2}(e)}{\varepsilon}\right)$$

An additional optimisation: when the gap $\gamma$ is large enough relative to the perturbation ($m\,dR \leq \gamma / (36 m^2 N_a \max\|dH/dR_i\|)$), the ground state can be **reused** across all $2m$ displaced configurations, making state preparation cost additive rather than multiplicative.

## When to reach for it

- Estimating gradients of eigenvalues on a quantum computer, where the quantum device is used as a black-box energy subroutine.
- When state preparation is cheap (good initial overlap, large gap) — the finite difference method avoids the $\lambda_H / \gamma$ reflection overhead that makes direct expectation value methods expensive.
- When the higher-order derivatives of the energy are well-behaved (bounded $c$ and $e$). This tends to hold for molecular systems with a clear energy minimum.

## Complexity

$\tilde{O}(N_a^{3/2} \lambda_H / \varepsilon)$ queries, with system-dependent constants $c$ and $e$. Per-query circuit cost is one [[Qubitization (Quantum Walk for Spectral Encoding)|qubitized]] phase estimation, so total Toffoli count is $\tilde{O}(N_a^{3/2} \lambda_H T_H / \varepsilon)$.

## Caveat

The constants $c$ and $e$ bounding higher-order derivatives $|d^k E/dR_i^k| \leq ec^k k^{k/2}$ are system-dependent and hard to estimate a priori. In plane-wave bases, $c$ might be $o(1)$ (due to oscillatory phases) or $O(N^{1/3})$ depending on the system. For molecular orbital bases, no general bounds are known. If $c$ is large, the advantage over direct expectation value methods vanishes.

The ground-state reuse condition also introduces a gap-dependent constraint on $dR$ that can conflict with the error-optimal $dR$ for small-gap systems, forcing a fallback to independent state preparation at each displaced configuration.

## Related notes
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]]
- [[Non-Uniform Gradient Estimation via Variable-Precision QFT]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Iterative Phase Estimation (Kitaev)]]
