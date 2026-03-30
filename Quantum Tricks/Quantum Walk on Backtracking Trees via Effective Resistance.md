# Quantum Walk on Backtracking Trees via Effective Resistance

> **Source:** Montanaro, arXiv:1509.02374; building on Belovs, arXiv:1302.3143
> **Tags:** #trick #quantum-walk #backtracking #effective-resistance #search #CSP

## What it does

Detects or finds a marked vertex in a tree that is defined implicitly (not known in advance), starting the quantum walk from the root rather than from the stationary distribution.

## The trick

Build a coinless quantum walk on the tree using local diffusion operators $D_x$ that reflect about the uniform superposition of a vertex and its children. Partition vertices into even-depth ($A$) and odd-depth ($B$) sets; one walk step is $R_B R_A$ where $R_A = \bigoplus_{x \in A} D_x$ and $R_B = |r\rangle\langle r| + \bigoplus_{x \in B} D_x$. Marked vertices have $D_x = I$ (identity — the walk "sticks" there).

The root diffusion gets extra weight $\sqrt{n}$ on its children to ensure the eigenvalue-1 eigenvector has $\Omega(1)$ overlap with $|r\rangle$. When a marked vertex exists, this eigenvector is:

$$|\phi\rangle = \sqrt{n}|r\rangle + \sum_{x \neq r,\, x \rightsquigarrow x_0} (-1)^{\ell(x)} |x\rangle$$

with $|\langle r | \phi' \rangle| \geq 1/\sqrt{2}$. Detection reduces to distinguishing eigenvalue 1 from non-1 via [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]].

The no-marked-vertex case uses the effective spectral gap lemma to bound the weight of $|r\rangle$ on near-1 eigenvalues.

This is Belovs' quantum-walk–effective-resistance connection specialised to trees: the detection complexity scales with $\sqrt{R_{\text{eff}} \cdot T}$ where $R_{\text{eff}}$ is the effective resistance from root to marked set. For trees of depth $n$, this gives $O(\sqrt{Tn})$.

## When to reach for it

- Any search problem where the search space is defined as a tree explored by branching and pruning (backtracking)
- The tree isn't known in advance — it's defined implicitly by the problem structure
- You want to speed up a classical backtracking algorithm without changing its structure
- The tree could be very unbalanced (the walk handles this automatically)

The key departure from [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy walks]] is that you don't need the graph in advance and don't start from the stationary distribution. The departure from [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] is that you search within the backtracking tree rather than the full solution space.

## Complexity

$O(\sqrt{Tn} \log(1/\delta))$ evaluations of $P$ and $h$ for detection. $O(\sqrt{T} n^{3/2} \log n)$ for finding a solution.

## Caveat

The $\sqrt{n}$ depth overhead is necessary in the worst case ($\Omega(\sqrt{Tn})$ lower bound, Aaronson-Ambainis). Finding one of $k > 1$ marked vertices doesn't give a $1/\sqrt{k}$ improvement. If the classical algorithm is "lucky" and finds a solution early, the quantum algorithm may not beat it — it always explores the whole tree.

## Related notes
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]]
- [[Eigenvector-Encoded Path for Unique Search]]
