# Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009)

> **Source:** Arthur Schmidt, *Quantum Algorithms for many-to-one Functions to Solve the Regulator and the Principal Ideal Problem*, arXiv:0912.4807 (2009)
> **Links:** [arXiv](https://arxiv.org/abs/0912.4807) · [PDF](https://arxiv.org/pdf/0912.4807)
> **Tags:** #hidden-subgroup-problem #period-finding #number-theory #quantum-algorithms

---

## The computational problem

The paper works in a real quadratic order $\mathcal{O}_\Delta = \mathbb{Z} + \frac{\Delta+\sqrt{\Delta}}{2}\mathbb{Z}$, where $\Delta > 0$ is a nonsquare discriminant with $\Delta \equiv 0,1 \pmod 4$.

**Regulator problem.** Given $\Delta$, compute an integer $R'$ such that
$$
|R' - R^+| < 1,
$$
where $R^+$ is the narrow regulator of $\mathbb{Q}(\sqrt{\Delta})$. Schmidt works with the narrow regulator because the period of the principal cycle for norm-one units is $R^+$; in a real quadratic field, $R = R^+$ or $R=R^+/2$.

**Principal ideal problem (PIP).** Given a reduced indefinite binary quadratic form $g=(a,b,c)$ of discriminant $\Delta$, decide whether $g$ is principal and, if it is, compute its distance $\delta(g)$ around the principal cycle. In the cryptographic use case from Buchmann-Williams key exchange, the input is principal by construction, so returning the distance is enough to break the protocol.

The complexity measure is bit complexity in $\log \Delta$, plus the number of qubits used by the quantum subroutines. The paper focuses on reducing the qubit footprint of the known [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes|Hallgren]] regulator/PIP algorithms rather than changing their asymptotic polynomial-time status.

## What the paper does

Schmidt gives lower-qubit variants of Hallgren's real-quadratic regulator and PIP algorithms. The two changes are specific but useful: allow the sampled period function to be many-to-one on a fundamental period, and store only a reduced quadratic form $(a,b,c)$ with $a>0$ rather than a form/ideal plus a distance.

The resulting algorithms still use QFT period-lattice sampling, but the period scale drops from roughly $\lceil\sqrt{\Delta}\rceil R$ to $4R^+$ or $8R^+$, and the logarithm precision needed in the function evaluation drops from $1/\sqrt{\Delta}$ to a constant-scale $1/8$. The headline resource claim is a saving of at least $2\log \Delta$ qubits over Hallgren's construction.

My assessment: this is not a new speedup class; it is an implementation-aware repair/refinement of the real-quadratic period-finding pipeline. The useful conceptual move is that one-to-one-on-a-period is stronger than the QFT argument actually needs.

## The algorithm / construction

### Number-theoretic representation

Schmidt switches between ideals and indefinite binary quadratic forms. A reduced form of discriminant $\Delta$ is
$$
(a,b,c), \qquad \Delta=b^2-4ac,
$$
with
$$
|\sqrt{\Delta}-2|a|| < b < \sqrt{\Delta}.
$$
There is a standard bijection between invertible ideals and $\Gamma$-orbits of irreducible indefinite forms with positive first coefficient $a$; distances on ideals correspond to distances on forms.

Let $R^+$ be the set of reduced principal forms with $a>0$. Walking by the reduction operator $\rho$ moves around the principal cycle. The important spacing fact is:
$$
\delta(\rho^2(g)) - \delta(g) > \ln 2
$$
for $g\in R^+$, while the spacing among all reduced forms can be as small as $1/\sqrt{\Delta}$. This is the qubit-saving reason for using only positive-$a$ forms.

To jump to the form left of a large real position $x$, the algorithm uses giant steps: compose forms, reduce, and track distance approximately. If $f_1*f_2$ is the reduced composition, then
$$
\delta(f_1*f_2)=\delta(f_1)+\delta(f_2)+\delta',
$$
where the correction $\delta'$ is bounded by $\pm \ln\Delta$. For $x<\Delta^2$, square-and-multiply uses at most $c\log_2\Delta$ operations for a constant $c<10$. Choosing logarithm precision at least $1/(8c\log\Delta)$ gives
$$
|\delta(\widetilde{g}_{x/4})-\widetilde{\delta}(\widetilde{g}_{x/4})|<1/8.
$$

### Regulator function

Define
$$
\operatorname{Reg}:\mathbb{Z}\to R^+,
\qquad
x\mapsto \widetilde{g}_{x/4},
$$
where $\widetilde{g}_{x/4}$ is the reduced principal form in $R^+$ left of or at $x/4$, computed using approximate logarithms.

This function is periodic with period about $4R^+$, but it is many-to-one inside each period: a block of consecutive integers maps to the same form. Lemma 2 says that if $R^+>5\ln\Delta$, then for each $g\in R^+$ and each period there is a run length $m(g,k)$ satisfying
$$
1\le m(g,k)<\ln\Delta+3,
$$
with variation at most $4$ between periods:
$$
\max_{k,k'} |m(g,k)-m(g,k')|\le 4.
$$
This bounded run-length variation is enough for constructive interference after the QFT.

