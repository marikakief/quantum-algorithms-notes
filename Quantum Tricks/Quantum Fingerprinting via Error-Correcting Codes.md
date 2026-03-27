
> **Source:** Buhrman, Cleve, Watrous & de Wolf, arXiv:quant-ph/0102001 (2001)
> **Tags:** #trick #fingerprinting #communication-complexity #error-correcting-codes

## What it does

Maps $n$-bit strings to $O(\log n)$-qubit states such that equal strings produce identical states and distinct strings produce states with bounded inner product. Exponentially more compact than classical fingerprints without shared randomness.

## The construction

Given an error-correcting code $E: \{0,1\}^n \to \{0,1\}^m$ with $m = cn$ and minimum relative distance $(1-\delta)$:

$$
|h_x\rangle = \frac{1}{\sqrt{m}} \sum_{i=1}^{m} |i\rangle|E_i(x)\rangle
$$

**Properties:**
- $|h_x\rangle$ is a $(\lceil\log m\rceil + 1)$-qubit state = $O(\log n)$ qubits
- $x = y \implies |h_x\rangle = |h_y\rangle$
- $x \neq y \implies |\langle h_x|h_y\rangle| \leq \delta$

The inner product bound follows from the error-correcting code distance: distinct codewords agree on at most $\delta m$ positions.

## When to reach for it

- Equality testing in the SMP communication model (exponential quantum advantage)
- Any setting where you need compact representations of classical strings that preserve distinctness
- Quantum communication protocols where parties can't share randomness
- Building blocks for quantum streaming algorithms

## Complexity

$O(\log n)$ qubits per fingerprint. With Justesen codes: $\delta < 9/10 + 1/(15c)$ for code rate $1/c$.

Comparison: Classical fingerprints without shared randomness require $\Theta(\sqrt{n})$ bits.

## Caveat

The fingerprints are **quantum states** — they can't be copied (no-cloning). Each use of the fingerprint consumes the state. For $k$ comparisons, need $k$ copies = $O(k \log n)$ qubits total.

The comparison requires a quantum referee who can perform the [[Swap Test for State Comparison|swap test]]. A classical referee can't benefit from quantum fingerprints.

## Related notes

- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]]
- [[Swap Test for State Comparison]]
- [[Superposition Query for Global Properties]]
