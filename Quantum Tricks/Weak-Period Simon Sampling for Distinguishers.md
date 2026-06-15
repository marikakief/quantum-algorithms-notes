# Weak-Period Simon Sampling for Distinguishers

> **Source:** Santoli--Schaffner, arXiv:1603.07856
> **Tags:** #trick #Simon #quantum-cryptanalysis #oracle-distinguishing

## What it does

Extracts linear equations orthogonal to a hidden XOR shift even when the oracle has extra collisions and does not satisfy the full Simon promise.

## The trick

For Simon’s algorithm, the usual promise is

$$
f(x)=f(y)\iff x\oplus y\in\{0^n,s\}.
$$

The Fourier-sampling interference that kills outcomes with $j\cdot s=1$ only needs the forward implication

$$
f(x)=f(x\oplus s)\qquad\text{for all }x.
$$

Indeed, after querying $f$ and grouping terms by an arbitrary value $y$ in the image of $f$, the preimage of $y$ can be written as unions of shift pairs

$$
\{a_1,a_1\oplus s,\ldots,a_r,a_r\oplus s\}
$$

possibly with more than one pair. The Hadamard amplitude for outcome $j$ contains the factor

$$
1+(-1)^{s\cdot j},
$$

so all outcomes with $s\cdot j=1$ vanish. Extra collisions change the distribution inside $s^\perp$; they do not produce samples outside it.

For a distinguisher, do not rely on this as a full recovery theorem. Collect equations, solve for a candidate shift only if the equations span enough, and verify the candidate directly by testing

$$
f(u)=f(u\oplus s)
$$

on fresh random $u$.

## When to reach for it

- You can prove a global hidden-shift invariance $f(x)=f(x\oplus s)$ but cannot prove that these are the only collisions.
- The task is distinguishing or certification rather than guaranteed hidden-shift recovery.
- A failed rank test is itself informative, as in [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes|Santoli--Schaffner’s]] Feistel distinguisher.

## Complexity

Each sample costs one coherent evaluation of $f$ plus Hadamards. Collecting $O(n)$ samples and solving over $\mathbb F_2$ is polynomial, but the sample distribution may be rank-deficient because of extra collisions. Add a direct period check before trusting the candidate.

## Caveat

This trick does not give the standard Simon guarantee. Extra collisions can bias all samples into a strict subspace of $s^\perp$, so recovery can fail. It is safe when the algorithm has a fallback branch or a verification step.

## Related notes

- [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Feistel Half-Output Period Test]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
