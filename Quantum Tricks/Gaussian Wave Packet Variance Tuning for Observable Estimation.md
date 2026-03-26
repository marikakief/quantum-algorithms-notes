# Gaussian Wave Packet Variance Tuning for Observable Estimation

> **Source:** Rubin, Berry, Kononov, Malone, Khattar, White, Lee, Neven, Babbush, Baczewski, arXiv:2308.12352
> **Tags:** #trick #state-preparation #observable-estimation #dynamics #non-Born-Oppenheimer #sampling

## What it does

Controls the tradeoff between physical accuracy and measurement efficiency when replacing a classical point particle with a quantum wave packet in a non-Born-Oppenheimer simulation.

## The trick

When promoting a classical nucleus to a quantum degree of freedom (e.g., to avoid time-dependent Hamiltonian simulation), initialize it as a Gaussian wave packet in momentum space:

$$\psi_{\text{proj}}(k, t=0) \propto e^{ik \cdot R_{\text{proj}}} e^{-\|k - k_{\text{proj}}\|^2 / 4\sigma_k^2}$$

The momentum-space width $\sigma_k$ (equivalently, real-space width $\sigma_r = 1/\sigma_k$) controls two competing effects:

1. **Sampling cost** scales as $\sigma_k^2/(2M_{\text{proj}}\varepsilon^2)$ for Monte Carlo estimation of kinetic energy to precision $\varepsilon$. Larger $\sigma_k$ → more samples needed.

2. **Physical bias** from the smeared charge distribution: the effective potential is $\zeta_{\text{proj}} \operatorname{erf}(\|r - R_{\text{proj}}\|/\sqrt{2}\sigma_r) / \|r - R_{\text{proj}}\|$, which deviates from the point-charge $\zeta_{\text{proj}}/\|r - R_{\text{proj}}\|$ for $\|r\| \lesssim \sigma_r$. Smaller $\sigma_k$ (broader real space) → more bias.

3. **Grid requirements:** the plane-wave cutoff needed to resolve the wave packet scales as $\sigma_k^2$ (roughly), requiring $n_n \approx n_p + 3$ qubits per spatial direction at $\sigma_k = 6$–10 a.u.

The calibration: use classical TDDFT convergence tests at different plane-wave cutoffs (equivalently, different effective $\sigma_r$) to determine the $\sigma_k$ that introduces bias below the target stopping-power precision. For low-$Z$ projectiles at ICF densities: $\sigma_k \approx 5$–10 a.u.

## When to reach for it

- Any quantum dynamics simulation where a heavy particle is promoted from classical to quantum to simplify the Hamiltonian (e.g., avoid time-dependent simulation)
- Quantum simulations where the observable of interest is sampled from a subsystem register (projectile, impurity, etc.)
- Designing initial states for scattering or transport simulations

## Complexity

The wave packet preparation itself is cheap: $O(\log N_n)$ cost from the method of Bagherimehrab et al. (2022), negligible compared to time evolution.

The real cost: $\sigma_k$ determines $n_n$ and thus the qubit count for the projectile register ($3n_n$ qubits) and the block-encoding overhead for controlled swaps ($6n_n$ Toffolis per SELECT call).

## Caveat

The calibration against TDDFT is model-dependent — it assumes TDDFT convergence behaviour is a good proxy for the exact dynamics' sensitivity to charge smearing. For very heavy or highly charged projectiles, the optimal $\sigma_k$ may differ significantly. The approach also assumes the wave packet doesn't disperse appreciably during the simulation (valid when $M_{\text{proj}} \gg m_e$ and the simulation time is short).

## Related notes
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes]]
- [[First-Quantized Plane-Wave Chemistry Encoding]]
- [[Non-BO Projectile Block Encoding Extension]]
- [[Shot-Noise vs Heisenberg-Scaling Observable Estimation Crossover]]
