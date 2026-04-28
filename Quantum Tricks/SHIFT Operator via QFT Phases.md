# SHIFT Operator via QFT Phases

> **Source:** Raeisi, Kieferová, Mosca, arXiv:1902.04439
> **Tags:** #trick #QFT #arithmetic #permutations

## What it does
Implements a cyclic basis shift $|x\rangle \mapsto |x+m \bmod N\rangle$ by conjugating diagonal phase rotations with the [[Quantum Fourier Transform Circuit|QFT]].

## The trick
Use the identity that translation in the computational basis becomes a phase ramp in the Fourier basis. Concretely,

$$
\mathrm{SHIFT}_m = F_N^\dagger \left(\sum_{k=0}^{N-1} e^{-2\pi i km/N} |k\rangle\langle k|\right) F_N.
$$

So if you can do the [[Quantum Fourier Transform Circuit|QFT]] and simple $Z$-axis phase rotations, you get an exact cyclic shift. In [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]], this is used as the main structured part of the TSAC compression circuit.

## When to reach for it
- When you need modular translation rather than arithmetic addition with carries.
- When a permutation is easier to express spectrally than bitwise.
- When you already have a QFT-based arithmetic stack nearby.

## Complexity
Exact QFT-based implementations use $O(n^2)$ one- and two-qubit gates on $n$ qubits, ignoring approximation tradeoffs.

## Caveat
If you do not already want the QFT for some other reason, this can be a heavy way to implement a simple permutation.

## Related notes
- [[Novel Technique for Optimal and Robust Algorithmic Cooling (Raeisi-Kieferová-Mosca 2019) — Paper Notes]]
- [[Lexicographic Two-Sort Compression]]
- [[Quantum Fourier Transform Circuit]]
