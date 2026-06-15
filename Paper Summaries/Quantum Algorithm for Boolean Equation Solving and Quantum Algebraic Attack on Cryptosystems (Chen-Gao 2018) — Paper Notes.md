# Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes

> **Source:** Yu-Ao Chen and Xiao-Shan Gao, *Quantum Algorithms for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems*, arXiv:1712.06239 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1712.06239) · [PDF](https://arxiv.org/pdf/1712.06239)
> **Tags:** #quantum-algorithms #boolean-equation-solving #HHL #cryptanalysis #polynomial-systems

---

## The computational problem

The paper studies two closely related problems.

**Boolean equation solving over $\mathbb F_2$.**  Input a Boolean polynomial system
$$
F=\{f_1,\ldots,f_r\}\subset R_2[X],\qquad R_2[X]=\mathbb F_2[X]/(x_1^2-x_1,\ldots,x_n^2-x_n),
$$
with total sparseness
$$
T_F=\sum_{i=1}^r \# f_i.
$$
Output either one solution in $\mathbb F_2^n$ or report no solution. The main cost measure is stated in terms of sparse-matrix oracle access, $n$, $T_F$, the target failure probability $\epsilon$, and a condition number $\kappa$ inherited from Macaulay matrices. Turning those sparse-oracle calls into a full fault-tolerant circuit is a separate resource-estimation problem.

**Boolean solutions of complex polynomial systems.**  Input $F\subset \mathbb C[X]$ and find a point in
$$
V_B(F)=V_{\mathbb C}(F,x_1^2-x_1,\ldots,x_n^2-x_n).
$$
This is the intermediate problem because [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] works over $\mathbb C$, not over $\mathbb F_2$.

The cryptanalytic application is algebraic attack: encode AES, Trivium, Keccak, or MPKC key/preimage recovery as a sparse Boolean system, then run the solver. The resulting runtime is polynomial only when the relevant Macaulay condition numbers are small; this is not a practical break of AES or Keccak.

## What the paper does

Chen and Gao propose an HHL-based solver for Boolean systems: linearise the polynomial system by a modified Macaulay matrix, use HHL to prepare a monomial-solution state, then measure monomials to set variables to $1$ until a Boolean solution is found. The formal headline is
$$
\widetilde O\!\left((n^{3.5}+T_F^{3.5})\kappa^2\log(1/\epsilon)\right)
$$
for sparse Boolean systems.

My assessment: the construction is clever as a reduction from Boolean solving to structured QLSA calls, but the security interpretation is much shakier than the tables make it sound. Everything lives or dies on $\kappa$, and the paper gives no usable upper bounds for the cryptographic systems it lists. So this is best read as a condition-number-dependent attack framework, not evidence that AES/Keccak are close to broken by HHL.

## The algorithm / construction

### 1. Modified HHL for the Macaulay setting

For an $M\times N$ sparse matrix $A$, the paper uses the standard Hermitian embedding
$$
I(A)=\begin{pmatrix}0&A\\ A^\dagger&0\end{pmatrix}.
$$
The useful special case is when
$$
A=\sum_{j=1}^s A_j
$$
with each $A_j$ 1-sparse and queryable in time $O(\gamma)$, and when the right-hand side $b\in\{0,1\}^M$ has its nonzero entries in regularly spaced blocks. Then a padded version of $b$ can be prepared by Hadamards, and the HHL runtime becomes
$$
\widetilde O\!\left((\log(M+N)+\gamma)s\kappa^2/\epsilon\right).
$$
The row-padding point is slightly odd but useful: adding zero rows with inconsistent-looking entries in the right-hand-side preparation does not change the minimum-norm solution state returned by HHL, because $B^\dagger B=A^\dagger A$.

### 2. Build a modified Macaulay linear system

For $F=\{f_1,\ldots,f_r\}\subset\mathbb C[X]$, choose $D\ge \max_i \deg f_i$. Let $m_{\le d}$ be monomials whose exponent in each variable is at most $d$, ordered lexicographically. Multiplying each $f_i$ by monomials of bounded degree gives a linear system in monomial variables:
$$
M_{F,D}m_D=b_{F,D}.
$$
The modified construction pads rows and columns so dimensions are powers of two:
$$
M_{F,D}\in \mathbb C^{r(\bar d+1)^n\times ((\bar D+1)^n-1)}.
$$
The key structural facts are:

- $M_{F,D}$ has a natural 1-sparse decomposition, one matrix per term of each $f_i$.
- Its sparsity is $T_F=\sum_i \#f_i$.
- Query complexity for the 1-sparse pieces is $O(n\log D+\log r)$, using base-$(\bar d+1)$ and base-$(\bar D+1)$ index arithmetic.

### 3. Use complete solving degree, not just solving degree

A standard solving degree is enough to obtain a Gröbner basis from a Macaulay matrix, but not enough to express all monomials correctly in terms of residue monomials. The paper introduces the **complete solving degree** $\operatorname{CSdeg}(F)$: the smallest $D$ such that every monomial multiple $mg$ of a Gröbner-basis element with $\deg(mg)\le D$ has a representation
$$
mg=\sum_i h_i f_i,\qquad \deg(h_i f_i)\le D.
$$
For systems of the form
$$
F=\{g_1,\ldots,g_r\}\cup\{f_1,\ldots,f_n\},
$$
with leading monomials $\operatorname{lm}(f_i)=x_i^{d_i}$ and $\deg_{x_i}(g_j)<d_i$, they prove
$$
\operatorname{CSdeg}(F)\le d-2n+2\sum_{i=1}^n d_i,
$$
where $d=\max_j\deg(g_j)$.

For Boolean-solution systems $F_B\cup H_X$, where
$$
H_X=\{x_1^2-x_1,
\ldots,x_n^2-x_n\},
$$
the bound becomes
$$
\operatorname{CSdeg}(F_B\cup H_X)\le 3n.
$$

### 4. HHL prepares a pseudo-solution state

Assume $(F)$ is radical and zero-dimensional, with solution set
$$
V_{\mathbb C}(F)=\{a_1,
\ldots,a_w\}.
$$
For $D\ge \operatorname{CSdeg}(F)$, any Macaulay solution has the form
$$
\sum_{i=1}^w \eta_i m_D(a_i)+\text{arbitrary zero-column components},
\qquad \sum_i \eta_i=1.
$$
The minimum-norm state returned by HHL has the zero-column components set to zero, so it is a **pseudo-solution**
$$
\sum_{i=1}^w \eta_i \widetilde m_D(a_i).
$$
This is the main algebraic trick in the paper: the useless-looking HHL solution state is not decoded coordinate-by-coordinate; it is measured as a monomial state.

### 5. Recover a Boolean solution by monomial measurements

For $F\subset\mathbb C[X]$, first reduce powers using the Boolean equations to obtain $F_B$, then iterate:

1. Check the easy all-zero and all-one assignments.
2. Form
   $$
   F_2=F_B\cup H_Y
   $$
   on the currently unset variables $Y$.
3. Use modified HHL on the Macaulay system for $F_2$ with $D=3|Y|$.
4. Measure the monomial state. If the measured monomial is
   $$
   m=\prod_{i\in S}x_i,
   $$
   set every $x_i$ with $i\in S$ to $1$.
5. Substitute, delete zero polynomials, and repeat. If the run fails, restart.

The correctness lemma is probabilistic: if the approximate HHL state is within squared distance $<\epsilon_1/n$ of the ideal state, the chance of measuring a monomial whose ideal amplitude is zero is $<\epsilon_1/n$. Over at most $n$ inner iterations, one run fails with probability $<\epsilon_1$; $\lceil\log_{\epsilon_1}\epsilon\rceil$ restarts bring the failure probability below $\epsilon$.

### 6. Reduce Boolean equations over $\mathbb F_2$ to sparse complex equations

The naïve embedding of a parity equation into $\mathbb C$ is wrong: for example $x_1+x_2+1=0$ over $\mathbb F_2$ is not the same as $x_1+x_2+1=0$ over $\mathbb C$ with Boolean constraints.

For a Boolean polynomial $f=\sum_{i=1}^t m_i$, the paper first notes the exact but dense conversion
$$
C(f)=\prod_{k=f(0)}^{\lfloor t/2\rfloor}(f-2k),
$$
whose Boolean zeros match the $\mathbb F_2$ zeros. To keep the system sparse, split each equation into 3-sparse equations by adding auxiliary variables, then convert each 3-term equation by a special map $\widehat C$:
$$
\widehat C(f)=
\begin{cases}
f,& \#f=1,\\
n_1-n_2,& \#f=2,\ f=n_1+n_2,\\
f-2,& \#f=3,\ f(0)=1,\\
2m_1m_2+2m_1m_3+2m_2m_3-f,& \#f=3,\ f(0)=0.
\end{cases}
$$
The resulting complex system is 6-sparse, has at most $T_F$ equations after splitting, and uses at most $n+T_F$ variables.

## Key results

**Theorem 2.2 / modified HHL.**  Under the 1-sparse decomposition and structured-$b$ assumptions,
$$
\widetilde O\!\left((\log(M+N)+\gamma)s\kappa^2/\epsilon\right)
$$
time suffices to prepare an $\epsilon$-approximate solution state for $Ax=b$.

**Theorem 3.21 / pseudo-solution from Macaulay-HHL.**  Let $(F)$ be radical and zero-dimensional, $V_{\mathbb C}(F)=\{a_1,\ldots,a_w\}$, and $D\ge \operatorname{CSdeg}(F)$. Applying the modified HHL algorithm to
$$
M_{F,D}m_D=b_{F,D}
$$
returns an $\epsilon$-approximation to
$$
\sum_{i=1}^w \eta_i\widetilde m_D(a_i),
\qquad \sum_i\eta_i=1,
$$
with runtime
$$
\widetilde O\!\left(n\log(D)T_F\kappa^2/\epsilon\right).
$$

**Theorem 4.3 / Boolean solutions over $\mathbb C$.**  For $F\subset\mathbb C[X]$, Algorithm 4.2 finds a Boolean solution, if one exists, with probability at least $1-\epsilon$ and runtime
$$
\widetilde O\!\left(n^{2.5}(n+T_F)\kappa^2\log(1/\epsilon)\right).
$$
The explicit bound in Lemma 4.7 is
$$
\sqrt2 c\bigl(n\log_2(6n+1)+\log_2(r+1)\bigr)n^{1.5}(n+1+T_F)\kappa^2\lceil\log_2(1/\epsilon)\rceil.
$$

**Theorem 5.8 / Boolean systems over $\mathbb F_2$.**  For $F\subset R_2[X]$, Algorithm 5.7 returns a zero of $F$, or reports none, with probability $>1-\epsilon$ and runtime
$$
\widetilde O\!\left((n^{3.5}+T_F^{3.5})\kappa^2\log(1/\epsilon)\right).
$$
Here $\kappa$ is the maximum condition number of the Macaulay matrices generated during the complex Boolean-solution subroutine.

**Theorem 5.10 / explicit sparse Boolean bound.**  If $F=\bigcup_s\{f_{s1},\ldots,f_{sr_s}\}$ with $\#f_{sj}=s$, $T_F=\sum_s sr_s$, and $r=\sum_s r_s$, then a solution can be found in time at most
$$
\sqrt2 c(\log_2(n+T_F)+3)(n+T_F)^{2.5}(n+7T_F)\kappa^2\lceil\log_2(1/\epsilon)\rceil.
$$

**Cryptanalytic table values.**  For $\epsilon=1\%$, the paper lists attack complexities of the form $2^a c\kappa^2$:

| Target | Variables | Equations | Total sparseness | Claimed complexity |
|---|---:|---:|---:|---:|
| AES-128, 10 rounds | 4,288 | 10,616 | 252,288 | $2^{73.30}c\kappa^2$ |
| AES-192, 12 rounds | 7,488 | 18,096 | 421,248 | $2^{76.59}c\kappa^2$ |
| AES-256, 14 rounds | 11,904 | 29,520 | 696,384 | $2^{78.53}c\kappa^2$ |
| Trivium, 1152 rounds | 3,543 | 4,407 | 24,339 | $2^{61.71}c\kappa^2$ |
| Trivium, 2304 rounds | 6,999 | 9,015 | 49,683 | $2^{65.38}c\kappa^2$ |
| Keccak, $N_h=384$, 24 rounds | 76,800 | 77,160 | 611,023 | $2^{78.25}c\kappa^2$ |
| Keccak, $N_h=512$, 24 rounds | 76,800 | 77,288 | 611,540 | $2^{78.25}c\kappa^2$ |

## Comparison with prior work

| Approach | Problem model | Scaling stated in this paper | What changes here |
|---|---|---:|---|
| Gröbner/F4/XL Macaulay linearisation | Classical polynomial solving | exponential in hard Boolean cases | Chen-Gao use Macaulay matrices only to prepare a quantum monomial state, not to classically eliminate all variables. |
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] key search / Boolean MQ search | oracle search over assignments | quadratic speedup, e.g. AES key search circuits in [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes|Grassl et al.]] | This paper replaces search by QLSA on a structured algebraic system, gaining polynomial dependence on $n,T_F$ but paying $\kappa^2$. |
| Schwabe--Westerbaan and Faugère et al. quantum MQ algorithms | Boolean MQ | quoted as $O(2^{0.462n})$ under conditions | Chen-Gao target arbitrary sparse Boolean systems with condition-number dependence. |
| [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] | sparse linear systems over $\mathbb C$ | polylog dimension, polynomial in sparsity and $\kappa$ | The paper builds a nonlinear Boolean solver by mapping solutions into monomial coordinates and using measurement to avoid full readout. |

