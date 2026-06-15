# Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes

> **Source:** Andrew M. Childs and Gábor Ivanyos, *Quantum computation of discrete logarithms in semigroups*, arXiv:1310.6238 (2013)
> **Links:** [arXiv](https://arxiv.org/abs/1310.6238) · [PDF](https://arxiv.org/pdf/1310.6238)
> **Tags:** #quantum-algorithms #semigroups #discrete-log #hidden-subgroup #query-complexity

---

## The computational problem

The main problem is the **discrete logarithm problem in a finite semigroup**. A semigroup $S$ has an associative binary operation, but need not have inverses or an identity. Elements are black-box encoded uniquely, and multiplication in $S$ is efficient.

**Input:** $x,g\in S$, and an upper bound $N$ on the order of $g$, meaning $t+r\leq N$ for the index $t$ and period $r$ defined below.

**Output:** the smallest $a\in\mathbb N=\{1,2,\ldots\}$ such that
$$
g^a=x,
$$
or a decision that no such $a$ exists.

For a fixed $g$, the generated subsemigroup has a tail and then a cycle:
$$
g,g^2,\ldots,g^{t-1},g^t,g^{t+1},\ldots,g^{t+r-1},
$$
with $g^{t+r}=g^t$. Formally,
$$
t:=\min\{j\in\mathbb N: g^j=g^k\text{ for some }k\in j+\mathbb N\},
$$
$$
r:=\min\{j\in\mathbb N:g^t=g^{t+j}\}.
$$
The paper also studies two harder variants:

1. **Shifted semigroup discrete log:** given $x,y,g\in S$, find $a\in\mathbb N$ with $x=yg^a$.
2. **Constructive membership in a black-box abelian semigroup:** given $S=\langle g_1,\ldots,g_k\rangle$ and $x\in S$, find $a_1,\ldots,a_k\in\mathbb N_0$, not all zero, with
$$
x=g_1^{a_1}\cdots g_k^{a_k}.
$$

The complexity measures are polynomial time in $\log N$ for the main algorithm, and quantum query complexity in $|S|$ for the constructive-membership lower bound.

## What the paper does

Childs and Ivanyos show that ordinary discrete log in any finite semigroup is quantum-easy: the tail/cycle structure of $\langle g\rangle$ can be found by [[Order-Finding via QFT and Continued Fractions]], and the cyclic part is an actual group. So semigroup DLP is not a good post-quantum hardness assumption.

The more interesting part is the separation: shifted semigroup DLP reduces to a dihedral hidden-shift instance and is only known in subexponential time, while $k$-generator constructive membership in black-box abelian semigroups needs
$$
\Omega\left(|S|^{1/2-1/(2k)}\right)
$$
quantum queries. My assessment: the paper is short, but it cleanly draws the line between “lack of inverses is harmless” for one generator and “lack of inverses changes the problem” for shifted or multi-generator tasks.

These three statements should not be conflated. Ordinary one-generator semigroup DLP is polynomial-time quantum under the paper's black-box assumptions. Shifted semigroup DLP is reduced to a dihedral hidden-shift problem, giving the known subexponential upper bound but not a matching hardness theorem. Constructive membership is different again: the lower bound is for the black-box model, and explicit semigroups may have exploitable structure outside that oracle model.

## The algorithm / construction

### 1. Find the period $r$ of $g$

Prepare, for $M>N^2+N$,
$$
\frac{1}{\sqrt M}\sum_{j=1}^M |j\rangle |g^j\rangle .
$$
The powers $g^j$ are computed efficiently by repeated squaring, using associativity only.

Discard or measure the second register. With probability at least
$$
\frac{M-t+1}{M}\geq 1-\frac{N}{M},
$$
the observed semigroup element lies in the cycle rather than the tail. The first register is then an $r$-periodic state
$$
\frac{1}{\sqrt L}\sum_{j=0}^{L-1}|x_0+jr\rangle,
$$
where $x_0\in\{t,t+1,\ldots,t+r-1\}$ and $L$ is either $\lfloor(M-t)/r\rfloor$ or $\lceil(M-t)/r\rceil$.

Apply the QFT over $\mathbb Z_M$ and measure. The outcome $k\in\mathbb Z_M$ has probability
$$
\Pr(k)=\frac{\sin^2(\pi krL/M)}{LM\sin^2(\pi kr/M)}.
$$
As in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor period finding]], with probability at least $4/\pi^2$ the measurement gives a nearest integer to a multiple of $M/r$, and continued fractions recover $r$ with constant success probability.

