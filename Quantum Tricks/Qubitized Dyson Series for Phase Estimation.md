
> **Source:** Su, Berry, Wiebe, Rubin, Babbush, arXiv:2105.12767 (2021)
> **Tags:** #trick #qubitization #Dyson-series #interaction-picture #phase-estimation #amplitude-amplification

## What it does

Replaces amplitude amplification in Dyson-series-based eigenvalue estimation with qubitization of $\sin(H\tau)$. In Su et al.'s first-quantized chemistry implementation, this gives the reported constant-factor savings over their amplitude-amplified Dyson alternatives; it is not a universal $3/2$ theorem independent of implementation details.

## The trick

In the standard interaction picture, one Dyson-series step approximates $e^{-i(A+B)\tau}$ as an LCU with sum of amplitudes $\beta \approx e^{\tau\lambda_B}$. Success probability is $1/\beta$. To make each step deterministic, oblivious amplitude amplification is used: forward step, reverse step, forward step — $3\times$ cost.

Instead: don't try to make the time evolution deterministic. Block-encode $\sin((A+B)\tau)$ by using a single ancilla qubit to coherently sum $e^{-i(A+B)\tau}$ and $e^{+i(A+B)\tau}$. This is *not* a unitary evolution operator — it's a Hermitian operator whose eigenvalues are $\mu_j = \sin(E_j\tau)$.

Now apply [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]: form a walk operator $W$ whose phase branches encode $\arcsin(\mu_j/\beta)$ up to signs. Phase estimation on $W$ extracts $\arcsin(\sin((E_j-E_0)\tau)/\beta)$ after shifting by an energy offset $E_0$, from which $E_j$ is recovered in the chosen energy window.

Choosing $\tau = 1/\lambda_B$ (rather than $\tau = \ln 2/\lambda_B$ as in the amplitude-amplified approach) gives $\beta = e$ in the source construction and effective evolution time $\tau_{\mathrm{eff}} = \tau/\beta = 1/(e\lambda_B)$. The number of phase estimation steps is

$$N = \left\lceil \frac{\pi e \lambda_B}{2\varepsilon_{\mathrm{pha}}} \right\rceil$$

**Cost comparison:**

Here $C$ denotes the cost of one source-specific Dyson LCU implementation, including sorting/time-register preparation and the potential-oracle work used in that comparison.

| Method | Steps | Cost per step | Total |
|---|---|---|---|
| Amplitude amplification (sorting, [69]) | $\lambda_B t/\ln 2$ | $3C$ | $3C\lambda_B t/\ln 2$ |
| Amplitude amplification (inequality, [3]) | $2\lambda_B t$ | $3C$ | $6C\lambda_B t$ |
| Qubitized Dyson (this work, sorting) | $e\lambda_B t$ | $C$ | $eC\lambda_B t$ |

The qubitized approach uses $eC\lambda_B t$ total cost vs. $3C\lambda_B t/\ln 2 \approx 4.33C\lambda_B t$ for the amplitude-amplified sorting implementation, giving a factor of $4.33/e \approx 1.59$ improvement in that comparison. Over the particular inequality-test method compared in the paper: $6/e \approx 2.21$.

## When to reach for it

- Any Dyson-series simulation where the goal is eigenvalue estimation (phase estimation), not time evolution
- The cost is dominated by the Dyson-series LCU implementation (not the $e^{-iA\tau}$ parts)
- The eigenvalue of interest is placed near zero by an energy offset $E_0$, so the arcsin nonlinearity is controlled in the target energy window

## Complexity

Number of steps in the linearized source setting: $\lceil\pi e\lambda_B/(2\varepsilon)\rceil$. Each step applies the Dyson-series LCU once (not three times). The ancilla qubit controlling the sine construction adds small phase-control overhead on time registers.

Nonlinearity of $\arcsin$: if $E_j - E_0$ is not small, the effective step count increases by $O((E_j - E_0)^2\tau^2)$ relative to the linearised case. This is handled by choosing $E_0$ close to the target energy.

## Caveat

Only works for phase estimation / eigenvalue estimation. If you need actual time evolution $e^{-iHt}$ (e.g., for dynamics), you still need amplitude amplification.

The $\arcsin$ nonlinearity means you need an initial energy estimate $E_0$ close enough to the target eigenvalue to choose the correct phase branch and keep the target in the near-linear regime. For ground-state estimation this is typically supplied by classical methods (Hartree-Fock, coupled-cluster estimates, etc.).

The factor-of-$3/2$ improvement seems modest, but since it multiplies every Dyson-series step, the absolute savings are significant — especially for the interaction-picture chemistry simulation where each step involves expensive potential block encodings.

## Related notes
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the qubitization framework applied here to $\sin(H\tau)$
- [[Interaction Picture Simulation (Kinetic Frame)]] — the simulation technique this optimises
- [[Truncated Taylor Series Simulation]] — the underlying LCU / Dyson series formalism
