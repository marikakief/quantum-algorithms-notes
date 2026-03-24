# Gearbox Circuit for Angle Squaring

> **Source:** Wiebe, Kliuchnikov, arXiv:1305.5528
> **Tags:** #trick #gate-synthesis #repeat-until-success #T-count #Clifford-T

## What it does

Given a unitary $U$ with off-diagonal amplitude $|U_{10}| = \sin\theta$, probabilistically implements $R_x(2\arctan(\tan^2\theta)) \approx R_x(2\theta^2)$ for small $\theta$ using one ancilla qubit and a constant T overhead.

## The trick

**Circuit (one ancilla, $d=1$ case):**
1. Initialise ancilla $|0\rangle$.
2. Apply $U$ to ancilla.
3. Apply controlled $-iX$ (control = ancilla, target = system).
4. Apply $U^\dagger$ to ancilla.
5. Measure ancilla in the computational basis.

**Outcome:**
- Measurement $= 0$ (success, probability $P = \cos^4\theta + \sin^4\theta$): system receives $e^{-iX\arctan(\tan^2\theta)}$.
- Measurement $= 1$ (failure): system receives $e^{i\pi X/4}$ (a known Clifford). Correct it and retry.

For $\theta \ll 1$: $\arctan(\tan^2\theta) \approx \theta^2$, so this squares the rotation angle. Success probability $\approx 1 - 2\theta^4$ — nearly certain for small angles.

**T-count:** $T(U) + T(U^\dagger) + 0 = 2T(U)$ (no T gates in the controlled $-iX$ for $d=1$). For the $d$-ancilla generalisation: $4(d-1) + 2\sum_j T(U_j)$.

**Iterated squaring:** applying the gearbox $d$ times to $U$ (composed gearbox $C^{\circ d}$) squares the angle at each level, giving final angle $\approx \theta_0^{2^d}$ after $d$ successful applications.

## When to reach for it

- You need a very small rotation $\phi \ll \epsilon$ (much smaller than your synthesis precision), and the cost of synthesizing $\phi$ directly with [[Solovay-Kitaev Recursive Gate Compilation]] or Selinger's method would be dominated by $\log(1/\phi)$.
- You can afford one or a few ancilla qubits and mid-circuit measurement.
- The target angle has a known "base" that can be expressed as $\theta_0^{2^d}$ for manageable $\theta_0$.

## Complexity

Per level: $2T(U_{\text{base}})$ T gates plus $4(d-1)$ for the multi-controlled gate.
Expected number of retries per level: $1/P \approx 1 + O(\theta^4)$ — negligible for small angles.
Total expected T-count after $d$ levels: $O(d \cdot T(U_{\text{base}}))$ where $d = O(\log\log(1/\phi))$.

## Caveat

- Failures yield a Clifford, not an arbitrary error — so retrying is safe. But you must track which Clifford correction to apply before retrying.
- Only works for angle squaring, not arbitrary angle arithmetic.
- Requires ancilla qubits with mid-circuit measurement capability.
- The base rotation $U$ still needs to be synthesized by some other method (Selinger, Fowler, etc.).

## Related notes

- [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes]]
- [[Repeat-Until-Success with Clifford Correction]]
- [[Floating-Point Angle Decomposition for Rotation Synthesis]]
- [[Solovay-Kitaev Recursive Gate Compilation]]
