# Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes

> **Source:** André Chailloux and Johanna Loyer, *Lattice Sieving via Quantum Random Walks*, arXiv:2105.05608, ASIACRYPT 2021
> **Links:** [arXiv](https://arxiv.org/abs/2105.05608) · [PDF](https://arxiv.org/pdf/2105.05608)
> **Tags:** #quantum-algorithms #lattices #shortest-vector-problem #quantum-walks #post-quantum-cryptography

---

## The computational problem

The target problem is heuristic exact SVP through sieving.

**Shortest Vector Problem (SVP).** Given a basis $B=(b_1,\ldots,b_d)$ for a lattice
$$
L(B)=\left\{\sum_{i=1}^d \lambda_i b_i : \lambda_i\in\mathbb Z\right\}\subseteq\mathbb R^m,
$$
find a non-zero lattice vector of minimum Euclidean norm.

The paper analyses one sieving step. Let
$$
N := (\sqrt{4/3})^d,
$$
and start from a list $L$ of $N'=N^{1+o(1)}$ lattice vectors normalized to the unit sphere. A sieve step outputs $N'$ lattice vectors of norm at most $\gamma<1$ by finding enough reducing pairs $(\vec x,\vec y)$ with
$$
\|\vec x\pm \vec y\|\leq \gamma.
$$
The asymptotic costs are heuristic in the usual lattice-sieving sense: list vectors and filter centres are treated as random points on high-dimensional spheres, and polynomial factors in $d$ are absorbed into $2^{o(d)}$.

## What the paper does

Chailloux--Loyer improve the best heuristic quantum exponent for lattice sieving from Laarhoven's $2^{0.2653d+o(d)}$ to
$$
2^{0.2570d+o(d)}.
$$
The move is not another direct Groverisation. They replace the Grover search inside a locality-sensitive-filter bucket by an MNRS/Johnson-graph [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|quantum walk]], and add a second, local layer of spherical filters inside each walk vertex so that checking for reducing pairs is cheaper.

My assessment: the exponent gain is small but real. The paper is mainly interesting as a template for turning a pair-search-in-buckets task into a quantum walk with extra stored local structure. The model assumptions are heavy: efficient QRAM/QRAQM and the standard heuristic random-sphere model for sieving.

Resource terminology in this note: **classical memory** stores the global sieve lists and filter data; **QRAM** means coherent read access to classical data structures; **QRAQM**/quantum-access quantum memory means coherent update/access to the walk's quantum data structures; **quantum memory** is the live coherent workspace maintained by the Johnson-graph walk.

## The algorithm / construction

### 1. LSF sieve framework

The paper rewrites the best LSF sieves in a common form. Pick a parameter $c\in(0,1)$ and choose a spherical-filter angle $\alpha\in[\pi/3,\pi/2]$ satisfying
$$
V_d(\alpha)=N^{-(1-c)},
$$
where $V_d(\alpha)=\operatorname{poly}(d)\sin^d(\alpha)$ is the spherical-cap volume fraction.

For each sieve attempt:

1. Sample a random product code $C$ of size $N^{1-c}$; codewords are the centres of $\alpha$-filters.
2. For every vector $\vec v\in L$, decode $\vec v$ against $C$ and insert it into the buckets for filters it survives.
3. For each filter bucket $f_\alpha(\vec s)$, call `FindAllSolutions` on its roughly $N^c$ vectors to find reducing pairs.
4. Add the resulting shorter vectors to the output list $L'$.
5. Repeat until $|L'|\geq N'$.

