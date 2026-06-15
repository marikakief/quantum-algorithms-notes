# Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis (Chen-Gao-Yuan 2018) — Paper Notes

> **Source:** Yu-Ao Chen, Xiao-Shan Gao, and Chun-Ming Yuan, *Quantum Algorithm for Optimization and Polynomial System Solving over Finite Field and Application to Cryptanalysis*, arXiv:1802.03856 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1802.03856) · [PDF](https://arxiv.org/pdf/1802.03856)
> **Tags:** #quantum-algorithms #polynomial-systems #finite-fields #HHL #cryptanalysis #lattice-problems

---

## The computational problem

The paper studies two problems, both reduced to Boolean polynomial solving over $\mathbb C$ and then handled using the B-POSSO algorithm from [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]].

**Polynomial system solving over a finite field.**  Input
$$
F=\{f_1,\ldots,f_r\}\subset \mathbb F_q[x_1,\ldots,x_n],\qquad q=p^m,
$$
with total sparseness
$$
T_F=\sum_{i=1}^r \# f_i.
$$
Output either a point in $V_{\mathbb F_q}(F)$ or report no solution. The cost is gate complexity in terms of $T_F$, the degree parameter
$$
D=n+\sum_{i=1}^n \left\lfloor \log_2\max_j\deg_{x_i} f_j\right\rfloor,
$$
$m$, $p$, error probability $\epsilon$, and a condition number $\kappa$ of the Boolean-complex system constructed from $F$.

**Finite-field / bounded-integer optimization.**  Given
$$
\begin{aligned}
\min_{X\in\mathbb F_p^n,\,Y\in\mathbb Z^m}\quad &o(X,Y)\\
\text{subject to}\quad & f_j(X)=0\pmod p,
\quad j=1,
\ldots,r,\\
&0\le g_i(X,Y)\le b_i,
\quad i=1,
\ldots,s,\\
&0\le y_k\le u_k,
\quad k=1,
\ldots,m,
\end{aligned}
$$
find an optimum, again with runtime polynomial in explicit input parameters and in a condition number $\kappa$ of the derived feasibility systems.

The cryptographic applications are PSWN/MAX-POSSO, SIS, SVP/CVP, and NTRU key recovery.

## What the paper does

This is the sequel to [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]. It supplies classical reductions from finite-field solving and bounded-integer optimization to B-POSSO, then imports the HHL/Macaulay Boolean solver.

My assessment: technically, the reductions are useful to have written down. The security claims are much weaker than the abstract makes them sound. Every speedup is conditional on the induced Macaulay/Boolean systems having small $\kappa$, and the paper does not bound $\kappa$ for the cryptographic instances. So this is a condition-number-dependent attack template, not a practical break of lattice cryptography or NTRU.

## The algorithm / construction

### 1. Encode bounded integer intervals by $\theta_b$

For $b>0$, set $s=\lfloor\log_2 b\rfloor$ and introduce Boolean variables $B_0,\ldots,B_s$. Define
$$
\theta_b(B)=\sum_{i=0}^{s-1}2^i B_i+(b+1-2^s)B_s.
$$
Lemma 2.1 proves that, over $\mathbb C$ or over $\mathbb F_p$ when $p>b$, this map is surjective from $\{0,1\}^{s+1}$ onto $\{0,1,\ldots,b\}$. Unlike ordinary binary encoding with $s+1$ bits, it does not generate illegal values above $b$.

This encoding is used for:

- finite-field variables $x_i\in\mathbb F_p$ via $x_i=\theta_{p-1}(X_i)$;
- bounded integer variables $0\le y_i\le u_i$ via $y_i=\theta_{u_i}(Y_i)$;
- slack variables for inequalities $0\le g\le b$ via $g=\theta_b(G)$;
- interval search for the objective value.

### 2. Reduce high-degree polynomial systems to MQ

Given $F\subset K[X]$, Lemma 2.3 gives an explicit quadratisation $Q(F)\subset K[X,V]$ by introducing variables for powers $x_i^{2^j}$ and for product chains. It preserves the elimination ideal:
$$
(F)_{K[X]}=(Q(F))_{K[X,V]}\cap K[X].
$$
The size bounds are
$$
\#V=(T_F+1)\sum_i\lfloor\log_2 d_i\rfloor+nT_F=O(T_FD),
$$
$$
N_{Q(F)}=O(T_FD),\qquad \#Q(F)=O(T_FD),\qquad T_{Q(F)}=O(T_FD),
$$
where $d_i=\max_j\deg_{x_i} f_j$ and $D=n+\sum_i\lfloor\log_2 d_i\rfloor$.

