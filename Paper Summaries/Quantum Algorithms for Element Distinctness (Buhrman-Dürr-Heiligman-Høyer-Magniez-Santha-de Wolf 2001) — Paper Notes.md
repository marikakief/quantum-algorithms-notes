> **Source:** Harry Buhrman, Christoph Dürr, Mark Heiligman, Peter Høyer, Frédéric Magniez, Miklos Santha, Ronald de Wolf, *Quantum algorithms for element distinctness*, SIAM J. Comput. **34**(6):1324–1330, 2005 (originally STOC 2001); arXiv:quant-ph/0007016
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0007016) · [SIAM](https://doi.org/10.1137/S0097539702402780)
> **Tags:** #element-distinctness #collision #claw-finding #amplitude-amplification #quantum-walk #query-complexity

---

## The computational problem

**Element distinctness (ED):** Given a function $f: [N] \to \mathbb{Z}$, determine whether there exist distinct $i, j$ with $f(i) = f(j)$.

**Claw-finding:** Given $f: [N] \to \mathbb{Z}$ and $g: [M] \to \mathbb{Z}$, find $(x, y)$ with $f(x) = g(y)$.

The complexity measure throughout is the number of **comparisons** — queries of the form "is $f(x) \leq f(y)$?" — not function evaluations. The results hold (up to log factors) in the function-evaluation model too.

---

## What the paper does

Gives an $O(N^{3/4} \log N)$ quantum algorithm for element distinctness, breaking the classical $\Theta(N \log N)$ barrier. The technique is nested [[Standard Amplitude Amplification|amplitude amplification]]: randomly sample subsets, sort to enable binary search, and amplify the probability of finding a collision. Also proves $\Omega(\sqrt{N})$ lower bounds.

This was the first quantum speedup for element distinctness. [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] later improved the bound to the optimal $O(N^{2/3})$ using [[Walk on the Johnson Graph for Subset Search|quantum walks on the Johnson graph]] — but the $N^{3/4}$ algorithm here is notable both historically and because its technique (classical sorting + Grover amplification) is simpler and uses less space ($O(\sqrt{N})$ vs $O(N^{2/3})$).

---

## The algorithm

### Generic claw-finder (parameterised by $\ell$)

For claw-finding between $f: [N] \to \mathbb{Z}$ and $g: [M] \to \mathbb{Z}$ (assuming $N \leq M$):

1. Select a random subset $A \subseteq [N]$ of size $\ell$
2. Select a random subset $B \subseteq [M]$ of size $\ell^2$
3. **Sort** $A$ by $f$-values — costs $O(\ell \log \ell)$ comparisons classically
4. For each $b \in B$, check if any $a \in A$ has $f(a) = g(b)$ via **binary search** on sorted $A$ — costs $O(\log \ell)$ per check. Apply [[Standard Amplitude Amplification|Grover search]] over $B$ to find a claw in $A \times B$: $O(\sqrt{|B|} \cdot \log \ell) = O(\ell \log \ell)$ comparisons
5. Apply [[Standard Amplitude Amplification|amplitude amplification]] on steps 1–4

**Cost analysis:** Steps 1–4 succeed with probability $\geq \ell^3 / (2NM)$ (the probability that the claw lands in $A \times B$, times $1/2$ for Grover). Amplitude amplification needs $O(\sqrt{NM/\ell^3})$ repetitions, each costing $O(\ell \log \ell)$. Total:

$$O\left(\sqrt{\frac{NM}{\ell}} \cdot \log \ell\right)$$

Maximise $\ell$ subject to $\ell \leq \min\{N, \sqrt{M}\}$.

### Element distinctness as self-claw-finding

Set $g = f$ and avoid trivial "claws" $(x, x)$. With $N = M$ and $\ell = \sqrt{N}$:

$$Q_2(\text{ED}) = O(N^{3/4} \log N)$$

### Recovering BHT for structured functions

If $f$ is 2-to-1 (the [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|BHT]] setting), set $\ell = N^{1/3}$. Then the success probability of steps 1–4 is $\Theta(1)$, and no outer amplification is needed: total $O(N^{1/3} \log N)$.

---

## Key results

**Theorem 3 (Element distinctness):**
$$\Omega(N^{1/2}) \leq Q_2(\text{ED}) \leq O(N^{3/4} \log N)$$

**Theorem 2 (Claw-finding, $N \leq M$):**
$$Q_2(\text{Claw}) = \begin{cases} O(N^{1/2} M^{1/4} \log N) & \text{if } N \leq M \leq N^2 \\ O(M^{1/2} \log N) & \text{if } M > N^2 \end{cases}$$

**Theorem 7 (Ordered claw-finding, both $f$ and $g$ monotone):**
$$Q_2 = O\left(\sqrt{N} \cdot c^{\log^* N}\right)$$
for a constant $c$. The $\log N$ factor in the unordered case is replaced by a near-constant via a recursive subdivision into subproblems of size $r = \lceil \log^2 N \rceil$, each solved recursively with amplitude amplification.

**Theorem 10 (Triangle finding):**
$$Q_2(\text{Triangle}) = O(n + \sqrt{nm})$$
where $n = |V|$, $m = |E|$. For sparse graphs ($m = O(n)$), this gives $O(n)$, a quadratic speedup.

---

## Comparison with prior and subsequent work

