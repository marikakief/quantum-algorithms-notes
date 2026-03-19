# Monte Carlo Average-Hamiltonian Product Formula

> **Source:** Poulin, Qarry, Somma & Verstraete, arXiv:1102.1360
> **Tags:** #trick #hamiltonian-simulation #randomization #product-formulas #time-dependent

## What it does

Converts a time-dependent Hamiltonian simulation (with arbitrarily fast-varying coefficients) into a standard product formula using Monte Carlo sampling, eliminating the need for time-ordered exponentials or smoothness assumptions.

## The trick

Start from the [[Generalised Trotter for Time-Dependent Hamiltonians|generalised Trotter]] decomposition, which produces time-ordered exponentials $\mathcal{T}\exp(-i\int_{t_j}^{t_j+\Delta t} H_X(s)\,ds)$ for each term. These aren't standard gates. To get a usable product formula:

**Step 1 — Drop the time-ordering.** For a single bounded term $H_X$:

$$\left\|\mathcal{T}\exp\!\left(-i\int_{t_j}^{t_j+\Delta t} H_X(s)\,ds\right) - \exp\!\left(-i\int_{t_j}^{t_j+\Delta t} H_X(s)\,ds\right)\right\| \le 2\|H_X\|^2 \Delta t$$

This is **linear** in $\Delta t$ (Eq. (4) / Appendix A of the paper). It arises from $[H_X(t_1), H_X(t_2)]$ at different times within the interval. Important: if $H_X(t) = f_X(t) H_X$ (scalar coefficient times a fixed operator), the commutator vanishes and time-ordering is exact — zero error. The bound is only non-trivial when the operator *structure* changes in time.

**Step 2 — Monte Carlo the integral.** Sample $m$ random times $\tau_j^1, \ldots, \tau_j^m \in [t_j, t_j+\Delta t]$ uniformly. Approximate:

$$\frac{1}{\Delta t}\int_{t_j}^{t_j+\Delta t} H_X(s)\,ds \approx \frac{1}{m}\sum_{k=1}^m H_X(\tau_j^k)$$

Error: $O(\|H_X\|/\sqrt{m})$ by standard concentration.

**Step 3 — Trotterise the samples.** Apply:

$$\prod_{k=1}^m \exp\!\left(-i\frac{\Delta t}{m} H_X(\tau_j^k)\right)$$

in increasing order of $\tau_j^k$. This is a genuine product formula — each factor is an exponential of the Hamiltonian at a specific sampled time point, and the whole thing compiles to standard gates.

## When to reach for it

- You need a product formula for time-dependent $H(t)$ but the time-dependence is non-smooth or rapidly oscillating, so standard "freeze the Hamiltonian" Trotter breaks down.
- The simulation result only needs to hold in expectation (averaged over the random sampling), not for every individual circuit.
- Conceptual arguments where you need to show *existence* of a polynomial-size circuit for arbitrary time-dependent evolution, without caring about tight constants.

## Complexity

Within each time interval $[t_j, t_j + \Delta t]$, you apply $m \cdot L$ exponentials ($L$ Hamiltonian terms, $m$ Monte Carlo samples each). Total gates across all $n = T/\Delta t$ intervals:

$$n \cdot m \cdot L \cdot (\text{gates per exponential})$$

The Monte Carlo sample count $m$ must be large enough that the total sampling error across all intervals stays within the error budget. Since the per-interval Monte Carlo error is $O(\|H_X\| \Delta t / \sqrt{m})$ and there are $T/\Delta t$ intervals, this adds a polynomial overhead on top of the deterministic generalised-Trotter gate count.

## Caveat

- Convergence is in expectation / average-case. Individual circuit instances have random errors.
- The Monte Carlo overhead is non-trivial. For practical simulation, [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|qDRIFT]] gives a much cleaner randomised product formula with explicit $\ell_1$-norm scaling.
- Requires black-box access to $H_X(t)$ at arbitrary times $t$. If evaluating $H_X(\tau)$ is classically expensive, the sampling step adds hidden cost.
- Superseded for practical purposes by later randomised methods, but the conceptual idea — Monte Carlo to eliminate smoothness requirements — is clean and reusable.

## Related notes
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes]]
- [[Generalised Trotter for Time-Dependent Hamiltonians]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]
- [[Permutation Averaging for Product-Formula Error Cancellation]]
