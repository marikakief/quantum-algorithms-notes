# Pre-History State as Guided Sparse Hamiltonian Input

> **Source:** Gyurik, Schmidhuber, King, Dunjko, Hayakawa, arXiv:2410.21258 (2024); technique adapted from Cade, Folkertsma, Gharibian et al. (ICALP 2023)
> **Tags:** #trick #history-state #guided-Hamiltonian #tda #BQP-hardness

## What it does

Constructs a guiding state for the [[Guided Sparse Hamiltonian Framework|guided sparse Hamiltonian]] / kernel overlap problem that is (a) efficiently classically describable, (b) efficiently quantumly preparable, and (c) has tunable $1 - 1/\text{poly}(n)$ overlap with the actual history state.

## The trick

Given a circuit $U_x = U_T \cdots U_1$ preceded by $L$ identity gates ($U_x = U_T \cdots U_1 \cdot I^L$), the full [[History-State Encoding with Unary Clock|history state]] is:

$$|\psi_{\text{hist}}\rangle = \frac{1}{\sqrt{2(T+L)}} \sum_{t=1}^{T+L} \left( |C_t\rangle \otimes |\psi_{t-1}\rangle + |C_t'\rangle \otimes |\psi_t\rangle \right)$$

Define the **pre-history state** using only the first $L$ time steps (where all gates are identity):

$$|\psi_{\text{prehist}}\rangle = \frac{1}{\sqrt{2L}} \sum_{t=1}^{L} \left( |C_t, 0^m\rangle + |C_t', 0^m\rangle \right)$$

Since the computation register is $|0^m\rangle$ throughout these steps, the pre-history state is a [[Subset States for Succinct Description of High-Dimensional Chains|subset state]] with an efficient classical description — just list the clock configurations and the all-zeros computation register.

The overlap with the history state is:

$$|\langle\psi_{\text{prehist}} | \psi_{\text{hist}}\rangle|^2 = \frac{L}{L+T}$$

By choosing $L = O(\text{poly}(n) \cdot T)$, this can be made $1 - 1/r(n)$ for any polynomial $r(n)$.

## When to reach for it

- BQP-hardness reductions that need a classically describable guiding state with near-unit overlap with the ground state
- Any construction where the circuit-to-Hamiltonian encoding produces a history state but you need an efficiently specified approximation
- Persistence problems where the "input harmonic" must be a subset state

## Complexity

The pre-history state has description size $O(L \cdot N)$ and can be prepared as a quantum state in $O(L \cdot N)$ gates. The tradeoff: increasing $L$ improves overlap but increases the total Hamiltonian size (more qubits in the clock register, more terms in $H_{\text{prop}}$). Everything stays polynomial.

## Caveat

The pre-idling technique only works for BQP$_1$ (perfect completeness) reductions. For standard BQP (two-sided error), the history state is in the kernel only in the YES case with certainty. For BQP, you'd need the Low-Energy Overlap variant (Theorem 2 of the paper) rather than Kernel Overlap.

## Related notes
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]]
- [[History-State Encoding with Unary Clock]]
- [[Guided Sparse Hamiltonian Framework]]
- [[Subset States for Succinct Description of High-Dimensional Chains]]
- [[Kernel Overlap as BQP-Complete Primitive]]
