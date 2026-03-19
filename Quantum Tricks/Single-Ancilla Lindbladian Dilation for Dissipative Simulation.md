# Single-Ancilla Lindbladian Dilation for Dissipative Simulation

> **Source:** Ding, Chen & Lin, arXiv:2308.15676
> **Tags:** #trick #Lindbladian #open-systems #single-ancilla #dilation

## What it does
Simulates one step of dissipative (Lindblad) dynamics $e^{\mathcal{L}_K \tau}[\rho]$ using only **one ancilla qubit**, to first-order accuracy in $\tau$.

## The trick

Given a jump operator $K$, construct the dilated Hermitian operator on one ancilla qubit plus the system:

$$\tilde{K} = \begin{pmatrix} 0 & K^\dagger \\ K & 0 \end{pmatrix}$$

Then the partial trace

$$\sigma(\tau) = \text{Tr}_a\!\left[e^{-i\tilde{K}\sqrt{\tau}}\,(|0\rangle\langle 0| \otimes \rho)\, e^{i\tilde{K}\sqrt{\tau}}\right]$$

satisfies $\|\sigma(\tau) - e^{\mathcal{L}_K \tau}[\rho]\|_1 = O(\|K\|^4 \tau^2)$.

The key insight: the $\sqrt{\tau}$ evolution time. Expanding $e^{-i\tilde{K}\sqrt{\tau}}$ to second order and tracing out the ancilla, the first-order terms in $\sqrt{\tau}$ vanish (they're off-diagonal in the ancilla), while the second-order terms ($\propto \tau$) reproduce exactly $K\rho K^\dagger$ and $-\frac{1}{2}\{K^\dagger K, \rho\}$ — the Lindblad dissipator. Higher-order terms contribute $O(\tau^2)$ error.

This is much cheaper than full [[Stinespring Dilation for Open-System Simulation]], which requires an environment register of dimension matching the number of Kraus operators. Here: one qubit, regardless of the dimension of $K$.

## When to reach for it
- Simulating Lindbladian dynamics on near-term or early fault-tolerant hardware where ancilla qubits are expensive
- Ground state preparation via dissipative quantum computing
- Any algorithm that iteratively applies a non-unitary channel and can tolerate $O(\tau^2)$ local error per step
- Quantum MCMC / quantum Gibbs sampling circuits

## Complexity
- **Ancilla qubits:** 1
- **Per-step accuracy:** $O(\|K\|^4 \tau^2)$ — first-order in $\tau$
- **Evolution time:** $\sqrt{\tau}$ (of the dilated Hamiltonian $\tilde{K}$)
- Combined with Trotter splitting for $K = \sum_l H_l$: second-order Trotter gives no improvement because the dilation itself is the accuracy bottleneck

## Caveat
Only first-order accurate in $\tau$. Getting higher accuracy requires either very small $\tau$ (many steps, more total cost) or a different approach. The paper notes that this first-order bottleneck — not the Trotter splitting — is what limits the continuous-time simulation cost.

## Related notes
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
- [[Stinespring Dilation for Open-System Simulation]]
- [[Entropy Pumping for State Preparation]]