### Regulator-Dual subroutine

Choose a power of two $q$ with
$$
q/2\le 5\Delta(\ln\Delta)^2 < q.
$$
The subroutine:

1. Prepare
   $$
   \frac{1}{\sqrt q}\sum_{x=0}^{q-1}|x\rangle |(1,\Delta\bmod 2)\rangle .
   $$
2. Compute $\operatorname{Reg}(x)$ into the second register.
3. Measure the second register, leaving a superposition over blocks near
   $$
   x' + 4kR^+ + m + \epsilon(x',k).
   $$
4. Apply the QFT over a register of size $4q$ and measure $y$.

With probability at least $2^{-11}$, the measured $y$ has the form
$$
y=(q/R^+)z+\omega,
\qquad z\in\mathbb{Z},\quad |\omega|\le 1/2.
$$
Two such samples are enough, with continued fractions, to recover an approximation to $R^+$.

### Full regulator algorithm

If $R^+<32\ln\Delta$, compute classically using Biehl-Buchmann/Maurer methods. Otherwise:

1. Run Regulator-Dual twice to get
   $$
   y_i=(q/R^+)z_i+\omega_i,
   \qquad |\omega_i|\le 1/2.
   $$
2. Use the continued fraction expansion of $y_1/y_2$ to recover $z_1/z_2$; Lemma 3 gives
   $$
   \left|\frac{y_1}{y_2}-\frac{z_1}{z_2}\right|\le \frac{1}{2z_2^2}
   $$
   when $q>(R^+)^2$.
3. Output $qz_1/y_1$ and improve it classically.

### PIP function and period lattice

For a reduced form $g$, define
$$
\operatorname{PIP}:\mathbb{Z}\times\mathbb{Z}\to R^+,
\qquad
(x,y)\mapsto \widetilde{g}_{(x,y)},
$$
where $\widetilde{g}_{(x,y)}$ is the reduced principal form in $R^+$ left of or at $\delta(g^x)+y/4$, again with logarithm precision chosen so that the distance error is below $1/8$.

Let $n$ be the smallest positive integer with $g^n\sim\mathcal{O}_\Delta$, and let $S=\operatorname{dist}(\mathcal{O}_\Delta,g^n)$. The period lattice is generated by
$$
\Lambda=\left\langle
\begin{pmatrix}n\\-S\end{pmatrix},
\begin{pmatrix}0\\R^+\end{pmatrix}
\right\rangle,
$$
with dual basis
$$
\begin{pmatrix}
1/n & 0\\
S/(4nR^+) & 1/(4R^+)
\end{pmatrix}.
$$

The AlgPIP-Dual routine samples approximate vectors from $8q\Lambda^*$ using a two-dimensional QFT. It then combines two sampled vectors using an extended gcd step on the second-coordinate coefficients to recover $S$ when $g$ is principal.

Two-dimensional Fourier sampling is needed because the PIP period object is a rank-2 lattice: one direction records the order $n$ of the form class, while the other records the regulator-period direction. A one-dimensional regulator sample would not recover the offset $S$.

## Key results

**Theorem 1 (Regulator-Dual).** Let $\Delta$ be a real-quadratic discriminant with regulator at least $32\ln\Delta$. Regulator-Dual computes an approximation to a random element of $(q/R^+)\mathbb{Z}$ of the form
$$
(q/R^+)z+\omega,
\qquad z\in\mathbb{Z},\quad |\omega|\le 1/2,
$$
with probability at least $2^{-11}$. It uses at most
$$
2\log\Delta+2\log\ln\Delta+N+7
$$
qubits, where $N$ is the temporary workspace needed for form operations. Schmidt cites $N<10.5\log\Delta+O(\log^2\log\Delta)$ from his thesis.

**Theorem 2 (Regulator).** The regulator algorithm computes an approximation of $R^+$ for $\mathbb{Q}(\sqrt{\Delta})$ in quantum polynomial time. The paper states the time as $O(\operatorname{polylog}(\log\Delta))$; read cautiously, since the surrounding discussion and operation counts are polynomial in $\log\Delta$. The algorithm is Monte Carlo with success probability at least
$$
2^{-26},
$$
and it uses at most
$$
2\log\Delta+2\log\ln\Delta+N+7
$$
qubits.

**Theorem 3 (AlgPIP-Dual).** AlgPIP-Dual samples approximate vectors from $8q\Lambda^*$ in quantum polynomial time with probability at least
$$
2^{-16},
$$
and uses at most
$$
3\log\Delta+4\log\ln\Delta+N
$$
qubits.

**Theorem 4 (AlgPIP).** AlgPIP solves the principal ideal problem in a real quadratic number field in quantum polynomial time. It is Monte Carlo with success probability at least
$$
2^{-37},
$$
and the paper states a qubit bound of
$$
3\log\Delta+2\log\ln\Delta+N.
$$
The output "not principal" is not independently certified by this basic algorithm; the paper notes that stronger lattice-basis recovery methods from Buchmann-Pohst and Buchmann-Kessler can address that case. If the output is a distance $\delta$, it can be checked classically.

