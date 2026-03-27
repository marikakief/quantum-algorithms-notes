
> **Source:** Kieferová, Wiebe, *On The Power Of Coherently Controlled Quantum Adiabatic Evolutions*, NJP 16, 123034 (2014); arXiv:1403.6545
> **Tags:** #trick #adiabatic #coherent-control #LCU #diabatic-error #state-preparation

## What it does

Cancels the leading $O(1/T)$ diabatic error in adiabatic state preparation by coherently combining two adiabatic evolutions along different Hamiltonian paths. The result is $O(1/T^2)$ diabatic error without increasing $T$.

## The trick

Prepare an ancilla in $\cos\theta|0\rangle + \sin\theta|1\rangle$. Apply controlled-$U_A$ on path $H(f_A(s))$ (duration $T_A$) and controlled-$U_B$ on path $H(f_B(s))$ (duration $T_B$). Rotate ancilla back and measure. On success (outcome $|0\rangle$):

$$|\psi'\rangle \propto (\cos^2\!\theta\, U_A + \sin^2\!\theta\, U_B)|g(0)\rangle$$

The leading diabatic error for each path is (by the first-order adiabatic expansion):

$$\langle e(1)|U_j|g(0)\rangle = \frac{1}{T_j}\left[\frac{\langle \dot{e}(s)|g(s)\rangle e^{i\int_0^s \gamma_{g,e}(\xi)T_j\,d\xi}}{-i\gamma_{g,e}(s)}\right]_{s=0}^{s=1} + O(1/T_j^2)$$

Choose $f_B$, $T_B$, and $\theta$ so:

$$\cos^2\!\theta \cdot [\text{boundary terms from } U_A] + \sin^2\!\theta \cdot [\text{boundary terms from } U_B] = 0$$

This is always achievable constructively (see [[Antisymmetric Path Design for Adiabatic Error Cancellation]] for how to build $f_B$). The residual error is $O(1/T^2)$.

**Success probability:** $p_0 \geq 1 - \|(U_A - U_B)|g(0)\rangle\|^2 = 1 - O(1/T^2)$ in the adiabatic regime. Failures are heralded; repeat-until-success keeps the expected overhead small.

## When to reach for it

- Adiabatic state preparation where $T$ is fixed (or only modestly improvable) and $O(1/T)$ error is the bottleneck.
- When you want better-than-boundary-cancellation scaling without requiring the Hamiltonian derivatives to vanish at endpoints.
- Combine with boundary cancellation (set $m$ derivatives to zero at boundaries) for $O(1/T^{m+2})$ scaling.
- When the Hamiltonian has spectral symmetry $\gamma(s) = \gamma(1-s)$, use [[Time-Reverse Path Pairing for Global Diabatic Error Suppression]] instead for universal cancellation.

## Complexity

One extra ancilla qubit. Two controlled adiabatic evolutions of durations $T_A$ and $T_B$ (typically comparable). Circuit overhead: single controlled rotation + measurement. Failure probability $O(1/T^2)$, so expected extra cost $O(1/T^2) \cdot (\text{evolution cost})$ — negligible in the adiabatic regime.

## Caveat

- Error cancellation is per-transition in the general case (partially antisymmetric construction). Cancelling every transition simultaneously requires spectral symmetry.
- $T_B$ and $\theta$ depend on the gap integrals $\int_0^1 \gamma_{g,e}(\xi)\,d\xi$, which generally must be estimated numerically.
- The path $f_B$ must be non-monotone (overshoots past 1 or below 0), which can be problematic if the Hamiltonian is ill-defined or poorly conditioned outside $[0,1]$.
- The polynomial-equivalence result (Corollary 3 of the paper) confirms this gives no super-polynomial advantage — the improvement is in the prefactor / error order, not the asymptotic scaling class.

## Related notes
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]]
- [[Antisymmetric Path Design for Adiabatic Error Cancellation]]
- [[Time-Reverse Path Pairing for Global Diabatic Error Suppression]]
- [[Linear Combination of Unitaries (LCU)]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Adiabatic Approximation Boundary-Term Dominance]]
- [[Superadiabatic Smooth Scheduling for Exponential Precision]]