## Limits / caveats

1. **The condition number is the whole story.**  The paper explicitly says generic polynomial systems usually have exponential condition numbers. It does not estimate $\kappa$ for AES, Trivium, Keccak, or MPKC. The security conclusion should therefore be read as: these schemes resist this attack if their induced systems have large $\kappa$.

2. **The output is one satisfying assignment, not a classical description of the solution set.**  The algorithm avoids full HHL readout by measuring monomials and restarting. That is the right move, but it also means the speedup is tied to the special Boolean extraction logic.

3. **The Macaulay dimension is still enormous.**  It is hidden inside logarithms for HHL, but the oracle model has to support sparse queries to the padded Macaulay matrix. The paper gives an index-query construction; implementing it cleanly as a fault-tolerant oracle is a separate resource-estimation problem.

4. **The cryptanalytic tables suppress a large constant $c$.**  The paper defines $c$ as the complexity constant of the HHL algorithm. That is not a small implementation detail; it hides Hamiltonian simulation, phase estimation/linear-system machinery, and arithmetic.

5. **The result is not a proof that NP-hard problems are easy on quantum computers.**  The runtime is polynomial only when $\kappa$ is polynomial. The known HHL lower-bound discussion in the paper points the other way: improving the $\kappa$ dependence too much would have complexity-theoretic consequences.

