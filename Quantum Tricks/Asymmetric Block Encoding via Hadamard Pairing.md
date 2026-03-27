
> **Source:** Costa, An, Sanders, Su, Babbush, Berry, arXiv:2111.08152
> **Tags:** #trick #block-encoding #qubitization #adiabatic #circuit-simplification

## What it does

Simplifies a parameter-dependent [[Qubitization (Quantum Walk for Spectral Encoding)|block encoding]] by replacing the inverse rotation $R(s)^\dagger$ with a fixed Hadamard gate. This avoids computing and applying the inverse schedule rotation at every step, while preserving the symmetric structure required for qubitization.

## The trick

In the QLSP adiabatic algorithm, the Hamiltonian $H(s) = (1 - f(s))H_0 + f(s)H_1$ is block-encoded using a rotation

$$R(s) = \frac{1}{\sqrt{(1-f)^2 + f^2}} \begin{pmatrix} 1-f & f \\ f & -(1-f) \end{pmatrix}$$

to select between the block encodings of $H_0$ and $H_1$. The standard approach uses $R(s)$ before and $R(s)^\dagger$ after the SELECT operation.

**The simplification:** replace $R(s)^\dagger$ with a Hadamard $H$. This block-encodes $A(f(s))/\sqrt{2[(1-f)^2 + f^2]}$ instead of $A(f(s))/[(1-f)^2 + f^2]$ — the normalization changes (to between $1/\sqrt{2}$ and $1$) but the encoded operator is the same up to rescaling.

**Maintaining symmetry:** Qubitization requires a symmetric block encoding. With one block using $R(s)$ at the start and $H$ at the end, the individual block is asymmetric. The fix: the full Hamiltonian $H(s)$ has two blocks (corresponding to $Q_b A(f)$ and $A(f) Q_b$). Use $R(s)$-then-$H$ for one block and $H$-then-$R(s)$ for the other. The overall encoding is symmetric.

## When to reach for it

- Adiabatic algorithms where the Hamiltonian interpolation requires a parameter-dependent rotation in the block encoding.
- Any setting where computing and applying $R(s)^\dagger$ is expensive (e.g., when the rotation angles require arctan evaluations).
- Circuit simplification for time-dependent block encodings where the symmetry can be restored by pairing blocks.

## Complexity

Saves one $R(s)$ synthesis per walk step (replaced with a single Hadamard). For circuits with $T = O(\kappa)$ walk steps, this saves $O(\kappa)$ arbitrary rotations.

## Caveat

The normalization factor changes, which slightly reduces the spectral gap (by at most $1/\sqrt{2}$). For the QLSP, this translates to a $\sqrt{2}$ factor in the gap bounds for the non-positive-definite case — small but present.

Only works when the Hamiltonian has a natural two-block structure where the asymmetry can be "swapped" between blocks.

## Related notes
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
