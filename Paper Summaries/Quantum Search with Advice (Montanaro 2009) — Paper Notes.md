# Quantum Search with Advice (Montanaro 2009) — Paper Notes

> **Source:** Ashley Montanaro, *Quantum search with advice*, Proceedings of TQC 2010; arXiv:0908.3066
> **Links:** [arXiv](https://arxiv.org/abs/0908.3066) · [PDF](https://arxiv.org/pdf/0908.3066)
> **Tags:** #quantum-search #query-complexity #average-case #amplitude-amplification #Grover #advice

---

## The computational problem

The paper studies unstructured search when the marked item is not uniformly distributed.

Input:

- an oracle $f : [n] \to \{0,1\}$ promised to have a unique marked item $x$, so $f(y)=\delta_{xy}$;
- an advice distribution $\mu=(p_y)_{y\in[n]}$, where $p_y$ is the prior probability that $y$ is marked.

Output: the marked item $x$.

The cost is **expected query complexity with respect to $\mu$**, not worst-case query complexity. The paper separates two models.

1. **Known advice model.** The distribution $\mu$ is known in advance and can be used when designing the algorithm. If probabilities are sorted in non-increasing order, the optimal classical strategy queries $1,2,\ldots,n$, with

$$
D(\mu)=\sum_{x=1}^n p_x x.
$$

2. **Unknown advice model.** The algorithm does not know $\mu$, but can query a state-preparation oracle

$$
O_\mu |0\rangle = |\mu\rangle := \sum_{x=1}^n \sqrt{p_x}|x\rangle
$$

and its inverse, each at unit cost. This is a strong input model; Montanaro notes it can be implemented when range sums $\sum_{x=a}^b p_x$ are computable efficiently, via the Grover-Rudolph state-preparation construction.

The success requirement is Las Vegas: the algorithm must output the marked item with certainty, but the number of queries may be random.

## What the paper does

Montanaro gives the right average-case quantum query complexity for search with known advice, up to constants:

$$
Q(\mu)=\Theta\!\left(\sum_{x=1}^n p_x\sqrt{x}\right)
$$

when the advice is sorted by decreasing probability. The upper bound is almost embarrassingly simple: search geometrically growing blocks with exact [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]]. The lower bound is just the optimality of search, applied to prefixes and then rearranged.

The second result is for unknown advice: if the algorithm can prepare $|\mu\rangle$, it can run random-iteration [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] from that advice state and then fall back to exact Grover search. This can still beat the best classical known-distribution strategy on some heavy-tailed priors.

My assessment: the paper is technically modest but clean. Its value is the model separation: once the performance metric is average-case rather than worst-case and weighted by the advice distribution, the prior becomes a real algorithmic resource, and the quantum/classical gap can be much larger than the usual worst-case quadratic search gap.

## The algorithm / construction

### Known advice: geometric quantum search

Assume $p_1\ge p_2\ge\cdots\ge p_n$. Algorithm 1 partitions the list into blocks whose sizes grow geometrically.

For a constant $k>1$:

1. Search the first block of size $1$.
2. Search the next block of size approximately $k$.
3. Search the next block of size approximately $k^2$.
4. Continue until the marked item is found.

Each block is searched using exact Grover search for the promise “zero or one marked item”. If $x$ lies in a block of size about $x$, the total cost of searching all earlier blocks is $O(\sqrt{x})$, because

$$
1 + \sqrt{k}+\sqrt{k^2}+\cdots+\sqrt{x}=O(\sqrt{x}).
$$

Averaging over $x\sim\mu$ gives the upper bound.

The reusable move is [[Geometric Grover Search over Advice-Ordered Blocks]]: sort by prior probability, then spend Grover effort according to rank rather than treating the whole domain uniformly.

### Unknown advice: amplify from the advice state, then fall back

In the unknown model, the algorithm has $O_\mu$ but not an explicit table of probabilities. It starts from

$$
|\mu\rangle=\sum_x \sqrt{p_x}|x\rangle.
$$

If the marked item has probability $p_x$, then amplitude amplification from $|\mu\rangle$ succeeds with probability

$$
\sin^2\!\left((2k+1)\arcsin\sqrt{p_x}\right)
$$

after $k$ iterations.

The algorithm does not know the right $k$. It uses the [[Randomised Iteration Count for Grover Search]] schedule:

1. For $j=0,1,\ldots,\lfloor \log_k \sqrt{n}\rfloor$:
   - sample once from $\mu$ and check $f$;
   - choose $i$ uniformly from $\{0,\ldots,\lfloor k^j\rfloor-1\}$;
   - run $i$ amplitude-amplification iterations from $|\mu\rangle$;
   - measure and verify with $f$.
2. If all attempts fail, run exact Grover search over $[n]$.

The final exact Grover step is important: it turns a bounded-success search process into a Las Vegas algorithm with finite worst-case fallback. See [[Advice-State Amplification with Exact Grover Fallback]].

## Key results

### Known advice upper bound

Proposition 2.1: choosing $k=e$, geometric quantum search has expected query cost at most

$$
\pi e \sum_{x=1}^n p_x\sqrt{x}.
$$

### Known advice lower bound

Proposition 2.4: for any distribution $\mu$,

