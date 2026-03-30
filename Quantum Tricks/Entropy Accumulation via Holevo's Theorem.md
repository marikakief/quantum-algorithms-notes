# Entropy Accumulation via Holevo's Theorem

> **Source:** Nayak, arXiv:quant-ph/9904093 (1999)
> **Tags:** #trick #quantum-information #entropy #holevo-bound #lower-bounds

## What it does
Converts the distinguishability of quantum operations into a monotone entropy increase, providing dimension lower bounds for quantum systems that must process distinguishable inputs.

## The trick

Let $\sigma_0, \sigma_1$ be two density matrices and $\sigma = \frac{1}{2}(\sigma_0 + \sigma_1)$. If some measurement distinguishes them with average probability $p > 1/2$, then:

$$S(\sigma) \geq \frac{1}{2}[S(\sigma_0) + S(\sigma_1)] + (1 - H(p))$$

where $H$ is the binary entropy. This follows directly from Holevo's theorem and Fano's inequality.

**The ratchet:** Each time a quantum system of dimension $d$ processes one of two distinguishable operations, its von Neumann entropy increases by at least $1 - H(p)$. After processing $n$ such operations, the entropy is at least $(1 - H(p))n$. Since the entropy of a $d$-dimensional system is at most $\log d$, we get:

$$d \geq 2^{(1 - H(p))n}$$

The argument holds up well: unitary operations preserve entropy (so intermediate unitaries don't help), and measurements can only increase entropy (so intermediate measurements don't help either).

## When to reach for it

- Proving exponential lower bounds on the number of states in quantum automata or memory-bounded quantum processors
- Lower-bounding the dimension of quantum encodings (random access codes, communication protocols) where multiple pieces of classical information must be independently recoverable
- Any setting where a quantum system must "remember" $n$ independent distinguishable operations — the system size must be exponential in $n$

## Complexity
The bound is $d \geq 2^{(1-H(p))n}$, where $d$ is the system dimension, $n$ is the number of distinguishable operations, and $p$ is the distinguishing probability. For $p$ close to 1, $(1 - H(p)) \approx 1$.

## Caveat
The bound requires that the operations are distinguishable *for every possible prior state* of the system. If the distinguishing probability degrades with the current state, the entropy accumulation may be slower. The bound also cannot handle adaptive distinguishing probability — each step contributes $1 - H(p)$ only if $p$ is a worst-case guarantee.

## Related notes
- [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes]]
- [[Dimension Counting for Quantum Decoding]]
- [[Fat-Shattering Dimension of Quantum States via Random Access Codes]]
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]]
