
> **Tags:** #trick #phase-estimation #ancilla-free
> **Source:** arXiv:2505.01528 (SOSSA), Lemma 6

## What it does
Decides whether an eigenphase lies in one of two intervals separated by a known gap — using **no ancilla qubits**.

## The trick
Given unitary $U$ with eigenstate $|\psi\rangle$ at eigenphase $\phi$, and two intervals $I_0$, $I_1$ on the unit circle separated by gap $\delta$: apply a carefully chosen polynomial $p(U)$ (constructed via Chebyshev approximation to a step function) that maps $I_0 \to$ "accept" and $I_1 \to$ "reject". The polynomial is applied via a sequence of controlled applications of $U$ and $U^\dagger$.

No ancilla register needed — the polynomial is applied in place.

## When to reach for it
- Binary decisions about eigenvalues (above/below a threshold)
- When ancilla qubits are expensive or unavailable
- As a subroutine in adaptive [[Gapped Phase Estimation|phase estimation]] (binary search over energy)

## Complexity
$O((1/\delta) \log(1/q))$ queries to $U$ (and $U^\dagger$), where $\delta$ = gap width between the two intervals, $q$ = failure probability. No dependence on eigenphase precision beyond the gap itself — this is a binary decision, not a precision measurement.

## Caveat
Only gives a binary answer (which interval), not a precise eigenvalue. Chain multiple calls for finer resolution.

## Related Paper Notes

- [[SOSSA — Sum-of-Squares Spectral Amplification]]
