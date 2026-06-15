# Log-Norm Shifted LCHS History States

> **Source:** Simon, Berry, and Somma, arXiv:2605.16195
> **Tags:** #trick #LCHS #non-unitary #history-state #log-norm

## What it does

Prepares history states for non-unitary evolution by shifting the generator to be dissipative and putting the removed growth/decay into the clock amplitudes.

## The trick

Suppose $Y$ has known log-norm upper bound $\xi_Y$. Then

$$
e^{sY}=e^{s\xi_Y}e^{s(Y-\xi_Y I)}.
$$

The shifted generator $Y-\xi_Y I$ has non-positive log-norm, so its propagator is suitable for [[LCHS Kernel for Non-Unitary Dynamics]]. Instead of asking LCHS to implement the full $e^{sY}$ directly, prepare a clock state weighted by $e^{s\xi_Y}$ and apply the controlled shifted propagator:

$$
\sum_m e^{tm\xi_Y/M}|m\rangle
\quad\xrightarrow{\mathrm{controlled}\ e^{tm(Y-\xi_Y I)/M}}\quad
\sum_m |m\rangle e^{tmY/M}.
$$

The resulting history state has the desired unshifted evolution, but the hard non-unitary part is handled by a contraction.

## When to reach for it

Use it when:

- a history-state algorithm needs $e^{sY}$ for non-Hermitian $Y$;
- you know a usable log-norm bound $\xi_Y$;
- after shifting, $Y-\xi_Y I$ is in the regime where LCHS applies;
- the clock-weight normalization remains controlled.

## Complexity

In the Simon--Berry--Somma matrix ODE algorithm, this gives history-state preparation using

$$
O(t\mu\log(1/\epsilon_{\mathrm{hist}}))
$$

queries to the relevant controlled block-encoding in the time-independent case, with history normalization bounded by

$$
O\!\left(\mu\int_0^t ds\,e^{2s\xi_Y}+\mu\frac{d}{c}e^{2t\xi_Y}\right).
$$

## Caveat

The shift only helps when the compensating clock weights do not blow up. If $\xi_Y>0$ and the true propagator grows exponentially, the normalization can still be bad. For bounded transient growth with positive log-norm, the paper's linear-systems route can be preferable.

## Related notes

- [[Efficient Quantum Algorithm for Linear Matrix Differential Equations and Applications to Open Quantum Systems (Simon-Berry-Somma 2026) — Paper Notes]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
- [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes]]
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes]]
- [[Two-Sided History-State Overlap for Matrix ODE Entries]]