### 3. Convert MQ over $\mathbb F_p$ to Boolean equations over $\mathbb C$

For an MQ system $F\subset\mathbb F_p[X]$:

1. Replace each finite-field variable by $x_i=\theta_{p-1}(X_i)$, giving Boolean polynomials $B(F)\subset\mathbb F_p[X_{\mathrm{bit}}]$.
2. For each polynomial $f_i^{\mathrm{bit}}$ with $t_i'$ terms, introduce Boolean variables $U_i$ and impose
   $$
   P(f_i^{\mathrm{bit}})=f_i^{\mathrm{bit}}-p\theta_{t_i'}(U_i)\in\mathbb Z[X_{\mathrm{bit}},U_i].
   $$
   A Boolean assignment satisfies $P(f_i^{\mathrm{bit}})=0$ over $\mathbb C$ exactly when $f_i^{\mathrm{bit}}=0\pmod p$.

The map from Boolean solutions of $P(F)$ back to $\mathbb F_p$ solutions is surjective, not injective. Sparseness and variable counts remain controlled:
$$
T_{P(F)}=\widetilde O(T_F\log^2 p),
$$
with
$$
N_{P(F)}=O\left(n\log p+\sum_i\log t_i+r\log\log p\right).
$$

Combining this with quadratisation yields the finite-field solver over $\mathbb F_p$. For $q=p^m$, the paper expands $\mathbb F_q=\mathbb F_p(\theta)$ and writes every equation as $m$ equations over $\mathbb F_p$. For MQ systems, this increases total sparseness by at most a factor $m^3$.

### 4. Turn inequalities into Boolean equations

For an integer inequality
$$
0\le g(X,Y)\le b,
$$
write
$$
\delta(g)=\theta_b(G)-g.
$$
After quadratising $g$ and encoding $X,Y,V$ by $\theta$ polynomials, Lemma 4.4 states that the original inequality is feasible exactly when the Boolean system
$$
I(I)=\{\delta(g_1),
\ldots,
\delta(g_s)\}\cup B(G)
$$
has a Boolean solution.

With $d_g=\max_i\deg(g_i)$, $h=\max\{p-1,b,u_1,\ldots,u_m\}$, $G=\{g_1,\ldots,g_s\}$, and total sparseness $T_G$, Lemma 4.5 gives
$$
N_{I(I)}=O((m+n)T_G d_g\log d_g\log h),
$$
$$
T_{I(I)}=O((m+n)T_G d_g^2\log d_g\log^2 h).
$$

### 5. Minimise by repeated Boolean feasibility checks

After reducing all equalities and inequalities, the optimization problem becomes a nonlinear $(0,1)$-program:
$$
\min_{W_{\mathrm{bit}}} o(W_{\mathrm{bit}})
\quad\text{subject to}\quad C(W_{\mathrm{bit}})=0.
$$
Assume $0\le o<u$. Maintain a feasible value interval $[\alpha,\mu)$. Choose
$$
\beta=\lfloor\log_2(
\mu-\alpha)\rfloor-1
$$
and ask whether there is a feasible assignment with
$$
o(W_{\mathrm{bit}})\in[\alpha,
\alpha+2^\beta).
$$
This is turned into one Boolean feasibility query by adding bits $F_j$ and the equation
$$
\delta_{\alpha\beta}(o)=\alpha+
\sum_{j=0}^{\beta-1}F_j2^j-o(W_{\mathrm{bit}})=0.
$$
If the feasibility oracle returns a solution, the upper bound is lowered to its objective value; if not, the lower interval is discarded. The interval shrinks by a constant factor, so the number of B-POSSO calls is $O(\log u)$.

### 6. Cryptographic reductions

The applications are mostly instantiations of the finite-field/optimization pipeline:

- **PSWN/MAX-POSSO:** introduce error variables $e_i$ and Boolean indicators $H_i=e_i^{p-1}$; minimise $\sum_i H_i$.
- **SIS:** solve $AX=0\pmod p$ with the norm inequality $0<\|X\|_2\le b$. The paper uses the centred encoding
  $$
  x_i=\theta_{p-1}(X_i)-\frac{p-1}{2}
  $$
  so the Euclidean norm picks the representative in $[-(p-1)/2,(p-1)/2]^n$.
- **SVP/CVP:** convert the lattice relation $v=\sum_i a_i b_i$ and norm objective to bounded integer optimization, using HNF bounds to bound the coefficients $a_i$.
- **NTRU:** encode the private key constraints $f\in L_f$, $g\in L_g$, the public relation $h=gf^{-1}\pmod q$, and invertibility mod $p,q$ as mixed complex / modular equations, then convert to Boolean equations.

## Key results

**Theorem 3.7 / solving over $\mathbb F_p$.**  For $F\subset\mathbb F_p[X]$, a solution in $\mathbb F_p^n$ can be found with probability at least $1-\epsilon$ in time
$$
\widetilde O\!\left(T_F^{3.5}D^{3.5}\log^{4.5}p\,\kappa^2\log(1/\epsilon)\right),
$$
where $\kappa$ is the condition number of $P(Q(F))$.

**Theorem 3.13 / solving over $\mathbb F_q$.**  For $q=p^m$,
$$
\widetilde O\!\left(m^{5.5}T_F^{3.5}D^{3.5}\log^{4.5}p\,\kappa^2\log(1/\epsilon)\right)
$$
suffices, with $\kappa$ the condition number of $P(G(Q(F)))$.

**Corollary 3.10 / MQ over $\mathbb F_p$.**  If $F$ is MQ, the stated runtime is
$$
\widetilde O\!\left((n\log p+r)^{2.5}T_F\log^2p\,\kappa^2\log(1/\epsilon)\right).
$$

**Theorem 5.4 / optimization.**  Algorithm QFpOpt solves the bounded optimization problem with success probability at least $1-\epsilon$ in time
$$
\widetilde O\!\left(N_{L_{\alpha\beta}}^{2.5}T_{L_{\alpha\beta}}\kappa^2\log(1/\epsilon)\log u\right),
$$
where
$$
N_{L_{\alpha\beta}}=
\widetilde O\!\left(nT_F\log d_f\log p+(m+n)T_{G_o}d_g\log h+
\log u\right),
$$
$$
T_{L_{\alpha\beta}}=
\widetilde O\!\left(nT_F\log d_f\log^2p+(m+n)T_{G_o}d_g^2\log^2h+
\log u\right),
$$
and $\kappa$ is the maximum condition number of the Boolean systems $L_{\alpha\beta}$ encountered during the interval search.

**Proposition 6.3 / PSWN.**  Polynomial systems with noise can be solved in
$$
\widetilde O\!\left(n^{3.5}T_F^{3.5}\log^8p\,\kappa^2\log(1/\epsilon)\right).
$$

**Proposition 7.5 / SIS.**  For $A\in\mathbb F_p^{r\times n}$ with $T_A$ nonzero entries and $T_A\ge n$, SIS can be solved in
$$
\widetilde O\!\left((n\log p+r)^{2.5}(T_A\log p+n\log^2p)\kappa^2\log(1/\epsilon)\right).
$$

**Proposition 8.5 / SVP.**  For a rank-$n$ lattice basis $B\subset\mathbb Z^m$ with $h=\|B\|_\infty$, SVP can be solved in
$$
\widetilde O\!\left(m(n^{7.5}+m^{2.5})(n^3+
\log h)\log^{4.5}h\,\kappa^2\log(1/\epsilon)\right).
$$

**Proposition 9.1 / NTRU key recovery.**  For NTRU parameters $(N,p,q)$ with $q>p$, a private key can be recovered from the public key in
$$
\widetilde O\!\left(N^{4.5}\log^{4.5}q\,\kappa^2\log(1/\epsilon)\right),
$$
where $\kappa$ is the condition number of the constructed Boolean system $F_{\mathrm{NTRU}}$.

## Comparison with prior work

| Approach | Problem model | Scaling / limitation | What this paper changes |
|---|---|---|---|
| Classical Gröbner / XL / Macaulay methods | Polynomial systems over finite fields | exponential in hard instances; sensitive to solving degree | Uses Macaulay matrices as QLSA input rather than for full classical elimination. |
| [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes|Chen--Gao 2018]] | Boolean systems / algebraic attacks | polynomial in $n,T_F$ only when $\kappa$ is small | This paper extends the front end to finite fields, inequalities, and optimization. |
| [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] search over assignments | generic NP search / cryptanalytic search | quadratic speedup, but still exponential in variable count | Replaces assignment search by QLSA on a derived algebraic system, paying $\kappa^2$. |
| [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes|Laarhoven--Mosca--van de Pol]] and later quantum sieves | SVP | exponential-time quantum speedups with explicit geometric heuristics | Gives a formal condition-number-dependent polynomial-time claim for SVP, but without evidence that $\kappa$ is small. |
| NTRU lattice attacks / algebraic attacks | NTRU key recovery | normally exponential or subexponential in security parameter | Encodes NTRU as a Boolean system for B-POSSO; the listed small exponents are meaningful only after multiplying by $\kappa^2$. |

