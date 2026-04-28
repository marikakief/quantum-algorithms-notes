# Quantum Divide-and-Conquer Recurrence (sqrt-a Rule)

> **Source:** Childs, Kothari, Kovacs-Deak, Sundaram, Wang, arXiv:2210.06419 (TQC 2025)
> **Tags:** #trick #query-complexity #divide-and-conquer #adversary-method #span-programs #recurrence

## What it does

Converts a classical divide-and-conquer recurrence into a quantum one, replacing the number of subproblems $a$ with $\sqrt{a}$ in the recurrence, yielding a polynomial quantum speedup for problems with recursive structure.

## The trick

If a classical algorithm solves a problem of size $n$ by splitting into $a$ independent subproblems of size $n/b$ with auxiliary cost $C^{\text{aux}}(n)$, giving
$$C(n) \leq a \cdot C(n/b) + C^{\text{aux}}(n),$$
then the corresponding quantum span program satisfies
$$C_Q(n) \leq \sqrt{a} \cdot C_Q(n/b) + O\!\left(C^{\text{aux}}_Q(n)\right).$$

**How:** Build a span program for the full problem by combining span programs for each subproblem using the [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|span program composition rules]] (AND/OR of span programs). The adversary complexity (= span program complexity) composes as $\sqrt{a}$ rather than $a$ because adversary witnesses for independent subproblems combine in an $\ell^2$-norm sense, not an $\ell^1$-norm sense. Then convert the resulting span program to a quantum algorithm via the Reichardt/Špalek-Szegedy correspondence.

The $\sqrt{a}$ factor arises from the same reason Grover gives $\sqrt{N}$ speedup for OR of $N$ items: quantum parallelism over independent branches contributes amplitudes that add in quadrature.

## When to reach for it

- You have a classical algorithm based on divide and conquer where the $a$ recursive branches are **independent** (no shared input).
- The function computed is a Boolean combination of the subproblem outputs (or reducible to such).
- You want a quantum speedup but standard tools (Grover, amplitude amplification, quantum walk) don't apply naturally.
- The problem has automaton-like or string-structured recursion (e.g., recognising formal languages, string matching).

## Complexity

If the classical recurrence solves to $C(n) = n^\alpha$ (master theorem), the quantum recurrence solves to approximately $C_Q(n) = n^{\alpha'}$ where $\alpha' < \alpha$ reflects the $\sqrt{a}$ substitution. In the balanced case $a = b$, $C(n) = \Theta(n)$ classically, while $C_Q(n) = \Theta(\sqrt{n} \cdot \text{polylog}\, n)$ quantumly.

Concretely, for regular language recognition: $C(n) = \Theta(n)$ classically, $C_Q(n) = \tilde{O}(\sqrt{n})$ quantumly.

## Caveat

- Subproblems must be **independent** — correlated or overlapping subproblems break the composition.
- This is a **query complexity** result; gate complexity requires an efficient circuit implementation of the span program, adding possible polylogarithmic overhead.
- Auxiliary work $C^{\text{aux}}_Q(n)$ must also be efficient — if it dominates, no speedup is obtained.
- The technique does not apply when the recursion has **data-dependent branching** (adaptive split points), only fixed splits.

## Related notes

- [[Quantum Divide and Conquer (Childs-Kothari-Kovacs-Deak-Sundaram-Wang 2025) — Paper Notes]]
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]
- [[Adversary Method for Quantum Query Lower Bounds]]
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]
- [[Szegedy Walk for Formula Evaluation]]
- [[Recursive Quantum Walk for Nested Collision Problems]]
