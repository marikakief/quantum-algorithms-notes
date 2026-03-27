
> **Source:** Kieferová, Wiebe, *On The Power Of Coherently Controlled Quantum Adiabatic Evolutions*, NJP 16, 123034 (2014); arXiv:1403.6545
> **Tags:** #trick #adiabatic #path-design #diabatic-error #polynomial-interpolation #state-preparation

## What it does

Constructs a second adiabatic path $f_B(s)$ (given a base path $f_A(s)$) such that their boundary derivative terms have opposite signs, enabling the coherent-averaging gadget ([[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]]) to cancel leading diabatic errors. The construction uses polynomial interpolation near one or both endpoints.

## The trick

The $O(1/T)$ diabatic error depends only on boundary terms at $s=0,1$ (see [[Adiabatic Approximation Boundary-Term Dominance]]). The sign of the $s=1$ boundary contribution depends on $\dot{f}(1)$.

**Partially antisymmetric construction** — flip the derivative at $s=1$ only:

$$\dot{f}_B(0) = \dot{f}_A(0), \qquad \dot{f}_B(1) = -\dot{f}_A(1)$$

Near $s = 1-\Delta$, splice in a quartic polynomial:

$$f_B(s) = \begin{cases} f_A(s) & s < 1-\Delta \\ es^4 + ds^3 + cs^2 + bs + a & s \geq 1-\Delta \end{cases}$$

Solve the 5 coefficients by matching: $f_B(1-\Delta) = f_A(1-\Delta)$, $\dot{f}_B(1-\Delta) = \dot{f}_A(1-\Delta)$, $\ddot{f}_B(1-\Delta) = \ddot{f}_A(1-\Delta)$, $f_B(1) = 1$, $\dot{f}_B(1) = -\dot{f}_A(1)$. Note: $f_B$ must transiently exceed 1 (overshoot) to achieve the sign flip while returning to $f_B(1)=1$.

Then choose $T_B$ and $\theta$:

$$T_B = \frac{\int_0^1 \gamma_{A,g,e}(\xi)\,d\xi}{\int_0^1 \gamma_{B,g,e}(\xi)\,d\xi}\,T_A + \frac{(2n+1)\pi}{\int_0^1 \gamma_{B,g,e}(\xi)\,d\xi}, \qquad \theta = \arctan\!\sqrt{T_B/T_A}$$

This cancels the $O(1/T)$ amplitude for the chosen transition $g \to e$.

**Completely antisymmetric construction** — flip at both endpoints:

$$\dot{f}_B(0) = -\dot{f}_A(0), \qquad \dot{f}_B(1) = -\dot{f}_A(1)$$

Requires two polynomial splices (near $s=0$ and $s=1$). The matching conditions are analogous; $f_B$ overshoots near both endpoints. The timing condition simplifies when gap integrals match:

$$\int_0^1 \gamma_{A,g,e}(\xi)\,d\xi = \int_0^1 \gamma_{B,g,e}(\xi)\,d\xi \pmod{2\pi}$$

At $\theta = \pi/4$ and $T_A = T_B$ this gives cancellation.

**Higher-order extension**: Combine with boundary cancellation (set first $m$ derivatives of $H$ to zero at boundaries). Use degree-$(m+3)$ or higher polynomial splices to match $m+1$ derivatives at the endpoints with opposite sign. Residual error becomes $O(1/T^{m+2})$.

## When to reach for it

- You have a working adiabatic path $f_A$ and want to build the complementary path for [[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]].
- The Hamiltonian is well-defined slightly outside $[0,1]$ (so the transient overshoot of $f_B$ is safe).
- You can compute (or estimate) the gap integrals $\int_0^1 \gamma_{g,e}(f_j(s))\,ds$ to set $T_B$ and $\theta$.

## Complexity

Path design is classical preprocessing. Evaluating the splice polynomial adds $O(1)$ overhead per time step. The gap integrals typically require numerical estimation; if $T$ is large and the gap is smooth, a coarse numerical integral suffices (the result only needs to be accurate to precision $\sim 1/T$).

## Caveat

- Cancels one transition at a time in the partially antisymmetric case. If multiple transitions are dangerous, you need either the symmetric-spectrum shortcut ([[Time-Reverse Path Pairing for Global Diabatic Error Suppression]]) or a multi-path gadget.
- The overshoot in $f_B$ means the system transiently experiences $H(s)$ with $s > 1$. If the Hamiltonian is only defined on $[0,1]$ or has a phase transition just above $s=1$, this can fail.
- $T_B - T_A \geq (2n+1)\pi / \int_0^1 \gamma_{B,g,e}\,ds$ — the two durations can't be equal in the partially antisymmetric case; there's a minimum gap.
- Piecewise $C^3$ smoothness of the composed path is required for the adiabatic error expansion to hold; the splice polynomials are designed to achieve this.

## Related notes
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes]]
- [[Coherent Averaging of Adiabatic Paths for Diabatic Error Cancellation]]
- [[Time-Reverse Path Pairing for Global Diabatic Error Suppression]]
- [[Adiabatic Approximation Boundary-Term Dominance]]
- [[Superadiabatic Smooth Scheduling for Exponential Precision]]
