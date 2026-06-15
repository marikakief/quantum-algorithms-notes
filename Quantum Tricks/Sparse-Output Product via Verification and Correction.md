# Sparse-Output Product via Verification and Correction

> **Source:** Buhrman-Špalek, arXiv:quant-ph/0409035
> **Tags:** #trick #matrix-multiplication #verification #output-sensitive-algorithms

## What it does

Converts a fast product verifier into an output-sensitive algorithm by repeatedly finding and correcting wrong entries.

## The trick

Start with a guess $C$ for the product $AB$; in Buhrman-Špalek, the initial guess is $C=0$. Use a fast verifier not merely to decide whether $AB=C$, but to locate a wrong entry by binary search over quadrants:

1. Split $A$ into top/bottom row blocks, $B$ into left/right column blocks, and $C$ into four quadrants.
2. Run product verification on the four subproblems.
3. Recurse into a quadrant that fails verification.
4. At a $1\times 1$ block, recompute that entry exactly.

After finding one wrong position $(r,c)$, recompute that entry, then use Grover search to find and correct the remaining wrong entries in row $r$ and column $c$. Repeat until the verifier accepts.

The row/column clean-up matters: it ensures that each main iteration can be charged to progress through an independent set of wrong entries, rather than paying once per nonzero entry in a dense row or column.

## When to reach for it

Use it when:

- verification is asymptotically cheaper than full computation;
- the output is expected to be sparse or close to a known approximation;
- wrong entries can be found by recursively restricting the verifier to subproblems.

This is a general recipe: verifier $\to$ locator $\to$ correction loop.

## Complexity

For $A_{n\times m}B_{m\times n}$ with $m\ge n^{2/3}$ and $w$ nonzero output entries, Buhrman-Špalek obtain expected time, up to constants,

$$
\begin{cases}
 m\log n\cdot n^{2/3}w^{2/3}, & 1\le w\le \sqrt n,\\
 m\log n\cdot \sqrt n\,w, & \sqrt n\le w\le n,\\
 m\log n\cdot n\sqrt w, & n\le w\le n^2.
\end{cases}
$$

## Caveat

The algorithm is useful only when the output is sparse enough to offset repeated verification and correction. It also inherits the verifier's assumptions: efficient entry access, arithmetic over an integral domain for the main verifier, and bounded-error amplification for recursive location.

## Related notes

- [[Quantum Verification of Matrix Products (Buhrman-Špalek 2005) — Paper Notes]]
- [[Random Bilinear Compression for Submatrix Product Tests]]
- [[Johnson-Product Quantum Walk for Matrix Product Verification]]
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
