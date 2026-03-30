> **Source:** Andris Ambainis, Harry Buhrman, Peter Høyer, Marek Karpinski, Pierre Kurur, *Quantum matrix verification*, unpublished manuscript (2002). Algorithm described in Section 2.3 of Buhrman-Špalek, arXiv:quant-ph/0409035. See also Zoo entry #6.
> **Links:** [Buhrman-Špalek (improves this result)](https://arxiv.org/abs/quant-ph/0409035)
> **Tags:** #matrix-multiplication #verification #Grover #quantum-speedup #linear-algebra

---

## The computational problem

**Matrix product verification:** Given three $n \times n$ matrices $A$, $B$, $C$ over some ring, determine whether $AB = C$.

| Algorithm | Query/time complexity | Notes |
|---|---|---|
| Naive multiplication | $O(n^3)$ | Compute $AB$ directly |
| Strassen | $O(n^{2.376...})$ | Best known classical multiplication (Coppersmith-Winograd era) |
| Freivalds' algorithm | $O(n^2)$ randomized | Pick random $x$, check $A(Bx) = Cx$. Optimal classically. |
| **This paper (ABH+02)** | $O(n^{7/4})$ | Recursive Grover + Freivalds |
| Buhrman-Špalek (2006) | $O(n^{5/3})$ | Quantum walk improvement |

---

## What the paper does

Gives the first quantum algorithm for matrix product verification that beats the classical $\Theta(n^2)$ bound of Freivalds. The algorithm runs in $O(n^{7/4})$ time by combining Freivalds' randomized technique with Grover search in a recursive structure. While never formally published, it established quantum verification of matrix products as a problem and the $O(n^{7/4})$ bound stood until Buhrman and Špalek improved it to $O(n^{5/3})$ using [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|quantum walks]].

This is a clean early example of quantum speedups applied to a basic linear algebra problem — not deep, but it shows how quantum search composes with classical randomized techniques.

---

## The algorithm

The construction (as described by Buhrman-Špalek):

### Step 1: Block decomposition

Partition the columns of $B$ and $C$ into $\sqrt{n}$ blocks of $\sqrt{n}$ columns each: $B = [B_1 | B_2 | \cdots | B_{\sqrt{n}}]$ and $C = [C_1 | C_2 | \cdots | C_{\sqrt{n}}]$. Then $AB = C$ if and only if $AB_i = C_i$ for all $i$.

### Step 2: Randomized verification of each block

To verify $AB_i = C_i$ (an $n \times \sqrt{n}$ matrix equation):
1. Choose a random vector $x \in \{0,1\}^{\sqrt{n}}$
2. Compute $y = B_i x$ and $z = C_i x$ classically in $O(n^{3/2})$ time
3. This reduces to checking $Ay = z$ — a matrix-vector product verification

### Step 3: Grover search over rows

Checking $Ay = z$ means verifying $n$ inner products $\langle A_j, y \rangle = z_j$ for each row $j$. Each inner product costs $O(n)$. Use [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] over the $n$ rows to find a violation in $O(\sqrt{n})$ iterations, each costing $O(n)$, for total $O(n^{3/2})$ per block.

### Step 4: Amplitude amplification over blocks

The randomised test for a single block succeeds with constant probability. Apply [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] over the $\sqrt{n}$ blocks: $O(n^{1/4})$ iterations, each costing $O(n^{3/2})$, for total:

$$O(n^{1/4} \cdot n^{3/2}) = O(n^{7/4})$$

---

## Key results

$$\text{Quantum matrix verification:} \quad O(n^{7/4}) \text{ time (bounded error)}$$

This beats the classical $\Theta(n^2)$ of Freivalds' algorithm. The quantum lower bound for this problem is $\Omega(n)$ (you need to read at least a linear number of entries).

---

## Comparison with subsequent work

| Paper | Complexity | Technique |
|---|---|---|
| **ABH+02 (this paper)** | $O(n^{7/4})$ | Block Freivalds + Grover |
| Buhrman-Špalek (2006) | $O(n^{5/3})$ worst case | Quantum walk on Johnson graph pairs; [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes\|Szegedy walk]] |
| Le Gall (2012) | $O(n^{5/3})$ time, improved constants | Quantum walk + rectangular verification |

The improvement from $n^{7/4}$ to $n^{5/3}$ comes from replacing the block-Freivalds + amplitude amplification approach with a quantum walk that jointly searches over row and column subsets — avoiding the $O(n^2)$ bottleneck of loading submatrices.

---

## Limits / caveats

- The original manuscript was never published, so the algorithm is only known through citations and the description in Buhrman-Špalek. The core idea is straightforward enough that the reconstruction is reliable.
- The $O(n^{7/4})$ bound was quickly superseded. The historical value is in establishing the problem and showing quantum speedups for matrix verification are possible.
- The algorithm works in the query model (counting oracle queries to matrix entries). The time complexity matches when entry access is $O(1)$, but for structured matrices (sparse, Toeplitz, etc.) the picture may differ.
- No matching lower bound beyond $\Omega(n)$ is known. The gap between $n$ and $n^{5/3}$ remains open.

---

## Reusable ideas

1. [[Block Decomposition plus Amplitude Amplification for Matrix Problems]] — Partition a matrix verification into blocks, solve each block with a randomised subroutine, then amplify over blocks using quantum amplitude amplification. The randomisation converts a multiplicative problem ($AB = C$) into a linear one ($Ay = z$), and amplitude amplification searches over blocks quantumly. Applicable wherever Freivalds-style random projection reduces dimensionality.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — the search algorithm used for row-checking
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification used to search over blocks
- Freivalds (1979) — classical $O(n^2)$ randomised verification; the starting point
- Coppersmith-Winograd (1990) — best classical multiplication bound; verification is easier
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2004/2007)]] — the quantum walk technique that Buhrman-Špalek later applied to improve this result
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — the general walk framework used by Buhrman-Špalek

---

## Cross-links

### Paper notes
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]

### Trick cards
- [[Block Decomposition plus Amplitude Amplification for Matrix Problems]]
- [[Amplitude-Amplified State Preparation inside Walk Operators]]