This is [[Semigroup Index-Period Detection by Tail-Skipping Period Finding]].

### 2. Find the index $t$

Given $r$, test whether $g^j$ is in the cycle by computing
$$
\gamma(g^j)=
\begin{cases}
1 & \text{if }g^{j+r}=g^j,\\
0 & \text{otherwise.}
\end{cases}
$$
The sequence
$$
\gamma(g),\gamma(g^2),\ldots,\gamma(g^N)
$$
is $t-1$ zeros followed by $N-t+1$ ones, so $t$ is found by binary search in $O(\log N)$ iterations.

### 3. Compute $\log_g x$

First decide whether $x$ is in the cycle by testing $xg^r=x$.

**Tail case.** If $x$ is in the tail, find the smallest $p>0$ such that $xg^p$ is in the cycle, again by binary search over the monotone tail/cycle predicate. Then
$$
\log_g x=t-p.
$$

**Cycle case.** If $x$ is in the cycle, the set
$$
C=\{g^{t+j}:j\in\mathbb Z_r\}
$$
is a cyclic group. Let $s=-t\bmod r$. Then $g^{t+s}$ is the identity of $C$, and $g^{t+s+1}$ generates $C$; multiplication by $g^{t+s+1}$ advances the exponent by one inside the cycle. The inverse of this generator is known explicitly:
$$
(g^{t+s+1})^{-1}=g^{t+s+r-1}.
$$
Run Shor's discrete-log algorithm in $C$ using
$$
f(a,b)=x^a g^{(t+s+r-1)b}=x^a(g^{t+s+1})^{-b},
\qquad (a,b)\in\mathbb Z_r\times\mathbb Z_r.
$$
If this returns $\log_{g^{t+s+1}}x$, then
$$
\log_g x=t+\bigl(s+\log_{g^{t+s+1}}x\bmod r\bigr).
$$
This is [[Cyclic Group Extraction from a Semigroup Cycle]].

### 4. Shifted semigroup discrete log

For $x=yg^a$, define the tail/cycle structure of the map $j\mapsto yg^j$ analogously, with index $\tilde t$ and period $\tilde r$. These can be found by the same method as above.

