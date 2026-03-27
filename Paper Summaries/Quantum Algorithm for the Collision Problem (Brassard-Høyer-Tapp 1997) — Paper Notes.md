> **Source:** Gilles Brassard, Peter Høyer, and Alain Tapp, *Quantum Algorithm for the Collision Problem*, Proc. LATIN '98, LNCS **1380**, 163–169 (1998); arXiv:quant-ph/9705002
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9705002) · [Springer](https://doi.org/10.1007/BFb0054319)
> **Tags:** #collision #query-complexity #Grover #cryptography #birthday #foundational

---

## The problem

**Collision problem:** Given a black-box function $F: X \to Y$ that is $r$-to-one (every image has exactly $r$ preimages), find a collision — two distinct $x_0, x_1 \in X$ with $F(x_0) = F(x_1)$.

- **Classical:** $\Theta(\sqrt{N/r})$ evaluations (birthday bound), using $\Theta(\sqrt{N/r})$ space
- **Quantum (this paper):** $O(\sqrt[3]{N/r})$ evaluations, using $\Theta(\sqrt[3]{N/r})$ space

For $r = 2$ and $N$ domain elements: classical needs $\Theta(\sqrt{N})$ queries, quantum needs $O(\sqrt[3]{N})$.

---

## What the paper does

Combines a classical birthday-style table with [[Standard Amplitude Amplification|Grover search]] to get a cube-root query complexity for collision finding. The idea is beautifully simple: build a table of $k$ function evaluations classically, then use Grover to find an element outside the table that collides with something in the table.

This is the first quantum algorithm for collision finding, and it established the $O(N^{1/3})$ bound that was later shown to be optimal by Aaronson and Shi (2004).

---

## The algorithm

**Collision($F$, $k$):**

1. Pick an arbitrary subset $K \subseteq X$ of size $k$. Build a sorted table $L$ of pairs $(x, F(x))$ for $x \in K$.
2. Check $L$ for internal collisions. If found, output and halt.
3. Run $\text{Grover}(H, 1)$ where $H: X \to \{0,1\}$ is defined by $H(x) = 1$ iff $\exists\, x_0 \in K$ with $F(x) = F(x_0)$ and $x \neq x_0$.
4. Find the matching $x_0 \in K$ via binary search in $L$.
5. Output the collision $\{x_0, x_1\}$.

### Query count

- Step 1: $k$ evaluations of $F$ (classical)
- Step 3: Grover search over $N$ elements with $t = (r-1)k$ solutions → $O(\sqrt{N/((r-1)k)})$ evaluations
- Total: $O(k + \sqrt{N/(rk)})$

**Optimising over $k$:** Set $k = (N/r)^{1/3}$ to balance the two terms:

$$
k + \sqrt{N/(rk)} = O\left(\sqrt[3]{N/r}\right) + O\left(\sqrt[3]{N/r}\right) = O\left(\sqrt[3]{N/r}\right)
$$

Space: $\Theta(k) = \Theta(\sqrt[3]{N/r})$.

### The space-time tradeoff

For general $k$: space $S = \Theta(k)$ and queries $T = O(k + \sqrt{N/(rk)})$, giving:

$$
ST^2 \geq |F(X)| = N/r
$$

This is a continuous tradeoff:
- $S = 1$: $T = O(\sqrt{N/r})$ (pure Grover, no table)
- $S = (N/r)^{1/3}$: $T = O((N/r)^{1/3})$ (the sweet spot)
- $S = \sqrt{N/r}$: $T = O(\sqrt{N/r})$ (pure birthday, no Grover advantage)

---

## Extension: claw finding

**Claw problem:** Given $F: X \to Z$ and $G: Y \to Z$ (both one-to-one), find $(x_0, y_0)$ with $F(x_0) = G(y_0)$.

**Claw($F$, $G$, $k$):**
1. Build table $L$ of $k$ pairs $(x, F(x))$ for $x \in K \subseteq X$
2. Sort $L$
3. Run $\text{Grover}(H, 1)$ where $H(y) = 1$ iff $(x, G(y)) \in L$ for some $x$
4. Output the claw $(x_0, y_0)$

**Query count:** $k$ evaluations of $F$ + $O(\sqrt{N/k})$ evaluations of $G$. For $k = N^{1/3}$: total $O(N^{1/3})$.

For $r$-to-one functions: same $O(\sqrt[3]{N/r})$ with minor modifications (random $K$ selection to avoid table collisions).

---

## Comparison of collision-finding approaches

| Algorithm | Queries | Space | Setting |
|---|---|---|---|
| Classical birthday | $\Theta(\sqrt{N})$ | $\Theta(\sqrt{N})$ | Any 2-to-1 function |
| Grover (no table) | $O(\sqrt{N})$ | $O(1)$ | Search for preimage, but need target |
| **BHT (this paper)** | $O(N^{1/3})$ | $O(N^{1/3})$ | **Any $r$-to-1 function** |
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes\|Simon's algorithm]] | $O(n)$ = $O(\log N)$ | $O(n)$ | Only for $f(x) = f(x \oplus s)$ (structured) |
| Ambainis (element distinctness, 2007) | $O(N^{2/3})$ | $O(N^{2/3})$ | No $r$-to-1 promise |

BHT sits between Simon (exponential speedup but requires algebraic structure) and Grover (quadratic speedup, no structure needed). It exploits the $r$-to-1 promise for a cube-root improvement.

---

## Optimality

Aaronson (2002) and Aaronson-Shi (2004) proved $\Omega(N^{1/3})$ for the collision problem in the quantum setting, confirming BHT is optimal. The proof uses the polynomial method for quantum query complexity.

For the claw problem (without the $r$-to-1 promise): the tight bound is $\Theta(N^{2/3})$ via Ambainis's element distinctness algorithm using quantum walks. BHT achieves $N^{1/3}$ only because of the $r$-to-1 promise, which guarantees many collisions.

---

## Cryptographic implications

The paper is motivated by hash function security:
- Classical collision resistance: an $n$-bit hash function requires $\Theta(2^{n/2})$ evaluations to find a collision (birthday attack)
- Quantum collision resistance: BHT reduces this to $O(2^{n/3})$
- Consequence: to achieve $\lambda$-bit quantum security against collision attacks, hash functions need $3\lambda$-bit output (not $2\lambda$)

This directly impacts:
- Hash function security parameters
- Bit commitment scheme security (claw-finding breaks Brassard-Chaum-Crépeau 1988)
- Post-quantum cryptography parameter choices

---

## Reusable ideas

1. **[[Classical Preprocessing plus Grover Search]]:** Build a classical table of $k$ oracle evaluations, then use Grover to search for elements that interact with the table. Balancing $k$ against the Grover cost $\sqrt{N/k}$ gives cube-root complexity. A general template for combining classical and quantum computation.

2. **[[Birthday-Grover Space-Time Tradeoff]]:** For collision-type problems, there's a continuous tradeoff $ST^2 \geq N$ between space $S$ (table size) and queries $T$. At one extreme: pure birthday ($S = T = \sqrt{N}$). At the other: pure Grover ($S = 1$, $T = \sqrt{N}$). The sweet spot is $S = T = N^{1/3}$.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — quantum search algorithm used as subroutine
- Boyer, Brassard, Høyer & Tapp (1996) — tight bounds on quantum searching; generalisation to unknown $t$
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — solves the structured collision problem ($f(x) = f(x \oplus s)$) in $O(n)$ queries
- Brassard & Høyer (1997) — exact quantum polynomial-time algorithm for Simon's problem
- Rains (1997, talk at AT&T) — posed the question of quantum collision finding
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — element distinctness ($O(N^{2/3})$) without BHT's $r$-to-1 promise, via quantum walk on Johnson graph
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — element distinctness without $r$-to-1 promise; $O(N^{2/3})$ via quantum walk

---

## Cross-links

### Paper notes
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — generalises the Grover search used as subroutine
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — the structured version (exponentially better but requires algebraic structure)
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — quantum walk approach to element distinctness (the unstructured collision problem)

### Trick cards
- [[Classical Preprocessing plus Grover Search]]
- [[Birthday-Grover Space-Time Tradeoff]]
- [[Standard Amplitude Amplification]] — the Grover subroutine
- [[Superposition Query for Global Properties]]
