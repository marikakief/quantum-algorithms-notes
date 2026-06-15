# Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes

> **Source:** Thomas Santoli and Christian Schaffner, *Using Simon's algorithm to attack symmetric-key cryptographic primitives*, arXiv:1603.07856v3 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1603.07856) · [PDF](https://arxiv.org/pdf/1603.07856)
> **Tags:** #quantum-cryptanalysis #Simon #symmetric-key #Feistel #CBC-MAC

---

## The computational problem

The paper studies symmetric-key primitives under **Q2 access**: the adversary may query the primitive coherently in superposition. A classical oracle $O$ is accessed as

$$
U_O|x,y\rangle=|x,y\oplus O(x)\rangle.
$$

Two tasks are treated.

### 3-round Feistel distinguishing

The oracle

$$
V:\{0,1\}^{2n}\to\{0,1\}^{2n}
$$

is promised to be either a uniformly random permutation or a three-round Feistel network with internal functions $P_1,P_2,P_3:\{0,1\}^n\to\{0,1\}^n$:

$$
V(a\|c)=c\oplus P_2(a\oplus P_1(c))
\;\|\;
 a\oplus P_1(c)\oplus P_3(c\oplus P_2(a\oplus P_1(c))).
$$

The goal is to distinguish these two cases with polynomially many quantum queries to $V$.

### CBC-MAC forgery

For a pseudorandom permutation $F_k$ (modelled first as a random permutation $\pi$), fixed-length CBC-MAC on $\ell$ blocks is

$$
\operatorname{CBC}^{\ell}_{F_k}(m_1\|\cdots\|m_\ell)
=F_k(F_k(\cdots F_k(F_k(m_1)\oplus m_2)\oplus\cdots)\oplus m_\ell).
$$

For $\ell\geq 3$, the adversary chooses a prefix, makes only queries on messages containing at least one zero block, and must output a valid tag for a message with that prefix that was never queried.

The paper uses [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon’s algorithm]] as the engine: if $f(x)=f(y)$ exactly when $x\oplus y\in\{0^n,s\}$, Simon sampling recovers $s$ from $O(n)$ quantum queries and linear algebra over $\mathbb F_2$. A classical bounded-error algorithm needs $\Omega(2^{n/2})$ queries for the standard Simon problem.

## What the paper does

Santoli and Schaffner show that two classical security statements for symmetric constructions fail in the Q2 model: three-round Feistel is classically pseudorandom but quantum distinguishable, and fixed-length CBC-MAC admits a polynomial-query quantum forgery. The Feistel part extends Kuwakado--Morii’s permutation-round attack to arbitrary internal functions by using only a one-way Simon-period implication; the CBC-MAC part is the more original contribution.

My assessment: this paper is best read as a model-separation result, not as an implementation threat. It says that classical proofs for modes and constructions do not automatically survive coherent oracle access. That point aged well; the concrete oracle model remains strong.

## The algorithm / construction

### Weak-period Simon sampling

The paper observes that Simon’s circuit does not need the full promise to ensure the measured vector is orthogonal to the intended shift. It is enough that

$$
f(x)=f(x\oplus s)\qquad\text{for all }x.
$$

Extra collisions may bias the distribution or prevent collection of enough independent equations, but every observed $j$ still satisfies

$$
j\cdot s=0\pmod 2.
$$

This is the reusable move behind the Feistel distinguisher: if the vectors fail to span quickly, the algorithm can still guess “Feistel”; if they do span, the candidate period is verified by a fresh test.

### 3-round Feistel distinguisher

Let $W(a\|c)$ be the first $n$ output bits of $V(a\|c)$. Fix two distinct strings $\alpha,\beta\in\{0,1\}^n$ and define

$$
f:\{0,1\}^{n+1}\to\{0,1\}^n
$$

by

$$
f(b\|a)=
\begin{cases}
W(a\|\alpha)\oplus\alpha, & b=0,\\
W(a\|\beta)\oplus\beta, & b=1.
\end{cases}
$$

If $V$ is the Feistel construction, then

$$
f(0\|a)=P_2(a\oplus P_1(\alpha)),\qquad
f(1\|a)=P_2(a\oplus P_1(\beta)).
$$

Hence

$$
f(u)=f(u\oplus s)\quad\text{for all }u,
\qquad
s=1\|\big(P_1(\alpha)\oplus P_1(\beta)\big).
$$

The distinguisher is:

