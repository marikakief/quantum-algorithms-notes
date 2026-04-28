# Gap as Passive Noise Filter in Adiabatic Computation

> **Tags:** #trick #adiabatic #decoherence #noise #open-quantum-systems #spectral-gap
> **Source:** Childs, Farhi, Preskill, arXiv:quant-ph/0108048, PRA **65**, 012322 (2002)

## What it does

Shows that the spectral gap $\gamma$ of an adiabatic Hamiltonian simultaneously (a) suppresses bath-induced transitions out of the ground state, and (b) reduces the required adiabatic run time. A large gap is therefore doubly protective against environmental noise — it filters out decoherence without any active error correction.

## The trick

Under a Markovian (Lindblad) bath with coupling $g$, the transition rate from the ground state $|0\rangle$ to an excited state $|k\rangle$ is proportional to:

$$\Gamma_{0 \to k} \propto g^2 |\langle k|A|0\rangle|^2 \, J(\gamma_{k0})$$

where $J(\omega)$ is the bath spectral density evaluated at the gap frequency $\gamma_{k0} = E_k - E_0$.

The probability of finding the system in an excited state after running for time $T$ accumulates as:

$$p_{\mathrm{exc}} \lesssim \frac{g^2 \, J(\gamma)}{\gamma^2} \cdot T$$

Meanwhile, the adiabatic condition itself requires $T \gtrsim \|\dot{H}\| / \gamma^2$. Substituting the minimum admissible $T$:

$$p_{\mathrm{exc}} \lesssim \frac{g^2 J(\gamma) \|\dot{H}\|}{\gamma^4}$$

The gap appears to the **fourth power** in the denominator of the total excitation probability when run at optimal speed. Algorithms with a polynomially large gap can be robust to weak noise without encoding or active correction.

**Optimal run time:** Balancing the adiabatic error (decreasing in $T$) against the bath error (increasing in $T$) gives an optimal total time:

$$T_{\mathrm{opt}} \sim \sqrt{\frac{\|\dot{H}\|}{g^2 J(\gamma)}}$$

at which the total infidelity is minimised.

**Bath-assisted cooling:** At low temperature ($\beta \gamma \gg 1$), downward transitions dominate. The bath actively drives the system toward the instantaneous ground state. The "noise" becomes a resource.

## When to reach for it

- Estimating whether an adiabatic algorithm with gap $\gamma$ will survive weak environmental coupling $g$.
- Justifying passive robustness of gapped Hamiltonian algorithms (topological codes, frustrated-free ground state preparation) without invoking active error correction.
- Designing the run time $T$ when bath coupling is known: use $T_{\mathrm{opt}}$ to balance unitary adiabatic error and bath-induced excitation.
- Arguing that bath-assisted relaxation (Davies generators, engineered Lindbladians) is beneficial when $\beta \gamma \gg 1$.

## Complexity

For weak coupling $g \ll \gamma$, the total infidelity at $T_{\mathrm{opt}}$ scales as:

$$\text{infidelity} \sim \sqrt{\frac{g^2 J(\gamma) \|\dot{H}\|}{\gamma^4}}$$

This is $\ll 1$ whenever $g^2 J(\gamma) \|\dot{H}\| \ll \gamma^4$. No overhead in circuit depth or qubit count; the gap provides the protection for free.

## Caveat

- **Perturbative:** Valid only for small coupling $g$. No threshold theorem — for large $g$ the analysis breaks down entirely.
- **Markovian bath assumed.** Non-Markovian environments (long correlation times, structured baths) can qualitatively change the error scaling.
- **Gap must stay open throughout.** Algorithms with exponentially small gaps (e.g., NP-hard optimisation instances near the satisfiability threshold) get no protection from this mechanism — see [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes]].
- **No encoding.** Unlike fault-tolerant computation, there is no redundancy. A single unlucky bath event that causes a large-angle rotation can still ruin the computation; the gap only suppresses the probability.

## Related notes

- [[Robustness of Adiabatic Quantum Computation (Childs-Farhi-Preskill 2001) — Paper Notes]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes]]
- [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes]]
- [[Gap-Adapted Adiabatic Scheduling]]
- [[Adiabatic Error as Sum Over Non-Adiabatic Paths]]
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