If $x$ is in the tail, binary search works as before. If $x$ lies in the cycle, write
$$
x=yg^{\tilde t+\ell}.
$$
Then define
$$
f:\mathbb Z_2\ltimes\mathbb Z_{\tilde r}\to S
$$
by
$$
f(0,j)=yg^{\tilde t+j},\qquad f(1,j)=xg^j.
$$
Since
$$
f(1,j)=xg^j=yg^{\tilde t+\ell+j}=f(0,j+\ell),
$$
$f$ hides the order-two subgroup $\langle(1,\ell)\rangle$ of the dihedral group. [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg's sieve]] recovers $\ell$ in time $2^{O(\sqrt{\log \tilde r})}$, giving $a=\tilde t+\ell$. The query complexity can be only $\operatorname{poly}(\log |S|)$ by using the Ettinger--Høyer query-efficient dihedral-HSP procedure, but the known time remains subexponential.

This reduction is [[Shifted Semigroup Log as Dihedral Hidden Shift]].

### 5. Constructive membership

For $k\geq 2$, the paper proves a black-box lower bound by reducing inversion of a permutation on
$$
\Sigma=\{(a_1,\ldots,a_{k-1})\in\mathbb N_0^{k-1}:a_1+\cdots+a_{k-1}\leq n\}
$$
to constructive membership in a specially encoded abelian semigroup.

The semigroup is
$$
S=\{g_1^{a_1}\cdots g_k^{a_k}:a_i\in\mathbb N_0,
1\leq a_1+\cdots+a_k\leq n\}\cup\{0\},
$$
with multiplication sending products of total degree greater than $n$ to $0$. The encoding is normal below degree $n$, but the degree-$n$ layer is scrambled by a black-box permutation $\pi:\Sigma\to\Sigma$:
$$
\operatorname{enc}(g_1^{a_1}\cdots g_k^{a_k})=(a_1,\ldots,a_k)\quad\text{if }a_1+\cdots+a_k<n,
$$
$$
\operatorname{enc}(g_1^{a_1}\cdots g_{k-1}^{a_{k-1}}g_k^{n-a_1-\cdots-a_{k-1}})=\pi(a_1,
\ldots,a_{k-1}).
$$
Solving constructive membership for a degree-$n$ encoding $\sigma\in\Sigma$ recovers $\pi^{-1}(\sigma)$. Since quantum permutation inversion on $m$ points needs $\Omega(\sqrt m)$ queries, and
$$
|\Sigma|=\Theta(n^{k-1}),\qquad |S|=\Theta(n^k),
$$
constructive membership needs
$$
\Omega(\sqrt{n^{k-1}})=\Omega\left(|S|^{1/2-1/(2k)}\right)
$$
queries.

The matching upper bound uses the lemma that if $(a_1,\ldots,a_k)$ is the lexicographically first representation of $x$, then
$$
(a_1+1)\cdots(a_k+1)\leq |S|.
$$
So for some $j$,
$$
\prod_{i\neq j}(a_i+1)\leq |S|^{(k-1)/k}.
$$
Grover-search over the $k-1$ other exponents in that bounded region, and for each candidate use the shifted semigroup-log algorithm to solve for $a_j$. This gives
$$
|S|^{1/2-1/(2k)+o(1)}
$$
time and
$$
|S|^{1/2-1/(2k)}\operatorname{poly}(\log |S|)
$$
queries.

This is [[Constructive Semigroup Membership by One-Missing-Generator Search]].

## Key results

**Lemma 1.** Given a black-box semigroup $S$ and $g\in S$, there is an efficient quantum algorithm to determine the index $t$ and period $r$ of $g$.

**Theorem 1.** Given $x,g\in S$, there is an efficient quantum algorithm to compute $\log_g x$.

Equivalently, semigroup DLP has quantum running time $\operatorname{poly}(\log N)$ when powers and products in $S$ are efficient and $N$ bounds $t+r$.

**Lemma 2.** Given a black-box semigroup $S$ and $x,y,g\in S$, there is a quantum algorithm to find $a\in\mathbb N$ such that $x=yg^a$ in time
$$
2^{O(\sqrt{\log |S|})}.
$$
There is also an algorithm using only $\operatorname{poly}(\log |S|)$ quantum queries.

**Theorem 2.** Given a black-box semigroup $S=\langle g_1,\ldots,g_k\rangle$ and $x\in S$, any quantum algorithm for constructive membership of $x$ with respect to $g_1,\ldots,g_k$ requires
$$
\Omega\left(|S|^{1/2-1/(2k)}\right)
$$
quantum queries.

**Theorem 3.** For fixed $k$, there is a quantum algorithm for the same constructive-membership problem with running time
$$
|S|^{1/2-1/(2k)+o(1)},
$$
and a query-efficient version using
$$
|S|^{1/2-1/(2k)}\operatorname{poly}(\log |S|)
$$
quantum queries.

## Comparison with prior work

| Problem | Groups | Semigroups in this paper | Takeaway |
|---|---:|---:|---|
| Ordinary discrete log | Efficient by [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor]] | Efficient by finding tail/cycle, then running Shor on the cycle | Inverses are not needed for one-generator DLP. |
| Shifted discrete log $x=yg^a$ | Reduces to ordinary DLP via $g^a=y^{-1}x$ | Reduces to dihedral hidden shift; time $2^{O(\sqrt{\log |S|})}$ known | Lack of inverses matters. |
| Constructive membership with $k$ generators | Efficient by abelian HSP and linear Diophantine post-processing, cf. [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes|Ivanyos--Magniez--Santha]] | Needs $\Omega(|S|^{1/2-1/(2k)})$ quantum queries; matched up to subpolynomial factors | Multi-generator semigroup membership is genuinely hard in the black-box model. |
| Matrix-semigroup DLP attacks | Myasnikov--Ushakov reduce a matrix-group-ring case to finite-field DLP | General finite semigroups handled directly | The result removes dependence on matrix-specific structure. |
| [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes|Banin--Tsaban (2013)]] | Classical reduction from periodic semigroup DLP to cyclic-group DLP inside the eventual cycle | Periodic one-generator semigroups | Removes the need for a direct quantum tail/cycle period-finding argument if a group-DLP oracle is available. |
| [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes|Childs--Ivanyos (2013)]] | Quantum period finding to get the tail/cycle structure, then group discrete log in the cycle | Finite one-generator semigroups; shifted DLP; constructive membership | Stronger quantum complexity picture: ordinary semigroup DLP is easy, shifted and multi-generator variants are not automatically easy. |

