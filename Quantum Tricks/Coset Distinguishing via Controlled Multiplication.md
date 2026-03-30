# Coset Distinguishing via Controlled Multiplication

> **Source:** Watrous, arXiv:cs/0009002 (2000)
> **Tags:** #trick #group-theory #coset-states #QMA #membership-testing

## What it does

Distinguishes whether an element $h$ belongs to a subgroup $H$ by testing whether $|H\rangle$ and $|Hh\rangle$ are the same state or orthogonal, using controlled multiplication and a Hadamard.

## The trick

Given a register $R$ containing $|H\rangle$ (the uniform superposition over subgroup $H$):

1. Prepare $|+\rangle = (|0\rangle + |1\rangle)/\sqrt{2}$ on control qubit $B$.
2. Apply controlled-multiply-by-$h$ on $R$, controlled by $B$:
   $$|0\rangle|H\rangle + |1\rangle|H\rangle \;\mapsto\; |0\rangle|H\rangle + |1\rangle|Hh\rangle$$
3. Apply Hadamard to $B$:
   $$\frac{1}{2}|0\rangle(|H\rangle + |Hh\rangle) + \frac{1}{2}|1\rangle(|H\rangle - |Hh\rangle)$$
4. Measure $B$.

**If $h \in H$:** $|Hh\rangle = |H\rangle$, so the $|1\rangle$ branch vanishes. Always measure $|0\rangle$. Acceptance probability = 0.

**If $h \notin H$:** $\langle H|Hh\rangle = 0$ (orthogonal cosets), so $\|(|H\rangle - |Hh\rangle)/2\|^2 = 1/2$. Measure $|1\rangle$ with probability $1/2$.

## When to reach for it

- Testing membership in a subgroup given the uniform superposition state
- As the "test" step in QMA verification for group non-membership
- Generalises to any pair of states $|\phi\rangle, |\psi\rangle$ where you want to distinguish $|\phi\rangle = |\psi\rangle$ from $\langle\phi|\psi\rangle = 0$ — the same circuit gives probability 0 vs. $1/2$ respectively
- Related to the [[Swap Test for State Comparison|swap test]], but simpler when you have a specific operation connecting the two states

## Complexity

One Hadamard, one controlled group multiplication, one Hadamard, one measurement. Constant overhead per trial.

## Caveat

The success probability is only $1/2$ per trial (not close to 1), so you need $O(\log(1/\delta))$ parallel repetitions to reach error $\delta$. This means the quantum proof is a compound certificate of $O(\log(1/\delta))$ copies of $|H\rangle$.

Also, the trick assumes you already have $|H\rangle$ — preparing it is the hard part, handled by [[Uniform Superposition as Quantum Proof|using it as a quantum proof]] or by [[Subgroup State Preparation via Subnormal Chain|algorithmic preparation]] for solvable groups.

## Related notes
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]]
- [[Uniform Superposition as Quantum Proof]]
- [[Invariance Testing via Reversible Random Generation]]
- [[Swap Test for State Comparison]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]