## Comparison with prior work

| Work | Period function | Function value stored | Period scale | Log precision issue | Qubits / resource point |
|---|---|---|---|---|---|
| [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] | one-to-one cosets over finite cyclic groups | modular powers | integer period | exact modular arithmetic | baseline finite-period method |
| [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes|Hallgren (2002/2007)]] | weakly periodic real-domain function | reduced ideal/form plus distance data | roughly $\lceil\sqrt{\Delta}\rceil R$ after discretisation | handled by subset periodicity in the journal proof | polynomial time; larger registers |
| Schmidt-Vollmer (2005) | number-field unit group construction | noncanonical number-theoretic data | higher-dimensional period lattice | avoids Hallgren's approximation gap by different constructions | arbitrary-degree unit groups |
| **Schmidt (2009)** | many-to-one periodic function | reduced form $(a,b,c)$ with $a>0$; store $a,b$ only | $4R^+$ for regulator, $8R^+$ for PIP | constant-scale $1/8$ distance tolerance via positive-$a$ spacing | saves at least $2\log\Delta$ qubits over Hallgren |

This is best understood as a resource refinement and a proof-pattern extension: many-to-one period functions can still give sharp enough Fourier samples when the fibre shapes vary mildly across periods.

| Feature | Hallgren regulator/PIP | Schmidt regulator/PIP |
|---|---|---|
| Period function | weakly periodic and essentially one-to-one after storing distance data | many-to-one on each period, with bounded run-length variation |
| Stored value | reduced ideal/form plus distance information | reduced form with positive first coefficient |
| Fourier proof burden | handles discretization and bad points in the real-domain function | shows repeated fibres still interfere constructively |
| Resource change | larger period scale and precision registers | smaller period scale and at least `2 log Delta` qubit saving |

## Limits / caveats

- The algorithms are for real quadratic fields. The paper leaves open whether the same many-to-one construction helps class groups of real quadratic fields or higher-degree number fields.
- Success probabilities are constant but small: $2^{-26}$ for regulator and $2^{-37}$ for PIP. Repetition is fine asymptotically, but constants matter for any resource estimate.
- The paper does not give a full gate/qubit accounting for the form arithmetic in this conference-style note; it cites Schmidt's thesis for $N<10.5\log\Delta+O(\log^2\log\Delta)$.
- The stated $O(\operatorname{polylog}(\log\Delta))$ time bound in Theorems 2 and 4 looks inconsistent with the rest of the paper's polynomial-in-$\log\Delta$ arithmetic. I would treat it as a notation/wording issue unless checked against the thesis.
- The PIP routine's "not principal" output is not self-certifying in the basic version. For breaking Buchmann-Williams, that is acceptable because the protocol supplies principal instances.

## Reusable ideas

1. [[Many-to-One Period Fibres with Bounded Run-Length Variation]] — QFT period finding can tolerate many-to-one fibres when each fibre appears in short, similarly sized runs in every period.
2. [[Positive-Coefficient Quadratic Forms for Constant-Spaced Infrastructure Sampling]] — restrict to reduced forms with $a>0$ so adjacent sampled forms are separated by at least $\ln 2$, not $1/\sqrt{\Delta}$.
3. [[Approximate Logarithms with Tie-Tolerant Period Functions]] — avoid exact distance comparisons by designing the period function so a fixed $1/8$ distance error only changes bounded transition zones.

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — QFT period finding, the template being adapted.
- Boneh-Lipton (1995) — quantum cryptanalysis of hidden linear functions; many-to-one period finding over integer domains.
- Mosca-Ekert (1999), Hales-Hallgren (2000), Hales (2002) — extensions of abelian HSP/period finding beyond one-to-one finite functions.
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes|Hallgren (2002/2007)]] — the real-quadratic regulator and PIP algorithm being refined.
- Schmidt-Vollmer (2005) — unit-group algorithm for number fields using different number-theoretic constructions.
- Buchmann-Williams (1990) — real-quadratic key exchange attacked by solving PIP.
- Buchmann-Vollmer (2007), Jacobson-Williams (2009), Lenstra (1982) — classical quadratic-form and infrastructure background.
- Biehl-Buchmann (1994), Maurer (2000) — classical regulator approximation and small-regulator handling.
- Buchmann-Pohst (1989), Buchmann-Kessler (1993) — lattice-basis recovery from approximate dual vectors.

## Cross-links

### Paper notes

- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]]
- [[Quantum Algorithms for Algebraic Problems (Childs-van Dam 2010) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]

### Trick cards

- [[Many-to-One Period Fibres with Bounded Run-Length Variation]]
- [[Positive-Coefficient Quadratic Forms for Constant-Spaced Infrastructure Sampling]]
- [[Approximate Logarithms with Tie-Tolerant Period Functions]]
- [[Period Finding for Irrational Periods on R]]
- [[Infrastructure of Real Quadratic Fields as Period Domain]]
- [[Interference-Based Period Finding]]
- [[Order-Finding via QFT and Continued Fractions]]
