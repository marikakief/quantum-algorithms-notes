# Gapped Phase Estimation for Eigenvalue Bucketing

> **Source:** Childs, Kothari, Somma, arXiv:1511.02306 (Lemma 22)
> **Tags:** #trick #phase-estimation #eigenvalue-bucketing #QLSA #variable-time

## What it does

Distinguishes eigenphases in two separated regions — $|\theta| \le \phi$ vs $|\theta| \ge 2\phi$ — without resolving the eigenvalue precisely, using $O(\frac{1}{\phi}\log\frac{1}{\varepsilon})$ queries.

## The trick

Standard [[Amplitude Estimation via Phase Estimation on Q|phase estimation]] resolves eigenphases to precision $\delta$ using $O(1/\delta)$ queries. That precision is often overkill — you just need to sort eigenvalues into coarse buckets.

**Gapped phase estimation (GPE)** exploits the promise gap: if $|\theta| \le \phi$ or $|\theta| \ge 2\phi$, a single round of phase estimation with $O(1/\phi)$ queries succeeds with probability $> 1/2$. Repeating $O(\log(1/\varepsilon))$ times and taking majority vote gives error at most $\varepsilon$.

Formally, $\mathrm{GPE}(\phi, \varepsilon)$ acts on $|0\rangle_C |0\rangle_P |\theta\rangle$ to produce $(\beta_0|0\rangle_C|\gamma_0\rangle_P + \beta_1|1\rangle_C|\gamma_1\rangle_P)|\theta\rangle$, where:
- If $|\theta| \le \phi$: $|\beta_1| \le \varepsilon$
- If $|\theta| \ge 2\phi$: $|\beta_0| \le \varepsilon$

The control register $C$ is 1 qubit; the phase register $P$ has $O(\log(1/\phi) \cdot \log(1/\varepsilon))$ qubits.

**Key application:** set $\phi_j = 2^{-j}$ for $j = 1, \ldots, \lceil\log_2 \kappa\rceil + 1$. Running GPE sequentially buckets eigenvalues into geometrically increasing ranges $[2^{-j-1}, 2^{-j}]$. Within each bucket, apply a version of $A^{-1}$ optimised for that eigenvalue range. This is the foundation of the [[Variable-Time Amplification for Condition-Number Reduction|VTAA construction]] that achieves near-linear $\kappa$ scaling.

## When to reach for it

- When you need to branch on eigenvalue magnitude without full resolution
- In [[Variable-Time Amplification for Condition-Number Reduction|variable-time algorithms]] where different eigenvalue ranges require different subroutines
- When full phase estimation is too expensive but a binary classification suffices
- Any algorithm where the cost scales with eigenvalue magnitude and you want to avoid worst-case behaviour

## Complexity

$O\!\left(\frac{1}{\phi}\log\frac{1}{\varepsilon}\right)$ queries to controlled-$U$. Ancilla: $O(\log(1/\phi) \cdot \log(1/\varepsilon))$ qubits.

## Caveat

Only distinguishes two promised regions with a gap of factor 2 between them. Eigenvalues in the transition region $(\phi, 2\phi)$ get an arbitrary superposition of both labels. This is fine for the VTAA application (eigenvalues in the transition region are correctly handled by either the $j$th or $(j+1)$th bucket), but not for applications requiring sharp cutoffs.

## Related notes
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Variable-Time Amplification for Condition-Number Reduction]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes]]
