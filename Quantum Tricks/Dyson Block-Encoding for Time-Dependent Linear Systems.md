
> **Source:** Berry and Costa, arXiv:2212.03544
> **Tags:** #trick #dyson-series #block-encoding #ODE #linear-systems

## What it does
Block-encodes the time-evolution propagator $V_m$ within a [[History-State Linear System Encoding for ODE Trajectories|history-state linear system]] by using a truncated Dyson series, rather than encoding each Taylor series term as a separate row in the block matrix.

## The trick

In [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]], the Taylor series for the propagator was encoded by adding extra rows to the block matrix — one row per Taylor term. This works for time-independent $A$, but breaks for time-dependent coefficients because the Taylor terms don't factor neatly.

The fix: keep a block-bidiagonal matrix where each row is one *time step* (not one series term), and handle the series inside the block-encoding of each diagonal block:

$$\mathcal{A} = I - S \cdot D, \quad D = \text{diag}(V_1, V_2, \ldots, V_r, I, \ldots, I)$$

where $V_m = W_K(m\Delta t, (m-1)\Delta t)$ is the truncated Dyson series propagator over the $m$-th segment. Each $V_m$ is block-encoded using the [[Sorting-Based Time-Ordering for Dyson LCU|sorting-based time-ordering]] machinery: $K$ time registers in superposition, sorted by a reversible network, feeding $K$ controlled applications of the block-encoding of $A(t)$.

The $\lambda$-value of the Dyson block-encoding is at most $e$ when $\lambda_A \Delta t \le 1$, keeping the overall $\lambda$-value for $\mathcal{A}$ at $O(1)$.

The same construction applies to the particular-solution vectors $v_m$, using $K-1$ calls to $U_A$ and one call to $U_b$.

## When to reach for it

- Solving time-dependent linear ODEs on a quantum computer via [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|QLSA]]
- Any problem where you need to block-encode a product of time-dependent operators as part of a larger linear system
- When the Taylor-series-as-extra-rows approach from Berry-Childs-Ostrander-Wang doesn't generalise to time-varying parameters

## Complexity

Per time step: $K$ calls to the block-encoding of $A(t)$, where $K = O(\log(\lambda_{Ax} T/\varepsilon) / \log\log(\cdots))$. Gate overhead: $O(K \log K \log M)$ for the sorting network.

## Caveat

Requires the [[Sorting-Based Time-Ordering for Dyson LCU|sorting network]] or [[Compressed Gap Encoding for Ordered Tuples|gap encoding]] infrastructure from Kieferová-Scherer-Berry / Low-Wiebe. Not simpler than the Taylor approach for time-independent problems — the Taylor encoding is more direct in that case.

## Related notes
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes]]
- [[Sorting-Based Time-Ordering for Dyson LCU]]
- [[History-State Linear System Encoding for ODE Trajectories]]
- [[Block-Encoding Composition Algebra]]
