# Middle-Value Quantum Walk for Iterated-Cipher Dissection

> **Source:** Kaplan, arXiv:1410.1434
> **Tags:** #trick #quantum-cryptanalysis #quantum-walks #dissection #block-ciphers

## What it does

Quantises a dissection attack by walking over candidate middle values and using a quantum meet-in-the-middle routine as the checker.

## The trick

For a four-layer composition

$$
C=F_{k_4}(F_{k_3}(F_{k_2}(F_{k_1}(P))))
$$

introduce the middle value

$$
X=F_{k_2}(F_{k_1}(P)).
$$

For each candidate $X$, check two independent two-layer conditions:

$$
\exists k_1,k_2: F_{k_2}(F_{k_1}(P))=X,
$$

and

$$
\exists k_3,k_4: F_{k_4}(F_{k_3}(X))=C.
$$

Each check can use [[Quantum Meet-in-the-Middle as Claw Finding]], costing $O(N^{2/3})$. The outer search over $X$ is implemented as a quantum walk on the complete graph of middle values. With one marked $X$ among $M$ candidates, the marked fraction is $\varepsilon=1/M$ and the complete-graph gap is

$$
\delta=1-\frac{1}{M-1}.
$$

The MNRS walk-search cost is

$$
S+\frac{1}{\sqrt{\varepsilon}}\left(\frac{1}{\sqrt{\delta}}U+C\right).
$$

Here $S=O(1)$ and $U=O(1)$ because the walk only changes the middle-value register; the nontrivial cost is the checker $C=O(N^{2/3})$. Thus the total query/time cost is

$$
O(M^{1/2}N^{2/3}).
$$

When $M\asymp N$, this is $O(N^{7/6})$.

## When to reach for it

- Composite search problems where a candidate middle object splits the instance into two subproblems.
- Cryptanalytic dissection attacks where classical analysis searches over intermediate states and checks each state by meet-in-the-middle.
- Cases where the checker is itself a quantum subroutine and you need explicit time/memory accounting rather than only a query-composition theorem.

## Complexity

For four-encryption with $M\asymp N$:

$$
T=O(N^{7/6}),\qquad S=O(N^{2/3}),\qquad TS=O(N^{11/6}).
$$

More generally, with $M$ candidate middle values and a checker cost $C_{\rm check}$, the outer walk contributes roughly $O(\sqrt{M}\,C_{\rm check})$ when setup and update are cheap.

## Caveat

This is an upper-bound template. Kaplan does not prove the $4$-encryption attack is optimal. Standard adversary composition theorems do not apply directly because the predicates for different middle values share the same oracle input.

The method also assumes the middle value is unique or that the marked fraction is known well enough for the walk-search analysis.

## Related notes

- [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]]
- [[Quantum Meet-in-the-Middle as Claw Finding]]
- [[Coined Quantum Walk Search on Graphs]]
- [[Generalised Grover Lemma for Quantum Walks]]
