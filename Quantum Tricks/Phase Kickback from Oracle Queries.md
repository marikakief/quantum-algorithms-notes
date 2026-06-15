
> **Source:** Implicit in Deutsch (1985); Deutsch & Jozsa, Proc. R. Soc. Lond. A 439 (1992); modern clean formulation from Cleve, Ekert, Macchiavello & Mosca (1998)
> **Tags:** #trick #oracle #phase-kickback #foundational

## What it does
Converts a standard bit-flip oracle $U_f |x\rangle |y\rangle = |x\rangle |y \oplus f(x)\rangle$ into a phase oracle $|x\rangle \mapsto (-1)^{f(x)} |x\rangle$ by preparing the target register in the right state.

## The trick

Set the target qubit to $|{-}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$. Then:

$$U_f |x\rangle |{-}\rangle = (-1)^{f(x)} |x\rangle |{-}\rangle$$

The target qubit is unchanged (it's an eigenstate of the bit-flip with eigenvalue $(-1)^{f(x)}$), and the phase "kicks back" onto the input register. The ancilla factors out and can be ignored.

More generally, if the target register is in an eigenstate $|\lambda\rangle$ of some operation with eigenvalue $e^{i\theta}$, then controlling that operation multiplies the corresponding control branch by $e^{i\theta}$. Phase estimation uses this eigenphase-kickback mechanism together with Fourier analysis; it is the controlled-unitary analogue of, not literally identical to, Boolean bit-oracle-to-phase-oracle conversion.

## When to reach for it
- Any time you have a bit oracle and need a phase oracle
- Explaining why controlled-$U$ on an eigenstate gives phase information to the control register in phase estimation
- Building oracle algorithms (Deutsch-Jozsa, Bernstein-Vazirani, [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon]], [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]])

## Complexity
No additional oracle calls. Circuit overhead is one ancilla qubit prepared in $|{-}\rangle$ (for example, a single $X$ gate followed by $H$).

## Caveat
This Boolean form only works when $f(x) \in \{0, 1\}$. For functions into a finite abelian group, use the qudit/Fourier-state generalization so addition by $f(x)$ contributes the corresponding character phase. More generally, prepare the target in an eigenstate of the operation being controlled. The ancilla must remain unentangled with the input register — if $U_f$ does anything to the target beyond a phase, kickback doesn't apply cleanly.

## Related notes
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]]
- [[Hadamard Sandwich for Global Function Properties]]
- [[Gapped Phase Estimation]]
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]]
