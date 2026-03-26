# Probability Oracle from Hadamard Test

> **Source:** Huggins, Wan, McClean, O'Brien, Wiebe, Babbush, arXiv:2111.09283 (construction); Kitaev (1995), Aharonov, Jones, Landau (2009) (Hadamard test)
> **Tags:** #trick #Hadamard-test #oracle-construction #measurement #amplitude-encoding

## What it does

Converts a parameterised overlap $\langle \psi | U(x) | \psi \rangle$ into a probability oracle — a unitary that encodes a function $f(x) \in [0,1]$ in the amplitude of an ancilla qubit — using one ancilla qubit and one call to the state preparation oracle $U_\psi$.

## The trick

Given a unitary $U(x)$ and a state $|\psi\rangle = U_\psi |0\rangle$, define:

$$f(x) = -\frac{1}{2}\operatorname{Im}\langle \psi | U(x) | \psi \rangle + \frac{1}{2}$$

The circuit:

$$F(x) = (H \otimes I) \cdot \text{c-}U(x) \cdot (S^\dagger H \otimes U_\psi)$$

acts on $|0\rangle_{\text{anc}} \otimes |0\rangle_{\text{sys}}$ to produce:

$$\sqrt{f(x)}\,|1\rangle|\phi_1\rangle + \sqrt{1-f(x)}\,|0\rangle|\phi_0\rangle$$

This is exactly the probability oracle definition required by Gilyen et al.'s gradient estimation framework.

The imaginary part comes from the $S^\dagger$ gate before the final Hadamard on the ancilla. (Replace $S^\dagger$ with $I$ to get the real part instead.)

To convert from a probability oracle to a phase oracle $|x\rangle \mapsto e^{if(x)}|x\rangle$, use $O(\log(1/\varepsilon))$ calls to the probability oracle and $O(\log\log(1/\varepsilon))$ additional ancillas (Theorem 14 of Gilyen et al.).

## When to reach for it

- Any time you need to feed a function of an overlap into a quantum algorithm that expects a probability or phase oracle.
- The [[Gradient Encoding for Multi-Observable Estimation]] trick uses this to build the probability oracle for the gradient algorithm.
- More broadly: whenever you want to do something with $\langle \psi | U | \psi \rangle$ beyond just measuring it — e.g., computing finite differences, phase estimation on a function of overlaps, etc.
- Also useful in constructing block encodings from overlap circuits.

## Complexity

- **Queries to $U_\psi$:** 1 per probability oracle call
- **Additional qubits:** 1 ancilla
- **Gates:** $O(1)$ single-qubit gates + controlled-$U(x)$
- **Probability $\to$ phase oracle conversion:** $O(\log(1/\varepsilon))$ calls, $O(\log\log(1/\varepsilon))$ ancillas

## Caveat

The function $f$ is bounded in $[0,1]$ by construction, but the useful information (the overlap) sits in the deviation from $1/2$. When the overlap is close to zero, $f \approx 1/2$ and the signal is weak — you're trying to distinguish $f$ from $1/2$ to precision $\varepsilon$, which is fine mathematically but means the probability oracle is operating in a low-signal regime.

Implementing controlled-$U(x)$ can be expensive if $U(x)$ involves many observables or long time evolution. The per-oracle cost is the cost of one controlled time evolution by all the observables.

## Related notes
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]]
- [[Gradient Encoding for Multi-Observable Estimation]]
- [[Analytical Gradient Estimation for Variational Quantum Circuits]] — Uses the same Hadamard test circuit but for a different purpose (variational parameter gradients)
- [[Iterative Phase Estimation (Kitaev)]] — Another use of the Hadamard test for extracting phase information
