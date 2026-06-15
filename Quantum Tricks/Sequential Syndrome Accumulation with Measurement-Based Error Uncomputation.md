# Sequential Syndrome Accumulation with Measurement-Based Error Uncomputation

> **Source:** Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, Jordan, arXiv:2510.10967
> **Tags:** #trick #DQI #syndrome-decoding #measurement-based-uncomputation #space-reduction

## What it does

Removes the $mb$-qubit DQI error register by generating one error symbol at a time, adding its syndrome contribution, and measuring it away.

## The trick

The original DQI circuit prepares an error vector $e \in \mathbb{F}_q^m$ and computes the syndrome $B^T e$. That needs an $mb$-qubit register for $e$.

This paper stores only sparse error locations and one reusable $b$-qubit error-value register:

1. Prepare sparse error locators with [[Sparse Dicke State Preparation by Combinatorial Unranking]].
2. For each constraint index $i$, test whether $i$ is in the locator list.
3. If so, generate the error value $e_i$ using the constraint-encoding gate $G_i$.
4. Add $B_i^T e_i$ into the syndrome register.
5. Measure the $e_i$ register in the $X$ basis and reuse it.

The $X$-basis measurement introduces a known phase $(-1)^{c_i\cdot e_i}$. Later, a reversible decoder reconstructs each $e_i$ from the syndrome and applies phase fixups. The decoder is now used for phase repair, not for clearing a live error register.

## When to reach for it

- A quantum algorithm accumulates a linear image of a sparse vector.
- The sparse vector is needed only to produce a syndrome or other linear summary.
- A decoder or inverse map can later reconstruct the sparse entries well enough to repair measurement phases.

## Complexity

Space drops from the original DQI-style $(m+n)b$ register cost to about $2nb+O(\log^2 n)$ once the decoder workspace is included. The scan costs $O(m\ell)$ locator-comparison work with [[Unary Iteration]] sharing, plus $m$ syndrome updates and one reversible decoder.

## Caveat

The measurement phases are not optional. If the later decoder cannot reconstruct the branch's error pattern, the phase fixup fails and the DQI output state is wrong. This trick is tied to decoding success just as tightly as [[Uncomputation via Syndrome Decoding]].

## Related notes

- [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]]
- [[Uncomputation via Syndrome Decoding]]
- [[Measurement-Based QROM Uncomputation]]
- [[Carry Venting via X-Basis Measurement]]
- [[Sparse Dicke State Preparation by Combinatorial Unranking]]