6. **Later condition-number lower bounds are bad news for this exact construction.**  [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes|Ding--Gheorghiu--Gilyén--Hallgren--Li (2023)]] show that the max-degree-$3n$ Macaulay system used here has truncated QLS condition number at least $\Omega((3n)^{h/2})$ for a unique solution of Hamming weight $h$, so Grover search over weight-$h$ assignments is generically better.

## Reusable ideas

1. [[Macaulay-HHL Pseudo-Solution States]] — use the minimum-norm HHL solution of a Macaulay system to get a linear combination of monomial evaluations at algebraic solutions.
2. [[Boolean Solution Extraction by Monomial Measurement]] — recover a Boolean satisfying assignment by measuring a pseudo-solution monomial state and fixing variables appearing in the measured monomial.
3. [[Sparse Parity-to-Complex Polynomial Embedding]] — split Boolean equations into 3-sparse constraints, then map them to low-sparsity complex polynomial constraints with the same Boolean zeros.
4. [[Complete Solving Degree for Monomial Recovery]] — strengthen solving degree so Macaulay linear systems solve all bounded-degree monomials, not just enough for a Gröbner basis.

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim, Lloyd (2009)]] — the linear-system subroutine used throughout.
- Ambainis (2012) — improved condition-number dependence for QLSA; the paper notes an alternate runtime $\widetilde O((n^{4.5}+T_F^{4.5})\kappa\log(1/\epsilon))$ when using Ambainis-style linear-system solving.
- Childs, Kothari, Somma (2017) — improved precision dependence for quantum linear systems.
- Berry, Ahokas, Cleve, Sanders (2007) and Berry, Childs, Kothari (2015) — sparse Hamiltonian simulation ingredients for HHL cost estimates.
- Macaulay (1902), Lazard (1983), Faugère (1999), Courtois--Klimov--Patarin--Shamir (2000), Caminata--Gorla (2017) — Macaulay matrices, solving degree, F4/XL background.
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — baseline quantum search approach to algebraic attacks.
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes|Grassl, Langenberg, Roetteler, Steinwandt (2016)]] — Grover AES resource comparison.
- Schwabe--Westerbaan (2016), Faugère et al. (2017) — quantum algorithms for Boolean MQ cited as prior algebraic-attack approaches.
- Murphy--Robshaw (2002), Teo et al. (2014), Wu--Feng (2016) — algebraic encodings for AES, Trivium, and Keccak.
- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes|Chen--Gao--Yuan (2018)]] — sequel that reduces finite-field systems and bounded optimization problems to this B-POSSO solver.

## Cross-links

### Paper notes

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes]]
- [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]

### Trick cards

- [[Macaulay-HHL Pseudo-Solution States]]
- [[Boolean Solution Extraction by Monomial Measurement]]
- [[Sparse Parity-to-Complex Polynomial Embedding]]
- [[Complete Solving Degree for Monomial Recovery]]
- [[Truncated QLS Condition Number as a Right-Hand-Side No-Go Test]]
- [[Boolean Macaulay Reduction over C]]