## Limits / caveats

1. **The condition number is the bottleneck.**  The algorithms are polynomial-time only when $\kappa$ is polynomial. The paper repeatedly treats this as a security criterion, but it does not compute or bound $\kappa$ for the listed cryptosystems.

2. **The reductions enlarge the algebraic system.**  The Boolean systems are sparse in the asymptotic accounting, but they contain many auxiliary variables from quadratisation, interval encodings, modular quotient variables, and objective-search bits.

3. **The oracle model hides implementation cost.**  As with [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]], the Macaulay matrices are not materialised. The speedup assumes efficient sparse access to the derived matrices.

4. **The NTRU and lattice conclusions are conditional statements.**  The paper says these systems resist this attack only if the relevant condition numbers are large. That is not the same as an attack with a concrete resource estimate.

5. **Some bounds are coarse.**  The HNF coefficient bound for SVP is enough for a reduction, but it is not tight. It pushes the problem into the formal optimization framework rather than giving a competitive lattice algorithm.

6. **The later Macaulay condition-number critique applies here too.**  [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes|Ding--Gheorghiu--Gilyén--Hallgren--Li (2023)]] show that the Chen--Gao Macaulay-HHL subroutine has exponential truncated condition-number lower bounds in the Hamming weight of the Boolean solution. This does not formally kill every reduced finite-field instance, but it makes the cryptanalytic claims even more conditional.