If $\vec x_0,\vec x_1$ lie in the same filter $f_\alpha(\vec s)$, the high-dimensional mass is concentrated near the cap boundary, so write
$$
\vec x_i=\cos(\alpha)\vec s+\sin(\alpha)\vec y_i,
$$
where $\vec y_i\perp \vec s$ and $\|\vec y_i\|=1$. A reducing pair in the original bucket is then equivalent to a close pair among residual vectors:
$$
\theta(\vec x_0,\vec x_1)\leq \pi/3
\quad\Longleftrightarrow\quad
\theta(\vec y_0,\vec y_1)\leq \theta^*_\alpha,
$$
with
$$
\theta^*_\alpha=2\arcsin\!\left(\frac{1}{2\sin\alpha}\right).
$$
If
$$
N^\zeta=N^{2c}V_d(\theta^*_\alpha),
$$
then each bucket has expected $N^\zeta$ reducing pairs.

### 2. Recovering prior algorithms in this framework

- **Classical BDGL-style LSF sieve.** Take $c\to 0$, so each filter bucket is tiny and `FindAllSolutions` costs $O(1)$ per filter. The exponent is
  $$
  N^{1.4094+o(1)}=2^{0.2925d+o(d)}.
  $$
- **Laarhoven quantum LSF sieve.** Take $c=0.2782$, the regime where $\zeta=0$, and use [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] over the $N^{2c}$ pairs in a bucket. This gives
  $$
  N^{1.2782+o(1)}=2^{0.2653d+o(d)}.
  $$

This note links that prior Groverised bottleneck to [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]] and [[Groverised Centre Search in Lattice Sieves]].

### 3. Quantum walk for `FindAllSolutions`

Inside one $\alpha$-bucket, let $L_y=\{\vec y_1,\ldots,\vec y_{N^c}\}$ be the residual vectors. The walk uses a Johnson graph
$$
J(N^c,N^{c_1})
$$
with parameters $c_1\leq c$ and $c_2\leq c_1$.

A vertex stores:

1. a subset $L_y^v\subset L_y$ of size $N^{c_1}$;
2. a second random product code $C_2$ in the residual sphere;
3. for every $\vec t\in C_2$, a local bucket
   $$
   J^v(\vec t)=f_\beta(\vec t)\cap L_y^v;
   $$
4. a marked bit indicating whether a local bucket contains a reducing residual pair.

The inner filter angle $\beta$ is chosen by
$$
V_d(\beta)=N^{c_2-c_1}.
$$
Let
$$
N^{\rho_0}=\frac{V_d(\beta)}{W_d(\beta,\theta^*_\alpha)},
$$
where $W_d(\beta,\theta)$ is the wedge volume fraction for two caps of angle $\beta$ around points separated by angle $\theta$.

The first version takes $|C_2|=N^{\rho_0+c_1-c_2}$, enough that a close residual pair in a vertex lands together in some inner filter with constant probability. A marked vertex is one where some truncated local bucket $\widetilde J^v(\vec t)$ contains a close residual pair. The truncation to the first $2N^{c_2}$ elements prevents a large bucket from blowing up the worst-case update cost.

For the Johnson graph, the spectral gap is
$$
\delta\approx N^{-c_1}.
$$
Using the MNRS cost formula, where $\varepsilon$ denotes the marked-vertex fraction and $\delta$ the spectral gap,
$$
S+\frac{1}{\sqrt\varepsilon}\left(\frac{1}{\sqrt\delta}U+C\right),
$$
the setup, update, and checking costs are approximately
$$
S=N^{c_1+\rho_0+o(1)},\qquad
U=N^{\max\{\rho_0,(\rho_0+c_2)/2\}+o(1)},\qquad
C=1.
$$
The first optimisation gives
$$
(c,c_1,c_2)\approx(0.3300,0.1952,0.0603),
$$
with total time
$$
N^{1.2555+o(1)}=2^{0.2605d+o(d)}.
$$
This already beats Laarhoven's $2^{0.2653d+o(d)}$, but not by the final amount.

### 4. Marked-vertex thinning

The improved construction introduces a free parameter $\rho\in(0,\rho_0]$ and uses a smaller inner code
$$
|C_2|=N^{\rho+c_1-c_2}.
$$
This intentionally marks only a fraction $N^{\rho-\rho_0}$ of the reducible pairs. The walk sees fewer marked vertices, but setup and update costs fall enough to win overall. In the valid regime
$$
\zeta+\rho-\rho_0>0,
$$
there are still enough marked pairs per bucket to recover $\Theta(N^\zeta)$ solutions after repeating over fresh $C_2$ codes.

