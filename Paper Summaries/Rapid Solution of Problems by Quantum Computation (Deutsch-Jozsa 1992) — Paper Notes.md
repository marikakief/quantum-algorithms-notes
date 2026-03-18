> **Source:** David Deutsch and Richard Jozsa, *Rapid Solution of Problems by Quantum Computation*, Proc. R. Soc. Lond. A **439**, 553–558 (1992)
> **Links:** [Royal Society](https://doi.org/10.1098/rspa.1992.0167)
> **Tags:** #oracle #quantum-algorithm #promise-problem #foundational

---

## The problem

Given a function $f: \{0,1\}^n \to \{0,1\}$ promised to be either **constant** (same value on all inputs) or **balanced** (1 on exactly half the inputs), determine which case holds. Access to $f$ is via a black-box oracle.

Classically (deterministically): you need to query $2^{n-1} + 1$ inputs in the worst case — just over half — before you can be certain. No shortcut exists.

## What the paper does

Gives a quantum algorithm that solves the Deutsch-Jozsa problem with **certainty** using a **single** query to the oracle (in the $n$-qubit version). This is an exponential separation between quantum and classical *deterministic* query complexity.

This was the first problem demonstrating that quantum computers can be exponentially faster than classical computers for *some* computational task. It generalises [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch's 1985 algorithm]] (the $n=1$ case) to $n$ bits.

**Important caveat:** A probabilistic classical algorithm can solve the problem with high confidence using $O(1)$ queries (just sample a few random inputs — if you ever see both 0 and 1, it's balanced; if not, it's almost certainly constant). So the separation is quantum vs. deterministic classical, not quantum vs. randomised classical. The first quantum-vs-randomised exponential separation came from [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]].

---

## The algorithm

### Oracle setup

The oracle implements $U_f |x\rangle |y\rangle = |x\rangle |y \oplus f(x)\rangle$. Using the [[Phase Kickback from Oracle Queries|phase kickback trick]]: prepare the target qubit in $|{-}\rangle = \frac{1}{\sqrt{2}}(|0\rangle - |1\rangle)$, then $U_f |x\rangle |{-}\rangle = (-1)^{f(x)} |x\rangle |{-}\rangle$. The oracle effectively applies a phase $(-1)^{f(x)}$.

### Circuit

$$|0\rangle^{\otimes n} \xrightarrow{H^{\otimes n}} \frac{1}{\sqrt{2^n}} \sum_{x \in \{0,1\}^n} |x\rangle \xrightarrow{U_f} \frac{1}{\sqrt{2^n}} \sum_{x} (-1)^{f(x)} |x\rangle \xrightarrow{H^{\otimes n}} |\psi\rangle$$

The final Hadamard transform gives:

$$|\psi\rangle = \sum_{z \in \{0,1\}^n} \left( \frac{1}{2^n} \sum_{x} (-1)^{f(x) + x \cdot z} \right) |z\rangle$$

The amplitude of $|0^n\rangle$ is:

$$\alpha_{0^n} = \frac{1}{2^n} \sum_{x} (-1)^{f(x)}$$

- **$f$ constant:** $\alpha_{0^n} = \pm 1$. Measuring gives $|0^n\rangle$ with certainty.
- **$f$ balanced:** $\alpha_{0^n} = 0$. Measuring *never* gives $|0^n\rangle$.

So: measure all qubits. If the result is $0^n$, output "constant"; otherwise, output "balanced." One query, zero error.

```
|0⟩^n ──[H^n]──[Oracle U_f]──[H^n]──[Measure]── 0^n → constant
                                                   else → balanced
|1⟩   ──[ H ]──────────────────────── (ancilla, discarded)
```

---

## Complexity

| Model | Queries needed |
|---|---|
| Classical deterministic | $2^{n-1} + 1$ |
| Classical randomised (bounded error) | $O(1)$ |
| Quantum (exact) | $1$ |

The quantum advantage over deterministic classical computation is exponential. Over randomised classical computation, the advantage is only constant (but the quantum algorithm gives a *certain* answer, not a probabilistic one).

---

## Why it matters

1. **First exponential quantum speedup** — even if only against deterministic classical algorithms, this was the proof of concept that quantum interference could be harnessed for computation.
2. **Template for oracle algorithms** — the Hadamard-oracle-Hadamard structure became the starting point for Bernstein-Vazirani, [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon's algorithm]], and eventually [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's algorithm]] (via the QFT generalisation).
3. **Introduced promise problems** to quantum complexity — the function is promised to be constant or balanced, not arbitrary. This "promise" structure reappears throughout quantum algorithms.

---

## Limits / caveats

- **Not useful in practice** — the promise (constant vs. balanced) is artificial, and randomised classical algorithms solve it efficiently anyway.
- **No quantum-vs-randomised separation.** That came two years later with Simon's problem.
- The paper's original presentation is slightly different from the modern "clean" version above — the modern version (using phase kickback on $|{-}\rangle$) is from Cleve, Ekert, Macchiavello & Mosca (1998).
- The algorithm is exact (zero error), which is stronger than bounded error, but this distinction matters less than the exponential query gap.

---

## Reusable ideas

1. [[Phase Kickback from Oracle Queries]] — prepare the target register in $|{-}\rangle$ so that $U_f$ applies a phase $(-1)^{f(x)}$ instead of flipping a bit. Converts a bit oracle into a phase oracle. Used in essentially every quantum oracle algorithm.
2. [[Hadamard Sandwich for Global Function Properties]] — apply $H^{\otimes n}$ before and after the oracle query. The initial Hadamard creates a uniform superposition; the oracle encodes function values as phases; the final Hadamard interferes them, concentrating amplitude on $|0^n\rangle$ iff the phases are uniform. Generalises to the QFT for periodic functions.

---

## References within this paper

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — the $n=1$ case (Deutsch's problem), quantum Turing machine, Church-Turing principle
- Feynman (1982) — quantum simulation conjecture (context)
- Bennett (1973) — reversible computation (relevant to oracle construction)

---

## Cross-links

### Paper notes
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]] — Deutsch's original $n=1$ algorithm; this paper is the direct generalisation
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]] — same circuit, different promise: learns hidden linear function in 1 query; first quantum-vs-randomised separation
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — first exponential quantum-vs-randomised separation; builds on the oracle framework established here
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — replaces the Hadamard with the QFT, turning the Deutsch-Jozsa template into a factoring algorithm
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — different oracle paradigm (amplitude amplification vs. interference)
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — references Deutsch-Jozsa as the first exponential oracle separation
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation generalises the Hadamard-sandwich structure

### Trick cards
- [[Phase Kickback from Oracle Queries]]
- [[Hadamard Sandwich for Global Function Properties]]
