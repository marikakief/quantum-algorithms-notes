# Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms (Roetteler-Naehrig-Svore-Lauter 2017) — Paper Notes

> **Source:** Martin Roetteler, Michael Naehrig, Krysta M. Svore, and Kristin Lauter, *Quantum Resource Estimates for Computing Elliptic Curve Discrete Logarithms*, arXiv:1706.06752, ASIACRYPT 2017
> **Links:** [arXiv](https://arxiv.org/abs/1706.06752) · [PDF](https://arxiv.org/pdf/1706.06752)
> **Tags:** #elliptic-curves #discrete-log #Shor #quantum-arithmetic #resource-estimation #Toffoli #cryptanalysis

---

## The computational problem

The paper gives a gate-level resource estimate for [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's discrete-log algorithm]] applied to prime-field elliptic curves.

**Input.** A short Weierstrass curve over a prime field,

$$
E: y^2 = x^3 + ax + b \pmod p,
$$

where $p>3$ has $n=\lceil \log_2 p\rceil$ bits, a point $P\in E(\mathbb F_p)$ of known prime order $r$, and a point $Q\in \langle P\rangle$.

**Output.** The discrete logarithm

$$
m\in \mathbb Z/r\mathbb Z\quad\text{such that}\quad Q=[m]P.
$$

**Complexity measure.** Logical qubits, Toffoli count, and Toffoli depth for reversible circuits. The resource count is at the logical Toffoli-network level; it does not include a full surface-code layout or magic-state factory schedule.

Local conventions: $n=\lceil\log_2 p\rceil$ is the prime-field bit length, $r$ is the subgroup order, "logical qubits" are algorithm-level qubits, "Toffoli count" is the number of Toffoli gates in the reversible network, and "Toffoli depth" is the corresponding logical depth before physical error-correction layout.

---

## What the paper does

This is the modern Toffoli-network resource estimate for prime-field ECDLP, filling in the implementation gap left by [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes|Proos--Zalka]]. They implement and classically simulate the reversible point-addition core in LIQUi$|\rangle$ for NIST-sized curves up to P-521.

The headline bound is clear: ECDLP over an $n$-bit prime field uses at most

$$
9n+2\lceil \log_2 n\rceil+10
$$

logical qubits and about

$$
448n^3\log_2 n+4090n^3
$$

Toffoli gates for the full Shor circuit. My assessment: this is not a new quantum algorithm, but it is the right baseline to cite when comparing fault-tolerant attacks on ECC and RSA before the Gidney--Ekerå style optimisations.

---

## The algorithm / construction

### 1. Shor ECDLP as double scalar multiplication

For $Q=[m]P$, Shor's ECDLP circuit prepares two $(n+1)$-qubit Fourier registers and computes

$$
\frac{1}{2^{n+1}}\sum_{k,\ell=0}^{2^{n+1}-1}|k,\ell\rangle
\longmapsto
\frac{1}{2^{n+1}}\sum_{k,\ell=0}^{2^{n+1}-1}|k,\ell\rangle|[k]P+[\ell]Q\rangle.
$$

After discarding the point register, it applies QFTs to the two scalar registers and classically post-processes the two measurements to recover $m$. As in [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]], the circuit can use the Griffiths--Niu semiclassical QFT so the two Fourier registers are replaced by one reusable control qubit plus classical feed-forward; see [[Semiclassical QFT Register Recycling for Abelian Period Finding]].

The double scalar multiplication is implemented as $2n$ controlled additions of classically precomputed points:

$$
[2^i]P,\qquad [2^i]Q.
$$

So the real cost driver is a reversible controlled map

$$
|S\rangle\mapsto |S+A\rangle
$$

for a fixed classical point $A$.

### 2. Reversible modular arithmetic

The implementation uses only Toffoli/CNOT/NOT networks for arithmetic, following the same design philosophy as [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]]. The modular subroutines are:

