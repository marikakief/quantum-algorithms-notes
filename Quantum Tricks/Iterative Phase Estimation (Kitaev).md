> **Source:** Kitaev, arXiv:quant-ph/9511026 (1995); O'Malley, Babbush et al., arXiv:1512.06860 (2016) [hardware demo]
> **Used in:** [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
> **Tags:** #trick #phase-estimation #QPE #Kitaev #energy-estimation

## What it does

Estimates the phase (eigenvalue) of a unitary $U$ one bit at a time using a single ancilla qubit, rather than requiring $b$ ancilla qubits in parallel. Reduces the ancilla register from $O(b)$ qubits to 1 qubit while keeping total measurement count $O(b/\delta^2)$ for $b$ bits at confidence $1-\delta$.

## The trick

Standard QPE: prepare $b$-qubit ancilla in $|0\rangle^b$, apply QFT + controlled-$U^{2^k}$ for $k = 0, \ldots, b-1$, inverse QFT, measure. Requires all ancilla qubits to be coherent simultaneously.

**Kitaev's iterative version:** extract one bit per round, feed classical outcomes forward.

For round $k$ (extracting bit $j_k$):
1. Prepare ancilla in $(|0\rangle + |1\rangle)/\sqrt{2}$.
2. Apply controlled-$U^{2^k}$ controlled on the ancilla.
3. Apply rotation $Z_{\Phi^{(k)}}$ to the ancilla, where the feed-forward phase compensates for previously measured bits:
   $$\Phi^{(k)} = \pi \sum_{\ell=0}^{k-1} j_\ell\, 2^{\ell-k+1}$$
4. Measure ancilla in the $X$ basis (apply $H$ then measure $Z$). Result $j_k \in \{0,1\}$.
5. After $b$ rounds, reconstruct the phase as a binary fraction:
   $$E_0^b = -\frac{\pi}{t_0} \sum_{k=0}^{b-1} j_k\, 2^{-(k+1)}$$

**Why it works:** After the feed-forward, the ancilla state before measurement is approximately $|j_k\rangle$ in the $X$ basis if the eigenvalue is $e^{i\phi}$. Majority voting over many repetitions (O'Malley et al. use 1000 per bit) gives reliable bit determination even for imperfect hardware.

**Majority vote trick:** If the initial state has overlap $|a_0|^2 > 0.5$ with the target eigenstate, then each individual bit measurement has success probability $> 0.5$. Majority vote over $O(1/\delta^2)$ shots drives the success probability to $1 - \delta$.

In O'Malley et al.: 3 qubits total (1 ancilla + 2 register), circuit for controlled-$U_\text{Trot}$ involves 51+ single-qubit gates and 14+ CZ gates per bit extraction round.

## When to reach for it

- Eigenvalue estimation when you have a controlled-$U$ but can't afford $b$-qubit coherent ancilla registers.
- Pre-fault-tolerant hardware where ancilla coherence is the bottleneck.
- When the target eigenstate has strong overlap ($> 0.5$) with your initial state (e.g., Hartree-Fock for weakly correlated molecules).
- As a hardware-friendly alternative to textbook QPE in near-term experiments.

## Complexity

- **Ancilla qubits:** 1 (vs. $b$ for textbook QPE).
- **Total circuit executions:** $O(b/\delta^2)$ for $b$ bits at confidence $1-\delta$. Each execution runs controlled-$U^{2^k}$ which has circuit cost proportional to $2^k$.
- **Total gate depth across all rounds:** $\sum_{k=0}^{b-1} 2^k \cdot \text{cost}(U) = O(2^b \cdot \text{cost}(U))$ — exponential in $b$ if you want $b$ bits. This is the dominant cost.
- **Compared to textbook QPE:** Same asymptotic gate cost $O(2^b)$, but only 1 ancilla needed.
- **Compared to Hadamard test:** Equivalent for single-qubit ancilla, but iterative version adds feed-forward.

## Caveat

- The majority-voting trick requires $|a_0|^2 > 0.5$. For strongly correlated molecules where the Hartree-Fock state is a poor approximation to the ground state, this fails. Need a better initial state preparation (adiabatic state preparation, or a short VQE circuit to improve overlap).
- Each round requires controlled-$U^{2^k}$. For large $k$, this is an exponentially deep circuit — requires long coherent evolutions. O'Malley et al. could only do a single Trotter step ($\rho = 1$) per round, limiting accuracy to $O(10^{-2})$ Hartree.
- Feed-forward requires real-time classical processing between measurement and the next circuit. This adds latency overhead.
- Not noise-resilient: unlike [[Variational Quantum Eigensolver (VQE)]], there is no classical outer loop to absorb systematic errors. Errors in $U$ directly corrupt the phase estimate.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes]] — adaptive Bayesian approach that outperforms Kitaev by $\sim 10^7\times$ in median error after 150 experiments; replaces the bit-by-bit strategy with Gaussian inference + PGH
- [[Trotterized Time Evolution for Chemistry]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Bravyi-Kitaev Transformation]]
- [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes]] — extends this framework to tolerate SPAM errors via additive error bounds
