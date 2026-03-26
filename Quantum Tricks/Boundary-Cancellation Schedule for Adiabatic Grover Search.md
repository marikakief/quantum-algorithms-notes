# Boundary-Cancellation Schedule for Adiabatic Grover Search

> **Source:** An, Costa, Berry, arXiv:2509.00171
> **Tags:** #trick #adiabatic #grover #search #boundary-cancellation #QAOA #scheduling

## What it does

Provides a scheduling function for adiabatic Grover search that simultaneously satisfies the boundary cancellation condition and slows down at the gap minimum. The Trotterised adiabatic evolution matches the [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] lower bound in query complexity and achieves super-polynomial precision scaling — using step size 1.

## The trick

Start with the "glue function":
$$g(s) = c_e^{-1} \int_0^s \exp\!\left(-\frac{1}{s'(1-s')}\right) ds', \quad c_e = \int_0^1 \exp\!\left(-\frac{1}{s'(1-s')}\right) ds'$$

This satisfies $g(0) = 0$, $g(1) = 1$, and $g^{(k)}(0) = g^{(k)}(1) = 0$ for all $k \geq 1$.

Compose two copies symmetrically:
$$f(s) = \begin{cases} \frac{1}{2}g(2s) & s \in [0, 1/2] \\ \frac{1}{2} + \frac{1}{2}g(2s-1) & s \in (1/2, 1] \end{cases}$$

This function is smooth, satisfies the boundary cancellation condition ($f^{(k)}(0) = f^{(k)}(1) = 0$), and flattens near $s = 1/2$ where $f(s) \approx 1/2$ — exactly where the gap minimum occurs for the Grover Hamiltonian. The flattening effectively slows the evolution through the gap bottleneck.

Apply first-order Trotter with step size $h = 1$:
$$|\tilde{\phi}\rangle = \prod_{j=0}^{T-1} e^{-if(j/T) H_1}\, e^{-i(1-f(j/T)) H_0}\, |u\rangle$$

**Key properties:**
- $f(s)$ is independent of both $N$ and $M$ — no a priori knowledge of marked states required
- For constant error: $T = \tilde{O}(\sqrt{N/M})$ — matches Grover up to log factors
- For fixed $N, M$: $T = O(1/\varepsilon^{o(1)})$ — super-polynomial precision (conditional on [[Discrete Adiabatic Theorem for Quantum Walks|high-order discrete adiabatic theorem]])
- Increasing $T$ always improves accuracy (no "overcooking" as in standard Grover)
- Produces [[AQC-to-QAOA Parameter Transfer|QAOA angles]] $\beta_j = 1-f(j/T)$, $\gamma_j = f(j/T)$ that match optimal QAOA for single-marked-state search at $O(\sqrt{N})$

## When to reach for it

- Unstructured search where the number of marked states $M$ is unknown.
- Situations where the fixed-point property (accuracy improves monotonically with $T$) matters — avoids the "overcooking" failure mode of amplitude amplification.
- When you want a constructive set of near-optimal [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|QAOA]] angles for the Grover problem without variational optimisation.
- Adiabatic algorithms where both the $N$-scaling and the $\varepsilon$-scaling need to be good.

## Complexity

$T = \tilde{O}(\sqrt{N/M} \cdot 1/\varepsilon^{o(1)})$ total Trotter steps. Each step applies $e^{-if(j/T)H_1}$ and $e^{-i(1-f(j/T))H_0}$. For the standard Grover Hamiltonians ($H_0 = I - |u\rangle\langle u|$, $H_1 = I - \sum_{j \in M} |j\rangle\langle j|$), each exponential is efficient.

The super-polynomial precision improvement $1/\varepsilon^{o(1)}$ compared to the $1/\varepsilon$ of [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf]] or [[Gap-Adapted Adiabatic Scheduling|gap-adapted scheduling]] with $1 < p < 2$ is because boundary cancellation suppresses diabatic errors exponentially in $T$.

## Caveat

- The super-polynomial precision scaling depends on Conjecture 10 (high-order discrete adiabatic theorem), which is numerically supported but unproven.
- The constant $C_k(N, M)$ in the $O(C_k/T^k)$ error bound may depend on $N$ and $M$ — the theorem doesn't track this dependence explicitly. The $\tilde{O}(\sqrt{N/M})$ complexity for constant error comes from a separate (unconditional) first bound.
- The schedule requires evaluating $\exp(-1/(s(1-s)))$, which is numerically well-behaved but needs care near $s = 0$ and $s = 1$.
- For the comparison with QAOA: the paper shows discrete AQC *matches* QAOA for Grover. It doesn't prove QAOA can't do better by a constant factor.

## Related notes
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes]]
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes]]
- [[Gap-Adapted Adiabatic Scheduling]]
- [[AQC-to-QAOA Parameter Transfer]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes]]
- [[Discrete Adiabatic Theorem for Quantum Walks]]
- [[Superadiabatic Smooth Scheduling for Exponential Precision]]