1. Build $U_f$ from at most two queries to $U_V$.
2. Run the Simon sampling circuit on $f$, collecting vectors $j\in\{0,1\}^{n+1}$.
3. Add samples until either $n$ independent equations have been collected, or $2n$ samples have been taken.
4. If $2n$ samples do not give rank $n$, guess Feistel.
5. Otherwise solve
   $$
   (j_1,\ldots,j_n)\cdot (s_1,
   \ldots,s_n)=j_0
   $$
   over $\mathbb F_2$ for the candidate $s=1\|s_1\cdots s_n$.
6. Pick random $u$ and test whether $f(u)=f(u\oplus s)$. If yes, guess Feistel; otherwise guess random permutation.

For a Feistel oracle the orthogonality condition is exact, and the final period test always passes. For a random permutation the samples behave as random vectors; failure to get rank $n$ after $2n$ samples has probability at most $2^{-n}$ by the row/column-rank argument in the footnote, and a random candidate period passes the final equality test only with negligible probability.

### CBC-MAC forgery

Model the block permutation as a random permutation $\pi$. Fix a target first block $\alpha_1$ and choose $\alpha_0\neq \alpha_1$. For $j=1,\ldots,\ell-1$, define

$$
g_j:\{0,1\}^{n+1}\to\{0,1\}^n
$$

by

$$
g_j(b\|x)=\operatorname{CBC}^{\ell}_{\pi}
(\alpha_b\|0^{(j-1)n}\|x\|0^{(\ell-j-1)n})
=
\pi^{\ell-j}(\pi^j(\alpha_b)\oplus x).
$$

Because $\pi$ is a permutation,

$$
g_j(u)=g_j(v)
\iff
v=u\oplus \big(1\|z_j\big),
\qquad
z_j=\pi^j(\alpha_0)\oplus\pi^j(\alpha_1).
$$

Run [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon’s algorithm]] on each $g_j$ to recover $z_j$ for $j=1,\ldots,\ell-1$. These are exact Simon instances, and each query to $g_j$ is a superposition over messages with a zero block; for $\ell\geq 3$, the eventual forgery message has no zero block in the positions used for the query restriction.

Then query classically

$$
t_0=\operatorname{CBC}^{\ell}_{\pi}(\alpha_0\|0^{(\ell-1)n})=\pi^\ell(\alpha_0),
$$

and

$$
t_1=\operatorname{CBC}^{\ell}_{\pi}(\alpha_1\|0^{(\ell-1)n})=\pi^\ell(\alpha_1).
$$

Output

$$
m=\alpha_1\|z_1\|z_2\|\cdots\|z_{\ell-1}
$$

with tag

$$
t=\begin{cases}
t_0, & \ell\text{ even},\\
t_1, & \ell\text{ odd}.
\end{cases}
$$

The proof is an induction using $z_j=\pi^j(\alpha_0)\oplus\pi^j(\alpha_1)$. The internal CBC value alternates between $\pi^r(\alpha_0)$ and $\pi^r(\alpha_1)$ as each $z_j$ cancels the previous branch and switches to the other one. Thus the final tag is $\pi^\ell(\alpha_0)$ for even $\ell$ and $\pi^\ell(\alpha_1)$ for odd $\ell$.

For a longer chosen prefix $\beta_1\|\cdots\|\beta_k$ with $k\leq \ell-2$, choose another prefix $\alpha_1\|\cdots\|\alpha_k$ so the induced CBC chaining states

$$
\alpha=\pi(\cdots\pi(\pi(\alpha_1)\oplus\alpha_2)\cdots)\oplus\alpha_k
$$

and

$$
\beta=\pi(\cdots\pi(\pi(\beta_1)\oplus\beta_2)\cdots)\oplus\beta_k
$$

are distinct. Then run the same attack on the effective shorter CBC instance of length $\ell-k+1$.

## Key results

### Simon lower and upper bounds used

For Simon’s problem on $n$ bits, the quantum query cost is

$$
O(n),
$$

while classical bounded-error query complexity is

$$
\Omega(2^{n/2}).
$$

### 3-round Feistel distinguisher

There is a polynomial-query quantum oracle distinguisher between a uniformly random permutation on $2n$ bits and a three-round Feistel network with arbitrary internal functions $P_1,P_2,P_3:\{0,1\}^n\to\{0,1\}^n$.

The distinguisher uses $O(n)$ Simon samples, each costing at most two calls to $U_V$, plus polynomial-time linear algebra over $\mathbb F_2$. Its error is negligible for a random permutation and zero in the Feistel case apart from the algorithm’s deliberate “rank failed” branch, which also outputs Feistel.

### CBC-MAC forgery

