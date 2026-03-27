
> **Source:** Deutsch, Proc. R. Soc. Lond. A **400**, 97–117 (1985); made explicit in Cleve, Ekert, Macchiavello & Mosca (1998)
> **Tags:** #trick #phase-kickback #oracle #fundamental

## What it does

Converts a **bit oracle** $U_f: |x\rangle|y\rangle \to |x\rangle|y \oplus f(x)\rangle$ into a **phase oracle** $|x\rangle \to (-1)^{f(x)}|x\rangle$ by preparing the target register in the $(-1)$-eigenstate of the XOR operation.

## The trick

Set the target register to $|-\rangle = \frac{|0\rangle - |1\rangle}{\sqrt{2}}$. Then:

$$
U_f|x\rangle|-\rangle = (-1)^{f(x)}|x\rangle|-\rangle
$$

The function value $f(x)$ moves from the computational basis of the target into the **phase** of the control register. The target register is unchanged — it's an eigenstate.

**Proof:** $U_f|x\rangle|-\rangle = |x\rangle \frac{|f(x)\rangle - |1 \oplus f(x)\rangle}{\sqrt{2}} = (-1)^{f(x)}|x\rangle|-\rangle$.

### Generalisation to phase oracles

For controlled-$U$ with eigenstate $|u\rangle$ satisfying $U|u\rangle = e^{i\phi}|u\rangle$:

$$
\text{controlled-}U\;|x\rangle|u\rangle = e^{ix\phi}|x\rangle|u\rangle
$$

The eigenphase $\phi$ kicks back into the control register. This is the mechanism behind [[Gapped Phase Estimation|phase estimation]]: controlled-$U^{2^k}$ accumulates the phase $2^k\phi$ in the control qubit, which the inverse QFT then extracts.

## When to reach for it

- **Deutsch's algorithm:** $f: \{0,1\} \to \{0,1\}$, detect constant vs balanced
- **Deutsch-Jozsa:** $f: \{0,1\}^n \to \{0,1\}$, same question
- **Bernstein-Vazirani:** extract hidden string $s$ from $f(x) = s \cdot x$
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|**Simon's algorithm:**]] the Hadamard after oracle query works because the oracle effectively applies phases
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|**Grover/amplitude amplification:**]] $S_\chi$ is exactly a phase oracle for the characteristic function $\chi$
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|**Phase estimation:**]] controlled-$U$ with eigenstate in the target register

## Caveat

Requires the ability to prepare the eigenstate (for the $|-\rangle$ version, this is trivial). For phase estimation, the eigenstate may not be known — but the algorithm works on any state, decomposing it into eigenstates and measuring the corresponding phases.

The trick only works for **reversible** (unitary) oracles. Classical irreversible functions must first be embedded into reversible gates (via ancilla bits and [[Reversible Computation via Compute-Copy-Uncompute|Bennett's method]]).

## Related notes

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Gapped Phase Estimation]]
- [[Coset Sampling via Fourier Transform]]
- [[Superposition Query for Global Properties]]
