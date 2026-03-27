> **Source:** David Deutsch, *Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer*, Proc. R. Soc. Lond. A **400**, 97–117 (1985)
> **Links:** [Royal Society](https://doi.org/10.1098/rspa.1985.0070)
> **Tags:** #quantum-computation #Church-Turing #Deutsch-algorithm #universality #foundational

---

## What the paper does

Three things, each of which alone would be a landmark:

1. **Defines the quantum Turing machine** — the first formal model of quantum computation, establishing that quantum mechanics provides a well-defined computational framework analogous to the classical Turing machine.

2. **Proposes the quantum Church-Turing principle:** "Every finitely realisable physical system can be perfectly simulated by a universal model computing machine operating by finite means." Deutsch argues that classical Turing machines violate this principle (they can't efficiently simulate quantum systems), and that quantum Turing machines restore it.

3. **Gives the first quantum algorithm** — Deutsch's algorithm, which determines a global property of a function with fewer queries than any classical deterministic algorithm.

This is the founding document of quantum computation as a field of computer science.

---

## The quantum Church-Turing principle

Deutsch's argument:
- Turing's thesis: any function computable by a physical device is computable by a Turing machine
- The **physical** version (Church-Turing principle): any physical system can be *simulated* by a universal computing machine
- Classical Turing machines can simulate quantum systems, but with exponential overhead (the state space grows exponentially with the number of particles)
- Therefore: either the Church-Turing principle is wrong, or we need a computing machine based on quantum mechanics
- The quantum Turing machine (QTM) restores the principle

This is a profoundly physical argument for quantum computation — not "can we build one?" but "physics demands one."

---

## The quantum Turing machine

Deutsch defines the QTM by modifying the classical TM:

| Classical TM | Quantum TM |
|---|---|
| Tape cells hold symbols from finite alphabet $\Sigma$ | Tape cells hold quantum states in $\mathbb{C}^{|\Sigma|}$ |
| Transition function $\delta: Q \times \Sigma \to Q \times \Sigma \times \{L, R\}$ | Transition amplitudes: $U(q', s', d \mid q, s) \in \mathbb{C}$ |
| Configuration: head position + tape contents + state | Configuration: superposition over all classical configurations |
| Evolution: deterministic step | Evolution: unitary transformation on the full configuration space |

**Key requirements:**
- The transition must be unitary (preserving normalisation)
- Measurement collapses the superposition (only at the end, or explicitly)
- The machine can exist in a superposition of computational paths

Deutsch shows this is well-defined and at least as powerful as a classical TM (can simulate any classical computation).

---

## Deutsch's algorithm

The first quantum algorithm. Simple, but the idea it demonstrates — querying a function in superposition and using interference to extract global information — is the seed of everything that followed.

### The problem

Given a function $f: \{0, 1\} \to \{0, 1\}$, determine whether $f$ is **constant** ($f(0) = f(1)$) or **balanced** ($f(0) \neq f(1)$).

**Classical:** Need 2 queries (evaluate $f(0)$ and $f(1)$, compare).

**Quantum:** 1 query suffices.

### The circuit

1. Prepare $|0\rangle|1\rangle$
2. Apply $H \otimes H$: $\frac{1}{2}(|0\rangle + |1\rangle)(|0\rangle - |1\rangle)$
3. Apply $U_f: |x\rangle|y\rangle \to |x\rangle|y \oplus f(x)\rangle$
4. The state becomes $\frac{1}{2}\sum_x (-1)^{f(x)} |x\rangle (|0\rangle - |1\rangle)$

   Specifically: $\pm \frac{1}{2}(|0\rangle + (-1)^{f(0) \oplus f(1)}|1\rangle)(|0\rangle - |1\rangle)$

5. Apply $H$ to the first qubit
6. Measure: get $|0\rangle$ if $f(0) \oplus f(1) = 0$ (constant), $|1\rangle$ if $f(0) \oplus f(1) = 1$ (balanced)

### Why it works

The key move: the second register starts in $|0\rangle - |1\rangle$ (an eigenstate of the XOR operation). When $U_f$ acts:

$$
|x\rangle\frac{|0\rangle - |1\rangle}{\sqrt{2}} \xrightarrow{U_f} (-1)^{f(x)} |x\rangle\frac{|0\rangle - |1\rangle}{\sqrt{2}}
$$

The function value $f(x)$ gets kicked into the **phase** of the first register (the "phase kickback" trick). The second register is unchanged. Then Hadamard + measurement on the first register distinguishes the two cases.

**Note:** Deutsch's original 1985 version is slightly different and succeeds with probability 1/2 (requiring repetition). The clean deterministic version above (with the $|1\rangle$ ancilla trick) is from [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes|Deutsch and Jozsa (1992)]] and independently Cleve, Ekert, Macchiavello, and Mosca (1998). But the core idea — superposition query + phase extraction — is already in the 1985 paper.

---

## The phase kickback trick

The conceptual breakthrough that makes Deutsch's algorithm work, and that recurs throughout quantum algorithms:

If $U_f|x\rangle|y\rangle = |x\rangle|y \oplus f(x)\rangle$ and $|y\rangle$ is in the $(-1)$-eigenstate $\frac{|0\rangle - |1\rangle}{\sqrt{2}}$ of the XOR operation, then:

$$
U_f|x\rangle \cdot \frac{|0\rangle - |1\rangle}{\sqrt{2}} = (-1)^{f(x)}|x\rangle \cdot \frac{|0\rangle - |1\rangle}{\sqrt{2}}
$$

The function value moves from the target register into the phase of the control register. This converts a **bit oracle** ($|x\rangle|y\rangle \to |x\rangle|y \oplus f(x)\rangle$) into a **phase oracle** ($|x\rangle \to (-1)^{f(x)}|x\rangle$).

This is the trick behind Deutsch-Jozsa, Bernstein-Vazirani, [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon]], [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]], and [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Grover/amplitude amplification]].

