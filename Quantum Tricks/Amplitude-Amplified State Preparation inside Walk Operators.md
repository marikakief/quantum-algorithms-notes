
> **Source:** Dominic W. Berry and Andrew M. Childs, *Black-box Hamiltonian simulation and unitary implementation*, arXiv:0910.4157  
> **Tags:** #trick #state-preparation #amplitude-amplification #quantum-walk

## Idea

In the non-sparse setting, the walk states $|\varphi_j\rangle \propto \sum_k \sqrt{H^*_{jk}}|k\rangle$ have amplitudes proportional to $\sqrt{H^*_{jk}}$. Preparing these exactly requires knowing all $M$ matrix elements in row $j$, costing $O(M)$ queries per state. Instead, use amplitude amplification (Grover 2000) to [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] these states using $O(M^{2/3})$ queries.

## How it works (Section V of 0910.4157)

The state $|\varphi_j\rangle$ must satisfy an orthogonality condition with the ancilla qubit (due to the modified isometry $T$ with an ancilla qubit). The key observation is that a uniform superposition $|u\rangle = M^{-1/2}\sum_k |k\rangle$ has overlap

$$
|\langle u|\varphi_j\rangle|^2 = \frac{\sigma_j}{M\|H\|_1}
$$

with the desired state. Amplitude amplification boosts this overlap from $\sqrt{\sigma_j/(M\|H\|_1)}$ to near 1, using $O(\sqrt{M\|H\|_1/\sigma_j})$ queries per preparation attempt.

In the worst case $\sigma_j \sim \|H\|_1/M$ (uniform row), this is $O(M)$ — no gain. But averaged over rows (weighted by the walk's stationary distribution), the gain gives a total cost of $O(M^{2/3})$ queries per walk step, leading to the $D^{2/3}$ complexity of Theorem 2.

## The ancilla orthogonality condition

The paper modifies the [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians|isometry $T$]] relative to the Szegedy walk (Ref. [10] in the paper) by including an ancilla qubit to satisfy an orthogonality condition that makes amplitude amplification more efficient. This is one of the technical innovations of the paper (listed as point 3 in Section I).

## Caveat

The $O(M^{2/3})$ bound is a worst-case average. For structured Hamiltonians with non-uniform row norms, the actual gain depends on the distribution of $\sigma_j$. For truly sparse Hamiltonians with $D \ll M$, the sparse preparation method (Section IV) is better: $O(D)$ per walk step.

## Related notes
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]]
- [[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians]]
- [[Lazy-Walk Phase-Correction for Simulation Accuracy]]