## Reusable ideas

1. [[Truncated Binary Interval Encoding by Theta Polynomials]] — encode exactly $\{0,\ldots,b\}$ with $\lfloor\log_2 b\rfloor+1$ Boolean variables and no illegal overflow values.
2. [[Modular Finite-Field Equations as Boolean Complex Systems]] — turn $f=0\pmod p$ into $f^{\mathrm{bit}}-p\theta(U)=0$ over $\mathbb C$ after Boolean expansion.
3. [[Objective Bisection by Boolean Feasibility Oracles]] — minimise a bounded objective by adding interval bits and making $O(\log u)$ feasibility calls.
4. [[Centered Finite-Field Encoding for Norm Constraints]] — represent $\mathbb F_p$ elements by centred integer lifts before imposing Euclidean norm bounds.

## References within this paper

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes|Chen--Gao (2018)]] — source of the B-POSSO solver used as the quantum subroutine.
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow--Hassidim--Lloyd (2009)]] — QLSA source behind the B-POSSO algorithm.
- Ambainis (2012) and Childs--Kothari--Somma (2017) — QLSA improvements cited through the Boolean-solver background.
- Ajtai (1996), Regev (2005), Peikert (2009), and Albrecht et al. (2018) — lattice-cryptography background for SIS/LWE/SVP/NTRU security discussion.
- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes|Laarhoven (2015)]] and Aono--Nguyen--Shen (2018) — cited as lattice/SVP algorithms in the SVP discussion.
- Hoffstein--Pipher--Silverman (1998) — NTRU definition and recommended parameter examples.
- Albrecht--Cid (2011), Huang--Lin (2017), Zhao--Gao (2009) — PSWN/MAX-POSSO background.
- Karp (1972), Balas (1965), Genova--Guliashki (2011) — $(0,1)$-programming context.
- Arora--Ge (2011), Albrecht et al. (2014) — inequality/linearisation comparisons for algebraic LWE-style problems.

## Cross-links

### Paper notes

- [[Quantum Algorithm for Boolean Equation Solving and Quantum Algebraic Attack on Cryptosystems (Chen-Gao 2018) — Paper Notes]]
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
- [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]

### Trick cards

- [[Truncated Binary Interval Encoding by Theta Polynomials]]
- [[Modular Finite-Field Equations as Boolean Complex Systems]]
- [[Objective Bisection by Boolean Feasibility Oracles]]
- [[Centered Finite-Field Encoding for Norm Constraints]]
- [[Macaulay-HHL Pseudo-Solution States]]
- [[Truncated QLS Condition Number as a Right-Hand-Side No-Go Test]]
- [[Boolean Solution Extraction by Monomial Measurement]]
