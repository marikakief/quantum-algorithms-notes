
> **Source:** Rubin, Berry, Kononov, Malone, Khattar, White, Lee, Neven, Babbush, Baczewski, arXiv:2308.12352
> **Tags:** #trick #observable-estimation #mean-estimation #sampling #Heisenberg-scaling #resource-estimation

## What it does

Determines when standard Monte Carlo sampling of an observable beats Heisenberg-scaling mean estimation (Kothari-O'Donnell algorithm) by comparing constant-factor Toffoli costs, not just asymptotic scaling.

## The trick

For estimating $\langle O \rangle$ to precision $\varepsilon$ on a state prepared by a circuit of cost $C_{\text{prep}}$:

- **Monte Carlo sampling:** $N_s = O(\sigma^2/\varepsilon^2)$ repetitions, each costing $C_{\text{prep}}$. Total: $O(\sigma^2 C_{\text{prep}}/\varepsilon^2)$.
- **Kothari-O'Donnell (KO) algorithm:** $O(\sigma^2/\varepsilon)$ calls to a Grover-like iterate, each costing $2C_{\text{prep}} + C_{\text{phase}}$ where $C_{\text{phase}}$ is the cost of constructing a phase oracle from the observable. The iterate also requires QPE-like overhead. Total: $O(\sigma^2(2C_{\text{prep}} + C_{\text{phase}})/\varepsilon)$.

The crossover precision:

$$\varepsilon_{\text{cross}} \approx \frac{C_{\text{prep}}}{2C_{\text{prep}} + C_{\text{phase}}}$$

Below $\varepsilon_{\text{cross}}$, KO wins. Above it, plain sampling is cheaper.

For the stopping power problem: $C_{\text{prep}}$ is the QSP time-evolution cost ($\sim 10^{10}$–$10^{13}$ Toffolis per time point), $C_{\text{phase}}$ involves encoding the projectile kinetic energy into a phase register (sum of squares, products, arctan — a few hundred to a few thousand Toffolis). The required precision is $\varepsilon \sim 0.1$ Ha. The crossover occurs at $\varepsilon \sim 0.01$ Ha — just below the stopping-power requirement. So Monte Carlo wins.

The analysis was done using Cirq-FT resource estimation for the KO algorithm's full circuit (synthesizer, reflection, phase oracle), giving the first concrete constant-factor comparison between the two approaches.

## When to reach for it

- Any quantum dynamics or eigenvalue estimation problem where you need to choose between sampling and coherent estimation
- Resource estimation for fault-tolerant algorithms where the constant factor in the preparation circuit matters
- When the target precision is "moderate" (not trying to resolve exponentially small differences) — standard sampling is likely better
- When the observable has small variance relative to its operator norm — sampling is particularly competitive

## Complexity

Monte Carlo: $O(\sigma^2 C_{\text{prep}}/\varepsilon^2)$ Toffolis total.

KO: $O(\sigma^2 (2C_{\text{prep}} + C_{\text{phase}})/\varepsilon)$ Toffolis total, plus classical reductions (a small integer multiple of the core decision problem).

The KO overhead factor $(2C_{\text{prep}} + C_{\text{phase}})/C_{\text{prep}} \approx 2$–$3$ means the quadratic advantage in $1/\varepsilon$ is offset by a $2$–$3\times$ per-query penalty plus the classical reduction overhead.

## Caveat

The crossover analysis is specific to observables where the phase oracle is cheap relative to state preparation. For observables with expensive phase oracles (e.g., many-body correlation functions), the crossover shifts to even lower $\varepsilon$, making sampling even more favourable. Conversely, for observables that are diagonal in the computational basis (like kinetic energy in momentum representation), the phase oracle can be very cheap, which could shift the balance.

The KO algorithm also requires a rough prior estimate of $\langle O \rangle$ to initialize the classical reductions. For a nearly-classical observable (like the kinetic energy of a massive projectile), this prior is easy to obtain. For more quantum observables, the initialization cost could add overhead.

## Related notes
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]]
- [[Kaiser Window Amplitude Estimation]]
- [[Pauli Expectation Value Estimation]]
- [[Gaussian Wave Packet Variance Tuning for Observable Estimation]]
