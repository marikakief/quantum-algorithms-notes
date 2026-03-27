
> **Source:** Deutsch & Jozsa, Proc. R. Soc. Lond. A 439 (1992); modern form from Cleve, Ekert, Macchiavello & Mosca (1998)
> **Tags:** #trick #oracle #phase-kickback #foundational

## What it does
Converts a standard bit-flip oracle $U_f |x\rangle |y\rangle = |x\rangle |y \oplus f(x)\rangle$ into a phase oracle $|x\rangle \mapsto (-1)^{f(x)} |x\rangle$ by preparing the target register in the right state.

## The trick

Set the target qubit to $|{-}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$. Then:

$$U_f |x\rangle |{-}\rangle = (-1)^{f(x)} |x\rangle |{-}\rangle$$

The target qubit is unchanged (it's an eigenstate of the bit-flip with eigenvalue $(-1)^{f(x)}$), and the phase "kicks back" onto the input register. The ancilla factors out and can be ignored.

More generally, if the target register is in an eigenstate $|\lambda\rangle$ of some operation with eigenvalue $e^{i\theta}$, then controlling that operation kicks the phase $e^{i\theta}$ onto the control register. This is the mechanism behind [[Gapped Phase Estimation|phase estimation]], [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's eigenvalue measurement]], and every controlled-unitary circuit.

## When to reach for it
- Any time you have a bit oracle and need a phase oracle
- Explaining why controlled-$U$ puts eigenphases onto the control register in phase estimation
- Building oracle algorithms (Deutsch-Jozsa, Bernstein-Vazirani, [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon]], [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]])

## Complexity
Zero overhead — just prepare one ancilla qubit in $|{-}\rangle$ (a single $X$ gate followed by $H$).

## Caveat
Only works when $f(x) \in \{0, 1\}$. For functions with larger range, you need the general eigenvalue kickback version (prepare the target in an eigenstate of $U_f$). Also, the ancilla must remain unentangled with the input register — if $U_f$ does anything to the target beyond a phase, kickback doesn't apply cleanly.

## Related notes
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]]
- [[Hadamard Sandwich for Global Function Properties]]
- [[Gapped Phase Estimation]]
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]]