1. **Integer addition and subtraction.** Based on Takahashi--Tani--Kunihiro adders, with controlled variants and constant-addition routines from [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]].
2. **Modular addition.** In-place map $|x\rangle|y\rangle\mapsto |x\rangle|(x+y)\bmod p\rangle$, using subtract-$p$, a carry/reduction flag, add-back-$p$, and uncomputation by comparison.
3. **Modular doubling.** In-place $|x\rangle\mapsto |2x\bmod p\rangle$ for odd $p$, implemented by a cyclic bit shift plus conditional subtract/add of $p$.
4. **Modular multiplication.** Two versions are studied: schoolbook double-and-add, and Montgomery multiplication. The reported estimates use Montgomery multiplication because the inversion circuit already supplies enough workspace, so Montgomery's extra ancillae are essentially free at the point-addition level.
5. **Modular inversion.** A reversible Montgomery--Kaliski binary-GCD inverse; this is the largest and most expensive subroutine.

The modular arithmetic counts in Table 1 of the paper include:

| Subroutine | Qubits | Ancillae | Toffoli count |
|---|---:|---:|---:|
| add/sub constant mod $p$ | $2n$ | $n$ | $16n\log_2 n-26.9n$ |
| controlled add/sub constant mod $p$ | $2n+1$ | $n$ | $16n\log_2 n-26.9n$ |
| controlled subtraction mod $p$ | $2n+4$ | $3$ | $16n\log_2 n-23.8n$ |
| controlled negation mod $p$ | $n+3$ | $2$ | $8n\log_2 n-14.5n$ |
| multiplication mod $p$ by doubling/addition | $3n+2$ | $2$ | $32n^2\log_2 n-59.4n^2$ |
| Montgomery multiplication mod $p$ | $5n+4$ | $2n+4$ | $16n^2\log_2 n-26.3n^2$ |
| squaring mod $p$ by doubling/addition | $2n+3$ | $3$ | $32n^2\log_2 n-59.4n^2$ |
| Montgomery squaring mod $p$ | $4n+5$ | $2n+5$ | $16n^2\log_2 n-26.3n^2$ |
| inversion mod $p$ | $7n+2\lceil\log_2 n\rceil+9$ | $5n+2\lceil\log_2 n\rceil+9$ | $32n^2\log_2 n$ |

### 3. Montgomery multiplication as the multiplier choice

The double-and-add multiplier computes

$$
xy\equiv \sum_{i=0}^{n-1}x_i2^iy\pmod p
$$

by repeated modular doubling and controlled modular addition. It uses fewer qubits, but each round has modular reductions.

The Montgomery multiplier represents $a$ as $aR\bmod p$, with $R=2^n$, and computes

$$
(xR)(yR)R^{-1}\equiv xyR\pmod p.
$$

Its rounds replace reductions by add-$p$-if-odd and halving shifts. The circuit stores one bit $m_i$ per round recording whether $p$ was added; after copying out the result, it runs the multiplication backward to erase these bits. See [[Montgomery Multiplication as a Quantum Workspace Tradeoff]].

### 4. Reversible Montgomery--Kaliski inversion

The inversion routine implements Kaliski's Montgomery inverse via the binary extended Euclidean algorithm. It starts with

$$
u=p,
\qquad v=x,
\qquad r=0,
\qquad s=1,
$$

and maintains the invariant

$$
p=rv+su.
$$

At each step it applies one of four branches:

$$
\begin{array}{ll}
 u\text{ even}: & (u,s)\leftarrow (u/2,2s),\\
 v\text{ even}: & (v,r)\leftarrow (v/2,2r),\\
 u>v\text{ odd}: & (u,r,s)\leftarrow ((u-v)/2,r+s,2s),\\
 u\le v\text{ odd}: & (v,r,s)\leftarrow ((v-u)/2,2r,r+s).
\end{array}
$$

The usual algorithm halts after an input-dependent number $k$ of steps. The quantum circuit cannot halt different basis states at different physical times, so the paper pads the computation to exactly $2n$ rounds, the worst-case bound for Kaliski's algorithm. Once $v=0$, a flag switches the circuit into counter mode for the remaining rounds.