The final optimum is
$$
\rho\to 0,
\qquad
c\approx0.3696,
\qquad
c_1\approx0.2384,
\qquad
c_2=0.
$$
Then
$$
\alpha\approx1.1514,
\qquad
\theta^*_\alpha\approx1.1586,
\qquad
\beta\approx1.1112,
\qquad
\zeta\approx0.1313,
\qquad
\rho_0\approx0.107.
$$
The walk has
$$
S=N^{0.2384+o(1)},\quad
U=N^{o(1)},\quad
C=1,
\quad
\varepsilon=\delta=N^{-0.2384+o(1)},
$$
and the total sieve cost balances as
$$
T=N^{c-\zeta}\left(N+N^{1-c+\zeta+c_1+\rho}\right)
  =N^{1.2384+o(1)}.
$$
Since $\log_2 N=(1/2)\log_2(4/3)d\approx0.2075d$, this is
$$
N^{1.2384+o(1)}=2^{0.2570d+o(d)}.
$$

## Key results

**Theorem 1 / Theorem 4.** Under the paper's sieving heuristics and in the QRAM model, there is a quantum random-walk SVP sieve in dimension $d$ with time
$$
2^{0.2570d+o(d)},
$$
QRAM of maximum size
$$
2^{0.0767d},
$$
quantum memory
$$
2^{0.0495d},
$$
and classical memory
$$
\operatorname{poly}(d)2^{0.2075d}.
$$

**Fixed quantum-memory tradeoff.** For $M\in[0,0.0495]$, the algorithm can run with quantum memory $2^{\mu_M d}$, QRAM $2^{\gamma_M d}$, and time $2^{\tau_M d+o(d)}$. The following are fitted exponent ranges quoted from the paper:
$$
\mu_M=M,
$$
$$
\tau_M\in 0.2653-0.1670M+[-2\cdot10^{-5},4\cdot10^{-5}],
$$
$$
\gamma_M\in 0.0578+0.3829M-[0,2\cdot10^{-4}].
$$
At $M=0$ this recovers Laarhoven's time exponent $0.2653$; at $M=0.0495$ it gives the main $0.2570$ exponent.