| Algorithm | ED bound | Technique | Space |
|---|---|---|---|
| Classical (sorting) | $\Theta(N \log N)$ | Sort + scan | $O(N)$ |
| [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes\|BHT (1997)]] | $O(N^{1/3})$ | [[Birthday-Grover Space-Time Tradeoff\|Birthday + Grover]] | $O(N^{1/3})$ |
| ↳ requires 2-to-1 promise | — | — | — |
| **This paper (2001)** | $O(N^{3/4} \log N)$ | Sort + Grover + amp. amplification | $O(\sqrt{N})$ |
| [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes\|Ambainis (2007)]] | $O(N^{2/3})$ | [[Walk on the Johnson Graph for Subset Search\|Johnson graph walk]] | $O(N^{2/3})$ |
| Aaronson-Shi lower bound | $\Omega(N^{2/3})$ | Polynomial method | — |

The gap between $N^{3/4}$ (this paper) and $N^{2/3}$ (Ambainis) is exactly the difference between classical random sampling and quantum walking on the search space. Ambainis's walk amortises the cost of moving between adjacent subsets to 1 query, while this paper pays $O(\ell \log \ell)$ to set up each subset from scratch.

Interestingly, Ambainis's paper recovers the $N^{3/4}$ bound of Buhrman et al. as the special case with $r = \sqrt{N}$ memory (see the [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis space-time tradeoff table]]).

---

## Lower bounds and hardness

The paper proves $\Omega(\sqrt{N})$ for bounded-error ED and $\Omega(N)$ for exact ED, both via reduction from OR. These are weaker than the $\Omega(N^{2/3})$ bound proved later by Aaronson and Shi (2004) using the polynomial method.

The paper also shows three related problems have $\Omega(N)$ quantum evaluation complexity (no speedup possible), proved via [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes|Ambainis's adversary method]]:
- **Parity-collision:** counting the parity of the number of collisions
- **No-collision:** finding an element not involved in any collision
- **No-range:** finding a value not in the range of $f$

These hardness results show that element distinctness's quantum speedup depends on the *search* nature of the problem — counting or complementary versions don't benefit.

---

## The ordered claw-finding recursion

For $f, g$ both monotone, the paper achieves a near-$\sqrt{N}$ bound via a recursive technique. Divide the domain into blocks of size $r = \lceil \log^2 N \rceil$. Each block pair (one from $f$, one from $g$) is a subproblem of size $r$. Use binary search ($O(\log N)$ comparisons) to align each $f$-block with the appropriate $g$-segment, then solve each subproblem recursively.

There are $O(N/r)$ subproblems, so [[Standard Amplitude Amplification|amplitude amplification]] over them costs $O(\sqrt{N/r})$, multiplied by the cost per subproblem. The recursion:

$$T(N) = O\left(\sqrt{\frac{N}{r}} \cdot [\log N + T(r)]\right)$$

With $r = \log^2 N$, the recursion depth is $O(\log^* N)$, yielding $T(N) = O(\sqrt{N} \cdot c^{\log^* N})$.

---

## Limits / caveats

- The $N^{3/4}$ bound was superseded by Ambainis's $N^{2/3}$ within a few years. The paper is historically important but algorithmically no longer the best approach.
- The log factors are real and somewhat annoying — they come from classical sorting and binary search. Ambainis's walk avoids them entirely.
- The lower bounds ($\Omega(\sqrt{N})$) are weak. The polynomial method hadn't yet been applied to element distinctness when this paper appeared.
- The triangle-finding algorithm ($O(n + \sqrt{nm})$) was later improved by [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes|Magniez-Santha-Szegedy (2007)]] to $\tilde{O}(n^{13/10})$ and eventually $\tilde{O}(n^{5/4})$ by Le Gall.

---

## Reusable ideas

1. **[[Nested Amplitude Amplification for Collision Search]]:** Sample a random subset, classically sort it, then use Grover over a second set to find a collision via binary search. Wrap the whole thing in an outer amplitude amplification. Gives $O(N^{3/4})$ for collision-finding without any structure on $f$. Simpler and lower-space than quantum walk approaches.

2. **[[Recursive Subproblem Amplification]]:** For problems with monotone structure, divide into $O(N/r)$ subproblems of size $r$, solve each recursively, and amplify over all subproblems. With $r = \log^2 N$, the recursion depth is $O(\log^* N)$, replacing $\log N$ factors with $c^{\log^* N}$ — essentially constant.

---

## References within this paper

- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard, Høyer & Tapp (1997)]] — $O(N^{1/3})$ collision finding for 2-to-1 functions
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2000)]] — amplitude amplification framework (the key tool)
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett, Bernstein, Brassard & Vazirani (1997)]] — $\Omega(\sqrt{N})$ lower bound for search
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — the underlying search primitive
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes|Ambainis (2000)]] — adversary method used for the hardness results
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1997)]] — context: the other main quantum algorithm family
- Aaronson & Shi (2004) — later proved the tight $\Omega(N^{2/3})$ lower bound

---

## Cross-links

### Paper notes
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — optimal $O(N^{2/3})$ successor
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]] — predecessor for structured collisions
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]] — triangle finding builds on element distinctness
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] — unified walk search framework
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — abstract walk search
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]] — survey covering both approaches

### Trick cards
- [[Nested Amplitude Amplification for Collision Search]]
- [[Recursive Subproblem Amplification]]
- [[Walk on the Johnson Graph for Subset Search]] — the technique that superseded this paper's approach
- [[Standard Amplitude Amplification]]
- [[Birthday-Grover Space-Time Tradeoff]]
- [[Classical Preprocessing plus Grover Search]]