For every fixed length $\ell\geq 3$, basic CBC-MAC built from a quantum-secure pseudorandom permutation is not a secure MAC under coherent message queries in the sense considered here. There is a polynomial-query quantum adversary that, for a chosen prefix of length $k\leq \ell-2$, outputs a valid tag for a message with that prefix while querying only superpositions of other same-length messages.

The attack uses $\ell-1$ Simon runs in the one-block-prefix case, so the query count is

$$
O(\ell n)
$$

coherent CBC-MAC queries, plus two classical CBC-MAC queries and polynomial post-processing. The same reasoning applies to the length-prepending extension of CBC-MAC because the attack keeps all queried and forged messages at the same length.

## Comparison with prior work

| Work | Result | Relation |
|---|---|---|
| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] | Exponential oracle separation via hidden XOR periods | The direct algorithmic engine. |
| Luby--Rackoff (1988) | 3-round Feistel from PRFs gives a classical PRP | This paper shows the construction is not a quantum PRP with Q2 access. |
| Kuwakado--Morii (2010) | Simon distinguisher for 3-round Feistel with permutation round functions | Santoli--Schaffner remove the permutation-round assumption. |
| Kuwakado--Morii (2012) | Simon attack on quantum Even--Mansour | Same period-finding cryptanalysis family. |
| Boneh--Zhandry (2013) | Quantum-secure MAC definitions | Santoli--Schaffner use a different forgery notion: the final message is never queried, even inside a superposition. |
| [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes|Kaplan--Leurent--Leverrier--Naya-Plasencia (2016/2017)]] | Independent Simon-period attacks on symmetric constructions and Q1/Q2 accounting | Overlaps in message: coherent access can break constructions beyond Grover speedups. |
| [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes|Rötteler--Steinwandt (2013)]] | Simon reduction for XOR related-key attacks | Another clean Q2 model-separation result for symmetric cryptography. |

## Limits / caveats

- The Q2 oracle model is strong. A standard online MAC or block-cipher API does not let an attacker query arbitrary coherent superpositions of messages.
- The CBC-MAC attack is a query attack, not a fault-tolerant resource estimate for a named primitive.
- The Feistel distinguisher separates three rounds only. The paper leaves open whether a constant number larger than three of Feistel rounds gives a quantum-secure PRP from a quantum-secure PRF.
- For weak-period Simon sampling, extra collisions can stop the samples from spanning the full orthogonal space. The Feistel algorithm handles this for distinguishing, but it is not a generic recovery theorem.
- The CBC-MAC proof first replaces the PRP with a random permutation. Turning any observable difference in the attack into a PRP distinguisher is standard, but the statement depends on assuming the underlying PRP is quantum-secure.

## Reusable ideas

1. [[Weak-Period Simon Sampling for Distinguishers]] — use only the implication $f(x)=f(x\oplus s)$ to obtain equations orthogonal to $s$, then verify the candidate period separately.
2. [[Feistel Half-Output Period Test]] — derive a Simon period from two fixed right halves of a 3-round Feistel oracle by cancelling the visible branch value.
3. [[CBC-MAC Zero-Block Simon Forgery]] — query only messages with a zero block to learn chained-state XORs, then assemble an unqueried valid CBC-MAC message.

## References within this paper

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — algorithmic basis for all attacks in the paper.
- Montanaro--de Wolf (2016) — cited for the $\Omega(2^{n/2})$ classical query lower bound for Simon’s problem.
- Luby--Rackoff (1988) — classical security of three-round Feistel from pseudorandom functions.
- Kuwakado--Morii (2010) — quantum distinguisher for 3-round Feistel with internal permutations.
- Kuwakado--Morii (2012) — quantum Even--Mansour attack.
- Bellare--Kilian--Rogaway (2000) — classical CBC-MAC security.
- Boneh--Zhandry (2013) — quantum-secure MAC and signature definitions.
- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes|Kaplan--Leurent--Leverrier--Naya-Plasencia (2016/2017)]] — independent related Simon-period attacks on symmetric constructions.
- [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes|Rötteler--Steinwandt (2013)]] — neighbouring Simon-style related-key attack.
- Zhandry (2016) — existence of quantum-secure PRPs not based on Feistel networks.

## Cross-links

### Paper notes

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]]
- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes]]

### Trick cards

- [[Weak-Period Simon Sampling for Distinguishers]]
- [[Feistel Half-Output Period Test]]
- [[CBC-MAC Zero-Block Simon Forgery]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
- [[Simon Reduction for XOR Related-Key Oracles]]
