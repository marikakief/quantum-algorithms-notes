# Ground-State Recycling in Serial Amplitude Estimation

> **Source:** O'Brien, Streif, Rubin, Santagati, Su, Huggins et al., arXiv:2111.12437
> **Tags:** #trick #amplitude-estimation #state-preparation #fault-tolerant #overlap-estimation

## What it does

After the overlap estimation algorithm (OEA) finishes, recovers the ground state $|\psi\rangle$ from the post-measurement state at average cost $\leq 2T_R$ (two reflection operations), avoiding full state re-preparation between serial expectation value estimates.

## The trick

The OEA estimates $\langle\psi|U_i|\psi\rangle$ via phase estimation of the Szegedy walk $S_i = R U_i R U_i^\dagger$ where $R = I - 2|\psi\rangle\langle\psi|$. After QPE, the system register is in one of the eigenstates $|s_i^\pm\rangle = \frac{1}{\sqrt{2}}(|\psi\rangle \pm i|\chi_i\rangle)$, which has overlap $|\langle s_i^\pm | \psi\rangle| = 1/\sqrt{2}$ — independent of the expectation value being estimated.

To recover $|\psi\rangle$:
1. Perform a Hadamard test on the reflection $R = I - 2|\psi\rangle\langle\psi|$: apply $R$ controlled on an ancilla in $|+\rangle$, measure the ancilla in the $X$ basis.
2. With probability $1/2$, the ancilla flips to $|-\rangle$ and the system is projected onto $|\psi\rangle$. Done.
3. If the ancilla stays in $|+\rangle$, the system is in $|\chi_i\rangle$. Apply $U_i^\dagger$ (or $U_i$), which maps $|\chi_i\rangle \to \sqrt{1 - (dE/(dR_i\lambda_{F_i}))^2}\,|\psi\rangle - \ldots$. Repeat the Hadamard test.
4. Iterate. The probability of failing at each subsequent step is $(dE/(dR_i\lambda_{F_i}))^{2(n-1)}$, which converges geometrically.

Average cost: $T_{P,i>1} = (1 + \frac{1}{2(1 - (dE/(dR_i\lambda_{F_i}))^2)}) T_R$. When $dE/dR_i \ll \lambda_{F_i}$ (typical), this approaches $\frac{3}{2} T_R$.

The total preparation cost across all $3N_a$ force components is then:

$$\sum_i T_{P,i} \in \tilde{O}\left(N_a T_R + a_0^{-1}(T_R + T_\phi + N)\right)$$

The $a_0^{-1}$ factor (initial state overlap) is additive with $N_a$, not multiplicative.

## When to reach for it

- Serial estimation of multiple expectation values on the same quantum state, where each uses amplitude/overlap estimation with a Szegedy walk.
- Any setting where state preparation is expensive relative to the walk operator cost — the trick converts the $O(N_a / a_0)$ multiplicative cost into $O(N_a + 1/a_0)$ additive cost.
- Force estimation is the motivating application, but the same trick applies to estimating multiple RDM elements, correlation functions, or any collection of observables via OEA.

## Complexity

Average cost per recycling: $\leq 2T_R$ (assuming $|dE/dR_i| / \lambda_{F_i} \leq 0.5$). First preparation: $\tilde{O}(a_0^{-1} T_R)$ via fixed-point amplitude amplification.

## Caveat

The cost *diverges* if $|dE/dR_i|$ approaches $\lambda_{F_i}$ — the recycling probability drops to zero when the block-encoded expectation value saturates. This is the opposite of what happens in NISQ, where large expectation values are easier to estimate. In practice, for molecular forces, $|dE/dR_i| \ll \lambda_{F_i}$, so this is not a concern. If it were, one could artificially inflate $\lambda_{F_i}$ at the cost of reduced precision per oracle call.

The reflection $R = I - 2|\psi\rangle\langle\psi|$ must be implemented approximately (via QSVT sign function), so each recycling step inherits the reflection cost $T_R \in \tilde{O}(T_H \lambda_H / \gamma)$.

## Related notes
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Gradient Encoding for Multi-Observable Estimation]]
- [[Dolph-Chebyshev Eigenstate Filtering]]
