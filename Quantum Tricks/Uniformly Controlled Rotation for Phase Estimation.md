# Uniformly Controlled Rotation for Phase Estimation

> **Source:** Wecker, Hastings, Wiebe, Clark, Nayak, Troyer, arXiv:1506.05135
> **Tags:** #trick #phase-estimation #circuit-optimization #gate-count

## What it does
Reduces the depth and rotation count of [[Iterative Phase Estimation (Kitaev)|quantum phase estimation]] by a factor of $\sim 2\times$ (depth) and $\sim 4\times$ (rotations) when the gate set consists of Cliffords plus arbitrary $Z$-rotations.

## The trick
Standard QPE controls the evolution: ancilla $|0\rangle$ → identity, ancilla $|1\rangle$ → $e^{-iHt}$. Replace this with:

- Ancilla $|0\rangle$ → $e^{+iHt/2}$ (backward evolution)
- Ancilla $|1\rangle$ → $e^{-iHt/2}$ (forward evolution)

The relative phase is still $e^{-iHt}$, so QPE works identically. But now:

1. **Half the Trotter steps** — simulate for $t/2$ instead of $t$
2. **Simpler controlled gates** — each $R_z(\theta)$ in the Trotter circuit becomes a *uniformly controlled rotation*: rotate by $+\theta$ if ancilla is $|0\rangle$, by $-\theta$ if $|1\rangle$. Implementation: CNOT, $R_z(\theta)$, CNOT — just 3 gates instead of 4 for a standard controlled rotation.

Net savings: $>2\times$ circuit depth, $4\times$ fewer arbitrary rotations. The Clifford gate count stays the same.

## When to reach for it
Any QPE-based algorithm where Trotter evolution provides the controlled unitary. Especially valuable when arbitrary rotations are the expensive operation (e.g., magic state distillation in fault-tolerant computation).

## Complexity
Saves one arbitrary rotation per controlled gate in the Trotter circuit. For a Trotter step with $k$ rotation gates: saves $k$ rotations per step and halves the number of steps.

## Caveat
Only works when the gate set treats Cliffords as cheap and rotations as expensive. If all gates cost the same (e.g., native gate set includes arbitrary rotations), the savings are marginal. Also requires that both $e^{+iHt/2}$ and $e^{-iHt/2}$ can be implemented — the reverse evolution circuit must exist, which it always does for time-independent Hamiltonians.

## Related notes
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Phase Gradient State for Controlled Rotations]]
