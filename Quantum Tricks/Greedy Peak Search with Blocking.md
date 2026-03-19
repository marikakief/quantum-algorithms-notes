# Greedy Peak Search with Blocking

> **Source:** Ding, Li, Lin, Ni, Ying, Zhang, arXiv:2402.01013
> **Tags:** #trick #spectral-estimation #peak-finding #multiple-eigenvalues #classical-postprocessing

## What it does

Extracts multiple eigenvalue estimates from a filtered spectral density by iteratively finding the highest peak and blocking a neighbourhood around it before searching for the next.

## The trick

Given a filtered density function $G(\theta)$ evaluated on a discrete grid (with peaks near each dominant eigenvalue):

1. Find $\theta_1 = \arg\max_\theta G(\theta)$
2. Block the interval $(\theta_1 - \alpha/T, \; \theta_1 + \alpha/T)$ — exclude it from future searches
3. Find $\theta_2 = \arg\max_{\theta \notin \text{blocked}} G(\theta)$
4. Block around $\theta_2$, repeat $K$ times

Output $\{\theta_k\}_{k=1}^K$ as estimated eigenvalue positions.

The blocking radius $\alpha/T$ is set wide enough that the tails of an already-detected peak don't create a false second detection, but narrow enough that genuinely distinct eigenvalues separated by more than $2\alpha/T$ are found individually.

This avoids the linear-algebraic overhead of ESPRIT or matrix-pencil methods. The trade-off: it gives peak locations on the grid rather than super-resolved estimates. For the applications where QMEGS targets (Heisenberg-limited eigenvalue estimation), this is sufficient — the grid resolution is already $q/T \ll \epsilon$.

## When to reach for it

- When you have a smooth spectral density with well-separated bumps and need a fast, robust peak finder.
- Multiple eigenvalue estimation problems where you don't need sub-grid resolution.
- As a preprocessing step before a refinement algorithm (e.g., find approximate locations via blocking, then refine each with targeted Hadamard tests around that frequency).

## Complexity

$O(KJ)$ classical operations where $J$ is the grid size and $K$ is the number of eigenvalues sought. Negligible compared to the quantum data-generation cost.

## Caveat

When eigenvalues are closer than the blocking radius $2\alpha/T$, they merge into a single detected peak — the algorithm guarantees a *covering* (each true eigenvalue is near some detected peak) but not a bijection. To resolve close eigenvalues individually, you need $T \gg \alpha / \Delta_{\text{dom}}$, which increases circuit depth. Also, the number of dominant eigenvalues $K$ must be guessed in advance (overestimating is safe — extra peaks will just be noise).

## Related notes
- [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes]]
- [[Gaussian Time-Sampling Filter for Spectral Estimation]]
- [[Dominant-Tail Separation Condition]]
