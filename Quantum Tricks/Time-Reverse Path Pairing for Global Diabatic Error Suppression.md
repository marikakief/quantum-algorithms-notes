# Time-Reverse Path Pairing for Global Diabatic Error Suppression

> **Source:** Kieferová, Wiebe, *On The Power Of Coherently Controlled Quantum Adiabatic Evolutions*, NJP 16, 123034 (2014); arXiv:1403.6545
> **Tags:** #trick #adiabatic #symmetry #diabatic-error #state-preparation #coherent-control

## What it does

When the Hamiltonian path has a symmetric gap profile ($\gamma_{g,n}(s) = \gamma_{g,n}(1-s)$ for all $n$), pairing a path $f_A$ with its time-reverse $f_B(s) = 1 - f_A(1-s)$ and combining them with equal weights cancels the $O(1/T)$ diabatic error for **every** excited state simultaneously, with a single ancilla qubit.

## The trick

Symmetry condition: $\gamma_{g,n}(s) = \gamma_{g,n}(1-s)$ for all $s \in [0,1]$, all $n \neq g$. This holds, for example, for Hamiltonians of the form $H(s) = (1-s)H_0 + sH_1$ when $H_0$ and $H_1$ have the same eigenvalue gap structure (symmetric about $s=1/2$).

Set $f_B(s) = 1 - f_A(1-s)$ (time-reversal of $f_A$). Then:

1. **Gap integrals match:** $\int_0^1 \gamma_{g,n}(f_A(s))\,ds = \int_0^1 \gamma_{g,n}(f_B(s))\,ds$ for all $n$ (symmetry of $\gamma$ + substitution $s \to 1-s$).

2. **Boundary derivatives flip sign:** $\dot{f}_B(0) = \dot{f}_A(1)$ and $\dot{f}_B(1) = \dot{f}_A(0)$, so if $f_A$ is constructed so $\dot{f}_A(0) = -\dot{f}_A(1)$ (achievable with the polynomial splice from [[Antisymmetric Path Design for Adiabatic Error Cancellation]]), then $f_A$ and $f_B$ satisfy the completely antisymmetric boundary condition at both endpoints.

3. **Coherent averaging:** Run both under [[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]] with $\theta = \pi/4$, $T_A = T_B$. The $O(1/T)$ amplitude for every transition $g \to e$ cancels:

$$\cos^2\!\tfrac{\pi}{4}\cdot [\text{boundary terms}_A^{(e)}] + \sin^2\!\tfrac{\pi}{4}\cdot [\text{boundary terms}_B^{(e)}] = \tfrac{1}{2}\bigl(\mathcal{B}_A^{(e)} + \mathcal{B}_B^{(e)}\bigr) = 0 \quad \forall\, e \neq g$$

The cancellation holds for all $n$ simultaneously without any transition-specific tuning of $T_B$ or $\theta$.

## When to reach for it

- The Hamiltonian path has a symmetric gap profile (e.g., linear interpolation between two Hamiltonians where the gap is symmetric about the midpoint).
- You want $O(1/T^2)$ diabatic error for **all** transitions with a single two-path gadget.
- Compared to [[Antisymmetric Path Design for Adiabatic Error Cancellation]]: no need to identify the dominant transition, no transition-specific tuning, equal $T_A = T_B$.

## Complexity

Same as [[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]]: one ancilla, two controlled adiabatic evolutions of equal duration. The time-reverse path $f_B$ is trivially computable given $f_A$. Success probability $1 - O(1/T^2)$.

## Caveat

- Requires the gap symmetry condition $\gamma_{g,n}(s) = \gamma_{g,n}(1-s)$. This is not generic — it holds for specific Hamiltonian families (linear interpolations with symmetric spectra) but not in general.
- Even when symmetry holds approximately, cancellation is approximate: the residual from symmetry breaking adds an $O(\delta/T)$ term where $\delta$ measures the asymmetry.
- When symmetry is absent, fall back to the per-transition [[Antisymmetric Path Design for Adiabatic Error Cancellation]] method (cancels one transition at a time).

## Related notes
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]]
- [[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]]
- [[Antisymmetric Path Design for Adiabatic Error Cancellation]]
- [[Adiabatic Approximation Boundary-Term Dominance]]
- [[Local Adiabatic Schedule via Gap-Dependent Evolution Rate]]