**Fixed QRAM tradeoff.** For $M'\in[0,0.0767]$, the algorithm can run with QRAM $2^{\gamma_{M'}d}$, quantum memory $2^{\mu_{M'}d}$, and time $2^{\tau_{M'}d+o(d)}$, where
$$
\gamma_{M'}=M',
$$
$$
\tau_{M'}\in0.2927-0.4647M'-[0,6\cdot10^{-4}],
$$
$$
\mu_{M'}\in\max\{2.6356(M'-0.0579),0\}+[0,9\cdot10^{-4}].
$$
For $M'=0$ this degenerates to the classical BDGL exponent $0.2925$; around $M'=0.0578$ it recovers Laarhoven's exponent; at $M'=0.0767$ it gives the main result.

**QRAM-cost sensitivity.** If a QRAM operation over $r$ registers is charged $r^{1/3}$, the paper minimises
$$
\lambda=\tau_{M'}+\frac13\max\{\gamma_{M'},\mu_{M'}\}.
$$
The previous value quoted from Laarhoven/AGPS is $\lambda=0.2849$; the new tradeoff gives $\lambda=0.2824$ at the main point.

## Comparison with prior work

| Work | Core idea | Heuristic time | Space / resources | Comment |
|---|---:|---:|---:|---|
| Nguyen--Vidick (2008) | Original heuristic lattice sieve | $2^{0.415d+o(d)}$ | $2^{0.2075d+o(d)}$ | First practical-style sieve family |
| Becker--Ducas--Gama--Laarhoven (2016) | LSF/NNS sieve | $2^{0.292d+o(d)}$ | $2^{0.208d+o(d)}$ | Classical baseline used here |
| [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes|Laarhoven / LMvdP]] | Grover over sieve bottlenecks | $2^{0.2653d+o(d)}$ | classical space $2^{0.2075d+o(d)}$ plus QRAM assumptions | Previous best heuristic quantum sieve exponent |
| Kirshanova--Mårtensson--Postlethwaite--Roy Moulik (2019) | Quantum $k$-list sieving | $2^{0.2989d+o(d)}$ | $2^{0.1395d+o(d)}$ | Worse time, better memory tradeoff |
| Chailloux--Loyer (this paper) | Johnson-graph quantum walk with local filters | $2^{0.2570d+o(d)}$ | QRAM $2^{0.0767d}$, quantum memory $2^{0.0495d}$, classical memory $2^{0.2075d}$ | Best asymptotic quantum heuristic exponent in this line |

The concrete security effect is modest. The authors estimate that a parameter set claiming 128 bits from the $0.2653d$ exponent drops to about 124 bits under $0.2570d$, before parameter retuning.

## Limits / caveats

- **Heuristic random-sphere model.** The analysis assumes sieve list vectors and code centres behave like random sphere points. This is standard in lattice-sieving exponent papers, but still not a theorem for real sieve trajectories.
- **QRAM/QRAQM model.** The walk needs quantum data structures supporting coherent insertion/deletion and access to classical lists. This is not just a query-model nicety; it is a major architectural assumption.
- **Small exponent gain.** The improvement from $0.2653$ to $0.2570$ matters for asymptotic cryptanalytic estimates, but it is not a practical attack recipe by itself.
- **Parallel depth is not improved beyond the classical parallel sieve.** The authors argue the outer filter calls parallelise well, but they do not know how to parallelise inside the quantum walk enough to beat depth $N^{0.409+o(1)}$ at width $N$.
- **The paper's formulas contain a few obvious min/max typos in the extracted text.** The intended estimates are clear from context and from the final theorems, but I would check the published version before quoting intermediate lemmas verbatim.

## Reusable ideas

1. [[Residual-Angle Reduction Inside Spherical Filters]] — reduce pair-finding inside a spherical cap to angle testing on residual vectors orthogonal to the filter centre.
2. [[Local-Filter Johnson Walk for Bucket Pair Finding]] — store a second layer of filters inside Johnson-walk vertices to lower checking/update cost for pair search.
3. [[Marked-Vertex Thinning in Quantum Walk Search]] — deliberately mark only a controlled fraction of solutions when the saved setup/update cost beats the loss in marked density.

## References within this paper

- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes|Laarhoven--Mosca--van de Pol (2015)]] — quantum search in SVP sieves; prior quantum baseline.
- Becker, Ducas, Gama, Laarhoven (2016) — LSF/NNS sieve giving classical $2^{0.292d+o(d)}$ time.
- Nguyen--Vidick (2008) — original practical heuristic sieve.
- Micciancio--Voulgaris (2010) — faster sieve variants and cap-volume estimates used in the background.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — quadratic search used in the prior quantum sieve and in the update subroutine.
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|Magniez--Nayak--Roland--Santha (2011)]] — quantum-walk search framework and cost formula.
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — Johnson-graph quantum walk and coherent data-structure precedent.
- Bernstein--Jeffery--Lange--Meurer (2013) — quantum subset-sum walks; cited for data structures and pair-search style walks.
- Albrecht--Gheorghiu--Postlethwaite--Schanck (2020) — practical estimates for quantum lattice-sieve speedups.

## Cross-links

### Paper notes

- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]

### Trick cards

- [[Residual-Angle Reduction Inside Spherical Filters]]
- [[Local-Filter Johnson Walk for Bucket Pair Finding]]
- [[Marked-Vertex Thinning in Quantum Walk Search]]
- [[Groverised Centre Search in Lattice Sieves]]
- [[QRACM Accounting for Quantum Search over Classical Lists]]
