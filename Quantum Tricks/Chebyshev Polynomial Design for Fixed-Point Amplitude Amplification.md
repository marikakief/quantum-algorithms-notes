# Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification

> **Source:** Yoder, Low, Chuang, arXiv:1409.3305
> **Tags:** #trick #amplitude-amplification #fixed-point #Chebyshev #polynomial-design #QSP-precursor

## What it does

Amplifies the target component of a quantum state to success probability $\geq 1 - \delta^2$ without knowing the exact overlap $\lambda$, using $O(\log(1/\delta)/\sqrt{\lambda})$ oracle queries — the same scaling as standard [[Standard Amplitude Amplification|Grover/amplitude amplification]] but without oscillation (the "soufflé problem").

## The trick

Replace the standard Grover iterate $G(\pi, \pi)$ with a sequence of **phase-modified iterates** $G(\alpha_j, \beta_j)$, where

$$G(\alpha, \beta) = -S_s(\alpha) S_t(\beta)$$

and $S_s(\alpha)$ reflects about $|s\rangle$ with phase $e^{-i\alpha}$, $S_t(\beta)$ reflects about $|t\rangle$ with phase $e^{i\beta}$.

Choose the phases as:

$$\alpha_j = -\beta_{l-j+1} = 2\cot^{-1}\!\left(\tan\!\left(\frac{2\pi j}{L}\right)\sqrt{1-\gamma^2}\right)$$

where $L = 2l+1$ and $\gamma^{-1} = T_{1/L}(1/\delta)$. The resulting success probability is a **Dolph-Chebyshev** function:

$$P_L(\lambda) = 1 - \delta^2 \left[T_L\!\left(\frac{\sqrt{1-\lambda}}{T_{1/L}(1/\delta)^{-1}}\right)\right]^2$$

This is the polynomial of degree $L$ that guarantees $P_L \geq 1-\delta^2$ over the widest possible range of $\lambda$ — an optimal design from antenna array theory.

**Key property:** $P_L \geq 1-\delta^2$ for all $\lambda \geq w \approx (\log(2/\delta)/L)^2$. As $L$ increases, $w$ shrinks, and the algorithm can never "overcook."

## When to reach for it

- Any time [[Standard Amplitude Amplification|amplitude amplification]] is used but the overlap $\lambda$ is unknown or varies across a superposition of instances.
- As a subroutine inside [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|eigenvalue filtering]] or [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] where the initial overlap with the target subspace isn't precisely known.
- Quantum rejection sampling, minimum finding, collision problems — anywhere Grover's algorithm is used as a black box.
- When the algorithm must be fully coherent (no intermediate measurements) and cannot restart from scratch.

## Complexity

- **Queries:** $O(\log(1/\delta)/\sqrt{\lambda_0})$ where $\lambda_0$ is the minimum guaranteed overlap
- **Phase computation:** closed-form expressions, $O(L)$ classical work
- **Circuit:** one ancilla qubit (for the oracle), $L-1$ controlled reflections

## Caveat

- Still requires a lower bound $\lambda_0 > 0$ on the overlap. If $\lambda$ can be exponentially small, the query count is correspondingly large.
- The polynomial oscillates for $\lambda < w$ (below the guaranteed range) — it's not monotone everywhere, just bounded above $1-\delta^2$ in the target range.
- For $\delta = 0$ (perfect fixed-point), the query complexity degrades to $O(1/\lambda)$ — the $\pi/3$-algorithm scaling. The quadratic speedup requires accepting $\delta > 0$.

## Related notes
- [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]]
- [[One-Ancilla Alternating Phase Sequence]]
- [[Chebyshev Nesting (Semigroup Concatenation)]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — QSP generalises this polynomial design to arbitrary bounded functions of eigenvalues
