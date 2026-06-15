
> **Tags:** #trick #phase-estimation #ancilla-free
> **Source:** arXiv:2505.01528 (SOSSA), Lemma 6

This is not the default card for Kitaev/textbook QPE. It is a SOSSA-style interval classifier: use it when the task is to decide which side of a promised phase gap an eigenphase lies on.

## What it does
Decides whether an eigenphase lies in one of two intervals separated by a known gap — using no phase-estimation register and, in the SOSSA lemma, no ancillas beyond the qubit used for the controlled-$U$ construction.

## The trick
Given unitary $U$ with eigenstate $|\psi\rangle$ at eigenphase $\phi$, and two intervals $I_0$, $I_1$ on the unit circle separated by gap $\delta$: apply the SOSSA gapped-phase-estimation unitary, a polynomial transformation built from uses of controlled-$U$ and controlled-$U^\dagger$, that maps one interval close to "accept" and the other close to "reject".

No $b$-qubit QPE register is needed. The output is a binary classifier qubit, not a coherent binary expansion of the phase.

## When to reach for it
- Binary decisions about eigenvalues (above/below a threshold)
- When ancilla qubits are expensive or unavailable
- As a subroutine in adaptive interval localization / binary search over energy

## Complexity
$O((1/\delta) \log(1/q))$ queries to $U$ (and $U^\dagger$), where $\delta$ = gap width between the two intervals, $q$ = failure probability. No dependence on eigenphase precision beyond the gap itself — this is a binary decision, not a precision measurement.

## Caveat
Only gives a binary answer (which interval), not the same output distribution or guarantee as standard QPE. Chaining multiple calls can localize a phase interval, but each call relies on a promised gap at the tested threshold.

## Related Paper Notes

- [[SOSSA — Sum-of-Squares Spectral Amplification]]