Each round emits one bit $m_i$ that records enough branch information to reverse the step. The non-obvious part is that two branch bits are not needed: after a round, parity of $r$ or $s$ recovers one of them because exactly one of $r,s$ is even. See [[One-Bit Branch Logging for Reversible Binary GCD]].

After $2n$ rounds, $r$ holds an almost inverse of the form $-x^{-1}2^k$. The circuit converts it to $x^{-1}2^n\bmod p$ by controlled modular doublings and a sign flip, copies out the answer, then runs the whole inverse backward. See [[Fixed-Round Reversible Montgomery-Kaliski Inversion]].

### 5. Controlled affine point addition

For two affine points $P_1=(x_1,y_1)$ and fixed $P_2=(x_2,y_2)$, the generic group law uses

$$
\lambda=\frac{y_1-y_2}{x_1-x_2},
\qquad
x_3=\lambda^2-x_1-x_2,
\qquad
 y_3=(x_1-x_3)\lambda-y_1.
$$

The reversible point adder is a straight-line program over $x_1,y_1,t_0,\lambda$ that computes $P_1\leftarrow P_1+P_2$ if a control bit is set, and restores all work registers if it is not. It uses the same slope-uncomputation idea as [[Slope-Uncomputed In-Place Elliptic-Curve Shifts]]: the slope can be recomputed from the output point via

$$
\frac{y_1-y_2}{x_1-x_2}=\frac{-y_3+y_2}{x_3-x_2}.
$$

The implemented point addition uses 4 inversions, 4 multiplications, 2 squarings, and lower-order additions/subtractions. Since the inversion workspace dominates, the full controlled point addition requires

$$
9n+2\lceil\log_2 n\rceil+10
$$

logical qubits.

### 6. Exceptional cases are still probabilistic

The affine formula is generic: it excludes $P_1=O$, $P_2=O$, $P_1=P_2$, and $P_1=-P_2$. The paper follows [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes|Proos--Zalka]] and starts the accumulator at a random nonzero offset $[a]P$ rather than at $O$.

For scalar multiplication by $P$, if $S_a$ is the set of scalars that trigger exceptional doublings, the paper bounds

$$
\#S_a\le 2^{n-i_a}+8,
\qquad i_a=\lceil\log a\rceil,
$$

and the average number of invalid scalars is roughly

$$
\lceil\log r\rceil+10\approx n+10.
$$

The same argument handles additions to a point's negative. For the double scalar multiplication, the probability that a random pair $(k,\ell)$ remains valid is at least

$$
\left(1-\frac{n}{2^n}\right)^2
=1-\frac{n}{2^{n-1}}+\frac{n^2}{2^{2n}}
\approx 1-\frac{n}{2^{n-1}}.
$$

So the expected fidelity loss is negligible at cryptographic sizes, but the circuit is not an exact complete group law. See [[Random-Offset Generic Elliptic-Curve Group Shifts]].

---

## Key results

### Controlled point addition

For an elliptic curve over an $n$-bit prime field, the controlled generic affine point addition circuit uses

$$
9n+2\lceil\log_2 n\rceil+10
$$

logical qubits.

The fitted Toffoli count for one controlled point addition is

$$
224n^2\log_2 n+2045n^2
$$

up to lower-order terms. The leading coefficient comes from

$$
4\cdot 32+2\cdot 16+4\cdot 16=224,
$$

namely 4 inversions, 2 Montgomery squarings, and 4 Montgomery multiplications.

### Full Shor ECDLP circuit

The full Shor circuit applies $2n$ controlled point additions and needs no additional quantum workspace beyond the point-addition circuit. Thus the Toffoli count is estimated as

$$
(448\log_2 n+4090)n^3.
$$

Equivalently,

$$
448n^3\log_2 n+4090n^3.
$$

### Simulated cryptographic-size instances

The following values are copied from the paper's resource table for simulated NIST-size instances and RSA comparisons.

