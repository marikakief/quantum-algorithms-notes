
> **Source:** Berry and Costa, arXiv:2212.03544
> **Tags:** #trick #dyson-series #ODE #time-discretization #oracle-complexity

## What it does
Separates the oracle call count from the derivative dependence in quantum ODE solvers: the number of queries to block-encodings of $A(t)$ and $b(t)$ is *completely independent* of derivatives of the coefficients. Derivatives affect only the gate count (through the time-register discretization).

## The trick

In Dyson-series-based ODE algorithms, the time integrals in the Dyson expansion are discretized into $M$ points per segment. The discretization error for the propagator $W_K$ is:

$$\|W_K - \tilde{W}_K\| = O\!\left(\frac{(\Delta t)^2}{M} \max_t \|A'(t)\|\right)$$

To make this $O(\varepsilon/r)$, choose:

$$M = O\!\left(\frac{TD}{\lambda_A \varepsilon}\right)$$

where $D$ captures first derivatives of $A(t)$ and $b(t)$. But $M$ only enters as $\log M$ in the sorting-network gate count — it's the number of bits in the time registers. The number of applications of the block-encoding oracle $U_A$ per Dyson term is $K$ (the truncation order), which depends only on $\log(\lambda_{Ax} T/\varepsilon)$.

So: oracle complexity depends on $T$, $\lambda_A$, $\varepsilon$ — not on how fast $A(t)$ varies. The gate complexity picks up an additive $\log(TD/(\lambda_A \varepsilon))$ factor.

Compare with the spectral method of Childs-Liu (2020), where the complexity scales as $\log^4$ of arbitrary-order derivatives through their variable $g'$. The Dyson approach completely eliminates this coupling.

## When to reach for it

- Designing quantum algorithms for ODEs with rapidly varying but bounded coefficients
- Situations where $\|A'(t)\|$ is large but $\|A(t)\|$ is moderate
- When you want oracle-count guarantees that don't degrade with coefficient regularity

## Complexity

Oracle calls: independent of $D$. Gate count: additive $\log(TD/(\lambda_A \varepsilon))$ factor per QLSA step.

## Caveat

The derivative dependence doesn't disappear — it moves to the gate count. For extremely rapidly varying coefficients where $D \gg \lambda_A$, the gate count can dominate. But the oracle calls, which typically correspond to expensive block-encoding queries, remain derivative-free.

Also, discontinuous $A(t)$ is not handled. The analysis requires $A'(t)$ to exist and be bounded. For discontinuous dynamics, the time-marching approach of [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] or a total-variation bound as in the same paper may be more appropriate.

## Related notes
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes]]
