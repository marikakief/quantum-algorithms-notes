
> **Source:** Ding, Chen & Lin, arXiv:2308.15676
> **Tags:** #trick #Lindbladian #filter-function #ground-state-preparation #Gevrey

## What it does
Constructs a quantum jump operator $K$ that only allows transitions to lower energy eigenstates, guaranteeing a target eigenstate (e.g., the ground state) is a fixed point of the resulting Lindblad dynamics.

## The trick

Given a Hamiltonian $H = \sum_i \lambda_i |\psi_i\rangle\langle\psi_i|$ and a coupling operator $A$, define:

$$K = \int_{-\infty}^{\infty} f(s)\, e^{iHs} A\, e^{-iHs}\, ds = \sum_{i,j} \hat{f}(\lambda_i - \lambda_j)\, |\psi_i\rangle\langle\psi_i| A |\psi_j\rangle\langle\psi_j|$$

Choose the frequency-domain filter $\hat{f}(\omega)$ to satisfy:

$$\hat{f}(\omega) = 0 \quad \text{for all } \omega \geq 0$$

Then $\langle\psi_i|K|\psi_j\rangle = \hat{f}(\lambda_i - \lambda_j)\langle\psi_i|A|\psi_j\rangle = 0$ whenever $\lambda_i \geq \lambda_j$ — no upward transitions. In particular, $K|\psi_0\rangle = 0$, so the ground state is annihilated by $K$ and is therefore a fixed point of the Lindblad dynamics $\mathcal{L}_K[\rho] = K\rho K^\dagger - \frac{1}{2}\{K^\dagger K, \rho\}$.

The practical construction: $\hat{f}(\omega) = \hat{u}(\omega/S_\omega)\hat{v}(\omega/\Delta)$ where $\hat{u}$ is a Gevrey-class bump function with $\text{supp}(\hat{u}) \subset [-1,1]$ and $\hat{v}$ enforces the one-sided support $\text{supp}(\hat{v}) \subset (-\infty, 0]$. The Gevrey regularity ensures the time-domain function $f(s)$ decays super-exponentially, so the integral can be truncated to a finite support $[-S_s, S_s]$ with $S_s = \Theta(\Delta^{-1} \cdot (\log(\cdot))^\alpha)$.

A concrete choice that works numerically: $\hat{f}(\omega) = \frac{1}{2}[\text{erf}(\frac{\omega+a}{\delta_a}) - \text{erf}(\frac{\omega+b}{\delta_b})]$ with $a \sim \|H\|$, $b \sim \Delta$.

## When to reach for it
- Designing Lindbladian dynamics for ground state preparation or cooling
- Quantum MCMC algorithms where you need energy-monotone transitions
- Any setting where you want to implement a non-unitary operator that respects an energy ordering
- Thermal state preparation via quantum detailed balance — the same filter idea works at finite temperature by adjusting the ratio $\hat{f}(\omega)/\hat{f}(-\omega) = e^{-\beta\omega}$

## Complexity
- **Quadrature points:** $M_s = S_s/\tau_s$ where $S_s = \Theta(\Delta^{-1} \text{polylog})$ and $\tau_s = \Theta(1/\max\{\|H\|, S_\omega\})$
- **Per-quadrature-point cost:** one Hamiltonian evolution of duration $\tau_s$ plus one application of $A$
- **Total [[Hamiltonian simulation]] per step:** $O(S_s) = O(\Delta^{-1} \text{polylog})$

## Caveat
The filter function assumes knowledge of (or a lower bound on) the spectral gap $\Delta$ and the spectral bandwidth $S_\omega$. The filter's quality degrades as $\Delta \to 0$: $S_s$ diverges, making each step more expensive. Also, the construction guarantees the ground state is *a* fixed point, but not that it's the *unique* fixed point — ergodicity requires additional conditions on $A$ (see Theorem 7 in the source paper).

## Related notes
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
- [[Chebyshev Polynomial Spectral Projector]] — alternative approach to spectral filtering
- [[Hamiltonian-to-Projection via Phase Estimation]] — QPE-based spectral filtering