| ECC field size $n$ | Logical qubits | Full ECDLP Toffolis | Toffoli depth | Matched RSA size | RSA qubits | RSA Toffolis |
|---:|---:|---:|---:|---:|---:|---:|
| 110 | 1014 | $9.44\times 10^9$ | $8.66\times 10^9$ | 512 | 1026 | $6.41\times 10^{10}$ |
| 160 | 1466 | $2.97\times 10^{10}$ | $2.73\times 10^{10}$ | 1024 | 2050 | $5.81\times 10^{11}$ |
| 192 | 1754 | $5.30\times 10^{10}$ | $4.86\times 10^{10}$ | — | — | — |
| 224 | 2042 | $8.43\times 10^{10}$ | $7.73\times 10^{10}$ | 2048 | 4098 | $5.20\times 10^{12}$ |
| 256 | 2330 | $1.26\times 10^{11}$ | $1.16\times 10^{11}$ | 3072 | 6146 | $1.86\times 10^{13}$ |
| 384 | 3484 | $4.52\times 10^{11}$ | $4.15\times 10^{11}$ | 7680 | 15362 | $3.30\times 10^{14}$ |
| 521 | 4719 | $1.14\times 10^{12}$ | $1.05\times 10^{12}$ | 15360 | 30722 | $2.87\times 10^{15}$ |

The RSA comparison uses interpolation from [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]].

---

## Comparison with prior work

| Work | Target | Main circuit choice | Logical qubits | Gate / Toffoli scaling | Comment |
|---|---|---|---:|---:|---|
| [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes|Proos--Zalka (2003)]] | prime-field ECDLP | affine point addition, Euclidean inversion, register sharing | about $5n+8\sqrt n+4\log_2 n$ with sharing | $O(n^3)$ in addition units | Better space estimate, but not a fully simulated Toffoli network. |
| Maslov et al. (2009) | binary-field ECDLP | $\mathrm{GF}(2^m)$ arithmetic | not directly comparable | $O(m^2)$ depth | Different field model. |
| [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner--Roetteler--Svore (2017)]] | RSA factoring | Toffoli modular multiplication | $2n+2$ | $(64(\log_2 n-2)+29.46)n^3$ | Resource comparison target for RSA. |
| **Roetteler--Naehrig--Svore--Lauter (2017)** | prime-field ECDLP | Toffoli affine point addition with Montgomery arithmetic | $9n+2\lceil\log_2 n\rceil+10$ | $(448\log_2 n+4090)n^3$ | Simulated point-addition networks through P-521. |
| [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes|Gidney--Ekerå (2021)]] | RSA factoring | windowed arithmetic, coset representation, carry runways | surface-code estimate | much lower RSA Toffoli count | Later RSA-specific improvement; not an ECDLP replacement. |

Assessment: compared to [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes|Proos--Zalka]], this paper pays extra qubits for a cleaner, fixed-round binary-GCD inversion with simulated Toffoli circuits. Compared to RSA estimates at matching classical security, ECC remains the easier quantum target in this model because ECC's input size is so much smaller.

---

## Limits / caveats

1. **No full fault-tolerant layout.** Toffoli counts are logical-circuit estimates. Physical qubit counts, factory scheduling, routing, and code distances are not costed.

2. **Generic affine addition is approximate.** The circuit does not implement a complete elliptic-curve group law. The paper bounds the exceptional-case amplitude and argues the fidelity loss is negligible, but this is still an approximation. Complete formulas or Edwards-like models are left open.

3. **Projective coordinates are not used.** Classically, projective coordinates avoid almost all inversions. Quantumly, the extra equivalence-class representation and extra work registers complicate uncomputation. This is a real open design question, not a minor optimisation.

4. **Inversion dominates space.** The $9n+2\lceil\log_2 n\rceil+10$ qubit count comes from the reversible Montgomery--Kaliski inverse. The paper explicitly leaves Proos--Zalka-style register sharing for binary GCD as future work.

5. **Montgomery multiplication helps only because inversion workspace is already present.** If the surrounding algorithm did not already require $5n+O(\log n)$ ancillae, the Montgomery multiplier's space cost would matter.

