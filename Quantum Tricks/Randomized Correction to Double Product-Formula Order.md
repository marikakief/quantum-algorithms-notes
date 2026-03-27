
> **Source:** Cho, Berry, Hsieh, arXiv:2210.11281
> **Tags:** #trick #product-formulas #randomization #hamiltonian-simulation

## What it does

Converts a $(2k)$-th order symmetric [[product formula]] into a $(4k+1)$-th order randomized channel — more than doubling the error order from $2k+1$ to $4k+2$.

## The trick

Given a symmetric [[product formula]] $S_{2k}(\lambda)$ for $H = \sum_j H_j$, write $S_{2k}(\lambda/2) = V(\lambda/2) + D(\lambda/2)$. The error correction $V^\dagger D + D V^\dagger$ decomposes into Hermitian operators $\mathcal{H}_l$ at odd orders $l \in \{2k+1, 2k+3, \ldots, 4k+1\}$:

$$V^\dagger D + D V^\dagger = \sum_{l \in \gamma} \frac{\lambda^l}{2^l} \mathcal{H}_l + O(\lambda^{4k+2})$$

Each $\mathcal{H}_l$ is a linear combination of products of $l$ original Hamiltonian terms. Construct random correction unitaries $U_h^{(l)} = e^{\alpha_{h,l} H_h^{(l)}}$ with sampling probabilities $p_{h,l}$ chosen so the average cancels the error:

$$\sum_{l,h} p_{h,l}\, S_{2k}(\lambda/2)\, U_h^{(l)}\, S_{2k}(\lambda/2) = V(\lambda) + O(\lambda^{4k+2})$$

The [[Diamond-Norm Channel Averaging for Random Compilers|mixing lemma]] converts the average-evolution guarantee into a diamond-norm bound $\|\mathcal{E} - \mathcal{V}\|_\diamond \le a^2 + 2b$ for the resulting channel.

The even-order terms ($2k+2, 2k+4, \ldots, 4k$) vanish automatically because of [[Time-Reversibility Parity Trick for Error Cancellation|time-reversibility parity]] — no need to correct them.

## When to reach for it

- Hamiltonian is a sum of Pauli strings (quantum chemistry, lattice models) so the correction terms $H_h^{(l)}$ are also Pauli strings and directly exponentiable.
- Channel output (diamond-norm error) is acceptable — you're estimating observables, not feeding the simulation into a coherent subroutine.
- You want higher effective order without the gate-count explosion of moving to the next Suzuki level.

## Complexity

Number of exponentials: $O\bigl(tL^2(tL/\varepsilon)^{1/(4k+1)}\bigr)$, where $L$ is the number of Hamiltonian terms. Improves over deterministic $(2k)$-th order Suzuki ($\varepsilon^{-1/(2k)}$) and matches or improves the [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Ostrander-Su]] randomized formula.

## Caveat

Only works efficiently when $H_j$ are structured (Pauli strings). For oracle-access sparse Hamiltonians, the correction terms are products of oracle-accessed operators and not cheaply exponentiable. Also, the output is a quantum channel, not a unitary — can't be used as a coherent subroutine.

## Related notes
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes]]
- [[Time-Reversibility Parity Trick for Error Cancellation]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Permutation Averaging for Product-Formula Error Cancellation]]
- [[Symplectic Corrector Injection for Product Formulas]]