---

## What this paper doesn't do

- Doesn't show an exponential speedup (that's [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon 1994]])
- Doesn't give a practical algorithm (that's [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor 1994]])
- Doesn't formally prove the QTM can simulate all quantum systems efficiently (that remains open — it's the quantum Church-Turing thesis)
- The original Deutsch algorithm succeeds probabilistically; the deterministic version came later

But it asks the right questions and provides the framework in which all subsequent work takes place. Without this paper, there's no field.

---

## Reusable ideas

1. **[[Phase Kickback from Eigenstate Ancilla]]:** Prepare the target register in an eigenstate of the oracle's action. The eigenvalue (here $(-1)^{f(x)}$) kicks back into the phase of the input register, converting a bit oracle into a phase oracle. The foundational trick of quantum query complexity.

2. **[[Superposition Query for Global Properties]]:** Evaluate a function on a superposition of inputs to extract global properties (constant vs balanced, periodicity, etc.) that would require multiple classical queries. The conceptual core of quantum speedups.

---

## References within this paper

- Turing (1936) — the classical Turing machine
- Church (1936) — Church's thesis on computability
- Feynman (1982) — suggestion that quantum physics may be hard to simulate classically, implying quantum computers might be more powerful
- Bennett (1973) — reversible computation
- Benioff (1980, 1982) — showed quantum mechanics can realise Turing-machine computation via reversible unitary evolution

---

## Cross-links

### Paper notes
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — first exponential oracle separation, building on the Fourier-sampling paradigm
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — exponential speedup for a concrete problem
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — [[Gapped Phase Estimation|phase estimation]] generalises the "extract eigenvalue into measurement" idea
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — the other major quantum speedup paradigm (search), also using phase kickback via $S_\chi$
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]] — makes the QTM's continuous gates compilable into finite gate sets

### Trick cards
- [[Phase Kickback from Eigenstate Ancilla]]
- [[Superposition Query for Global Properties]]
- [[Coset Sampling via Fourier Transform]] — the generalisation of superposition queries for structured problems
- [[Standard Amplitude Amplification]] — uses phase kickback via $S_\chi$