## Limits / caveats

- The efficient DLP algorithm assumes unique encodings and efficient semigroup multiplication. It is a black-box algebraic result, not an implementation recipe for messy nonunique representations.
- The period-finding step needs an upper bound $N$ on $t+r$. The semigroup size is a trivial bound, but in explicit settings the cost depends on how $N$ is represented and how powers are computed.
- The shifted-DLP algorithm inherits the dihedral-HSP bottleneck. The paper does not prove shifted semigroup DLP is as hard as DHSP; it gives a reduction from the shifted problem to DHSP-style hidden shift, not an equivalence.
- The constructive-membership lower bound is black-box. It does not rule out faster algorithms for explicit semigroups with exploitable representation theory or geometry.
- For constructive membership, the near-matching upper bound uses shifted semigroup DLP as a subroutine, so its time bound includes Kuperberg-style subexponential overhead, hidden in the $o(1)$ exponent for fixed $k$.

## Reusable ideas

1. [[Semigroup Index-Period Detection by Tail-Skipping Period Finding]] — use a long exponent superposition so the cyclic part dominates the tail, then run ordinary period finding.
2. [[Cyclic Group Extraction from a Semigroup Cycle]] — once a cyclic semigroup enters its cycle, the cycle is a group with an explicit identity and generator.
3. [[Shifted Semigroup Log as Dihedral Hidden Shift]] — convert $x=yg^a$ inside a semigroup cycle into a dihedral hidden subgroup instance.
4. [[Constructive Semigroup Membership by One-Missing-Generator Search]] — use a volume bound on the lexicographically first representation, search all but one exponent, and solve the last by shifted log.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — period finding and discrete logarithms.
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] — subexponential solver for the shifted-log reduction.
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] — motivation for DHSP hardness through lattice reductions.
- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes|Ivanyos--Magniez--Santha (2001)]] — efficient constructive membership in black-box groups via HSP methods.
- Ambainis (2000/2002) — permutation-inversion lower bound used in Theorem 2.
- Ettinger--Høyer (1998/2000) — polynomial-query nonabelian-HSP method, used for the query-efficient shifted-DLP statement.
- Friedl--Ivanyos--Magniez--Santha--Sen (2003) — hidden translation and translating cosets; context for hidden-shift formulations.
- [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes|Banin--Tsaban (2013)]] — independent classical reduction of periodic semigroup DLP to cyclic-group DLP inside the eventual cycle.
- Beaudry (1988) — NP-completeness of membership testing in explicit abelian transformation semigroups.
- Maze--Monico--Rosenthal (2007) — semigroup action problem, discussed as a related cryptographic setting.

## Cross-links

### Paper notes

- [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) — Paper Notes]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
- [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]

### Trick cards

- [[Semigroup Index-Period Detection by Tail-Skipping Period Finding]]
- [[Cyclic Group Extraction from a Semigroup Cycle]]
- [[Shifted Semigroup Log as Dihedral Hidden Shift]]
- [[Constructive Semigroup Membership by One-Missing-Generator Search]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Coset Sampling via Fourier Transform]]
- [[Quantum Sieve for Labelled Qubits]]