6. **RSA comparison is time-local.** Later RSA factoring estimates, especially [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes|Gidney--Ekerå]], change the RSA side substantially. This paper remains the ECDLP baseline, but not the last word on RSA.

---

## Reusable ideas

1. [[Montgomery Multiplication as a Quantum Workspace Tradeoff]] — use Montgomery form to reduce arithmetic depth and Toffoli count when another subroutine already provides enough work qubits.
2. [[Fixed-Round Reversible Montgomery-Kaliski Inversion]] — turn a data-dependent binary-GCD inverse into a fixed $2n$-round reversible circuit using counter padding and compute-copy-uncompute cleanup.
3. [[One-Bit Branch Logging for Reversible Binary GCD]] — record a four-way binary-GCD branch with one persistent bit by recovering the other bit from the parity invariant of the coefficient registers.
4. [[Slope-Uncomputed In-Place Elliptic-Curve Shifts]] — reuse the output-side slope identity to make a fixed affine point shift in-place.
5. [[Random-Offset Generic Elliptic-Curve Group Shifts]] — avoid exact exceptional-case logic by shifting the accumulator to a random nonzero group element and bounding the lost amplitude.
6. [[Binary Search Fault Localisation for Toffoli Networks]] — the same Toffoli-network testability argument matters here because the point-addition core is classically simulable on basis states.

---

## References within this paper

- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes|Proos--Zalka (2003)]] — main predecessor for ECDLP circuits; this paper implements and simulates the arithmetic they sketched.
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes|Häner--Roetteler--Svore (2017)]] — Toffoli-based RSA factoring estimate used for comparison; also supplies constant-addition tools.
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994, 1997)]] — underlying factoring and discrete-log quantum algorithms.
- Griffiths and Niu (1996) — semiclassical QFT, used to recycle Fourier-register qubits.
- Beauregard (2003) — $2n+3$ qubit Shor factoring circuit using phase-estimation style register recycling.
- Kaliski (1995) — Montgomery inverse algorithm used inside the reversible inversion circuit.
- Montgomery (1985) — Montgomery multiplication and reduction.
- Takahashi, Tani, and Kunihiro (2010) — integer addition circuits used as the base reversible adder.
- Renes, Costello, and Batina (2016) — complete addition formulas for prime-order elliptic curves; cited as a possible route to exact group addition.
- [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes|Ekerå--Håstad (2017)]] — reduces RSA resources via shorter quantum exponent plus classical lattice post-processing; does not apply to generic ECDLP in this paper's table.
- Bernstein, Biasse, and Mosca (2017) — low-resource factoring using partial quantum resources; shifts RSA comparisons but requires separate constant analysis.

---

## Cross-links

### Paper notes

- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]]
- [[Factoring using 2n+2 qubits with Toffoli based modular multiplication (Häner-Roetteler-Svore 2017) — Paper Notes]]
- [[How to Factor 2048 Bit RSA Integers in 8 Hours Using 20 Million Noisy Qubits (Gidney-Ekera 2021) — Paper Notes]]
- [[Quantum Algorithms for Computing Short Discrete Logarithms and Factoring RSA Integers (Ekerå-Håstad 2017) — Paper Notes]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]
- [[A Reduction of Semigroup DLP to Classic DLP (Banin-Tsaban 2013) — Paper Notes]]

### Trick cards

- [[Montgomery Multiplication as a Quantum Workspace Tradeoff]]
- [[Fixed-Round Reversible Montgomery-Kaliski Inversion]]
- [[One-Bit Branch Logging for Reversible Binary GCD]]
- [[Semiclassical QFT Register Recycling for Abelian Period Finding]]
- [[Slope-Uncomputed In-Place Elliptic-Curve Shifts]]
- [[Random-Offset Generic Elliptic-Curve Group Shifts]]
- [[Binary Search Fault Localisation for Toffoli Networks]]
- [[Dirty Ancilla Constant Addition]]