$$
Q(\mu) \ge 0.206\sum_{x=1}^n p_x\sqrt{x}-1.
$$

The proof uses Zalka's optimality bound for Grover search. If a Las Vegas search algorithm had low expected cost on all items in a prefix, Markov's inequality would convert it into a bounded-error worst-case search algorithm beating the $\Omega(\sqrt{m})$ lower bound on a domain of size $m$. Rearranging these prefix lower bounds against sorted probabilities gives the average-case lower bound.

### Power-law priors in the known model

For $p_x\propto x^k$ with $k<0$,

$$
D(\mu)=
\begin{cases}
\Theta(n), & -1<k<0,\\
\Theta(n/\log n), & k=-1,\\
\Theta(n^{k+2}), & -2<k<-1,\\
\Theta(\log n), & k=-2,\\
\Theta(1), & k<-2,
\end{cases}
$$

while

$$
Q(\mu)=
\begin{cases}
\Theta(\sqrt n), & -1<k<0,\\
\Theta(\sqrt n/\log n), & k=-1,\\
\Theta(n^{k+3/2}), & -3/2<k<-1,\\
\Theta(\log n), & k=-3/2,\\
\Theta(1), & k<-3/2.
\end{cases}
$$

So for any $k\in(-2,-3/2)$, classical search has polynomial expected cost while quantum search has constant expected cost.

### Unknown advice bound

Proposition 3.2: with $k\approx 1.162$, the unknown-advice algorithm uses, on input $x$, expected query count at most

$$
\min\{83/\sqrt{p_x}+4/3,\; 53\sqrt n\}
$$

to each of $f$, $O_\mu$, and $O_\mu^{-1}$.

Corollary 3.3: there are constants $K,L,M$ such that

$$
T_A^*(\mu)
\le
K\sum_{x:p_x>1/n}\sqrt{p_x}
+L\sqrt n\sum_{x:p_x\le 1/n}p_x
+M.
$$

### Power-law priors in the unknown model

For $p_x\propto x^k$, $k<0$, Algorithm 3 has

$$
T_A^*(\mu)=
\begin{cases}
O(\sqrt n), & -1\le k<0,\\
O(n^{-(1/2+1/k)}), & -2<k<-1,\\
O(\log n), & k=-2,\\
O(1), & k<-2.
\end{cases}
$$

This can beat even the best classical algorithm that knows $\mu$.

## Comparison with prior work

| Work | Model | Main role here |
|---|---|---|
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] | worst-case unstructured search | exact search subroutine for each block and final fallback |
| [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] | general amplitude amplification | unknown-advice algorithm starts from $|\mu\rangle$ |
| [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp (1998)]] | unknown number of marked items | randomised iteration-count analysis reused in Algorithm 3 |
| Zalka (1999) | optimality of Grover search | lower bound for known advice |
| Grover-Rudolph (2002) | distribution-state preparation | justifies $O_\mu$ when interval sums are efficient |
| Ambainis-de Wolf (2001) | average-case quantum query complexity | broader average-case setting where exponential separations are possible |

## Limits / caveats

- The strong separation is average-case. Worst-case total-function barriers do not vanish; the paper sidesteps them by measuring expected queries under $\mu$.
- The known-advice algorithm needs the ability to order items by decreasing $p_x$. It does not need exact probabilities once that order is known.
- The unknown-advice model assumes coherent access to $O_\mu$ and $O_\mu^{-1}$. That is a real modelling cost, not a free classical sample oracle.
- The constants in the unknown-advice bound are not pretty: $83/\sqrt{p_x}+4/3$ and $53\sqrt n$. The asymptotic behaviour is the point.
- This is a query-complexity result. It does not account for the gate cost of sorting advice, preparing $|\mu\rangle$, or implementing exact Grover variants.

## Reusable ideas

1. [[Geometric Grover Search over Advice-Ordered Blocks]] — rank items by prior probability and search geometrically growing prefixes/blocks so item $x$ costs $O(\sqrt{x})$ rather than $O(\sqrt n)$.
2. [[Advice-State Amplification with Exact Grover Fallback]] — when an advice state gives the marked item amplitude $\sqrt{p_x}$ but $p_x$ is unknown, use randomised amplification schedules and then exact Grover search to get Las Vegas behaviour.
3. [[Average-Case Search Lower Bound by Prefix Rearrangement]] — convert worst-case Grover lower bounds on prefixes into an average-case lower bound weighted by sorted advice probabilities.

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — exact search subroutine.
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification framework.
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp (1998)]] — randomised Grover iteration counts and tight search bounds.
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph (2002)]] — efficient preparation of $|\mu\rangle$ from interval-sum access.
- Zalka (1999) — exact optimality of Grover search.
- Ambainis-de Wolf (2001) — average-case quantum query complexity separations.
- Press (2009) — motivating discussion of why direct sampling from a heavy-tailed distribution can be suboptimal.

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]]
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]

### Trick cards

- [[Geometric Grover Search over Advice-Ordered Blocks]]
- [[Advice-State Amplification with Exact Grover Fallback]]
- [[Average-Case Search Lower Bound by Prefix Rearrangement]]
- [[Randomised Iteration Count for Grover Search]]
- [[Classical Preprocessing plus Grover Search]]
