# Quantum Attacks Against Iterated Block Ciphers (Kaplan 2015) — Paper Notes

> **Source:** Marc Kaplan, *Quantum attacks against iterated block ciphers*, arXiv:1410.1434v2 (2015)
> **Links:** [arXiv](https://arxiv.org/abs/1410.1434) · [PDF](https://arxiv.org/pdf/1410.1434)
> **Tags:** #quantum-cryptanalysis #symmetric-key #block-ciphers #quantum-walks #query-complexity #element-distinctness

---

## The computational problem

The paper studies black-box key extraction from iterated ideal block ciphers. A block cipher is modelled as a public family of random permutations

$$
F = \{F_1,\ldots,F_N\},\qquad F_i\in S[M],
$$

where $[N]$ is the key space and $[M]$ is the block space. The attacker may query $F_i(X)$ and $F_i^{-1}(X)$ quantumly.

### Two-encryption key extraction

Given plaintext/ciphertext values $P,C\in[M]$, the $2$-key extraction problem $\mathrm{KE}^{P,C}_2$ is:

- **Input:** oracle access to $F=\{F_1,\ldots,F_N\}$;
- **Promise:** there is a unique pair $(k_1,k_2)$ such that
  $$
  F_{k_2}(F_{k_1}(P))=C;
  $$
- **Output:** the pair $(k_1,k_2)$.

The decision version $d\text{-}\mathrm{KE}_2$ asks whether such keys exist. The paper also uses claw finding:

$$
G_1(x)=F_x(P),\qquad G_2(y)=F_y^{-1}(C),
$$

so the secret keys are exactly the unique claw $G_1(k_1)=G_2(k_2)$.

### Four-encryption key extraction

Given four plaintext/ciphertext pairs $P=(P_1,\ldots,P_4)$ and $C=(C_1,\ldots,C_4)$, the $4$-key extraction problem $\mathrm{KE}^{P,C}_4$ asks for $(k_1,k_2,k_3,k_4)$ satisfying

$$
F_{k_4}(F_{k_3}(F_{k_2}(F_{k_1}(P_i))))=C_i
$$

for all $i=1,\ldots,4$. The four pairs are used so that, with high probability, the consistent quadruple is unique while $M$ and $N$ remain comparable.

Complexity measures are quantum query complexity, time, memory, and time-space product.

## What the paper does

Kaplan gives the first clean quantum accounting for generic attacks on iterated block ciphers. For double encryption, the natural quantum meet-in-the-middle attack costs $\Theta(N^{2/3})$ queries and is optimal by a generalized-adversary lower bound. That is the main theorem: quantumly, double encryption really is harder than single encryption, unlike the classical MITM story.

For four encryptions, the paper quantises the Dinur--Dunkelman--Keller--Shamir dissection idea by walking over the middle value after two encryptions. The resulting attack runs in $O(N^{7/6})$ time and $O(N^{2/3})$ memory, but no matching lower bound is known. My assessment: the $2$-encryption part is the durable result; the $4$-encryption part is best read as a proof that quantum composition effects in symmetric cryptanalysis are subtle, not as a final security estimate.

## The algorithm / construction

### 1. Quantum meet-in-the-middle for two encryption

Given $(P,C)$, define two injective functions

$$
G_1(x)=F_x(P),\qquad G_2(y)=F_y^{-1}(C).
$$

A claw $(x,y)$ with $G_1(x)=G_2(y)$ is exactly a key pair satisfying

$$
F_y(F_x(P))=C.
$$

Then run a claw-finding / [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|element-distinctness]] quantum walk. This gives:

1. prepare a data structure containing a subset of candidate keys and their forward/backward middle values;
2. use Ambainis-style updates to replace sampled keys coherently;
3. check whether the stored forward and backward lists contain a collision;
4. output the colliding pair $(k_1,k_2)$.

This is the reusable construction in [[Quantum Meet-in-the-Middle as Claw Finding]]. It costs $O(N^{2/3})$ time/queries and uses $O(N^{2/3})$ memory.

A low-memory alternative is Grover search over $(k_1,k_2)$, costing $O(N)$ time and logarithmic memory. So the time-optimal quantum attack has worse time-space product than the slower low-memory attack:

$$
T_{\rm MITM}=O(N^{2/3}),\quad S_{\rm MITM}=O(N^{2/3}),\quad TS=O(N^{4/3}),
$$

while Grover gives $T=O(N)$ and $S=\operatorname{polylog}(N)$.

### 2. Lower bound for two encryption

The obstacle is that $\mathrm{KE}_2$ is not literally claw finding: the attacker can query arbitrary values $F_k(X)$ and inverse values $F_k^{-1}(X)$, not only $F_k(P)$ and $F_k^{-1}(C)$. Kaplan handles this with the generalized adversary method.

The proof starts with an optimal adversary matrix $\Gamma_{\rm CF}$ for decision claw finding. For an input $u=\{F_i\}$ to $d\text{-}\mathrm{KE}_2$, project it to the claw-finding input

$$
u^{(P,C)}=(G_1,G_2),\qquad G_1(k)=F_k(P),\quad G_2(k)=F_k^{-1}(C).
$$

Define

$$
\Gamma_{\rm KE2}[u,v]=\Gamma_{\rm CF}[u^{(P,C)},v^{(P,C)}].
$$

Inputs with the same projection form fibres of equal size $D$, giving a tensor-product form

$$
\Gamma_{\rm KE2}=\Gamma_{\rm CF}\otimes J_{D\times D}.
$$

The key technical step is to show that arbitrary queries $(X,k,b)$, with $b=1$ for $F_k$ and $b=-1$ for $F_k^{-1}$, do no better than the projected queries $(P,k,1)$ or $(C,k,-1)$ after a row/column permutation of the adversary matrix. The ratio

$$
\frac{\|\Gamma\|}{\max_q\|\Gamma\circ\Delta_q\|}
$$

therefore matches the claw-finding adversary ratio up to the fibre factor $D$, which cancels. This yields the $\Omega(N^{2/3})$ lower bound. The reusable proof pattern is [[Projected Adversary Matrix Lower Bound for Structured Oracles]].

Kaplan then upgrades worst-case decision lower bounds to random-input search lower bounds using a self-reduction and Markov-style stopping argument: an algorithm that solved random instances too often with $o(N^{2/3})$ queries would imply an average-case decision algorithm contradicting the adversary lower bound.

### 3. Quantum dissection attack for four encryption

For $4$-encryption, write $X$ for the middle value after two encryptions:

$$
X = F_{k_2}(F_{k_1}(P_i)).
$$

For a candidate $X$, define two decision predicates:

$$
f_X(F)=1 \iff \exists k_1,k_2: F_{k_2}(F_{k_1}(P))=X,
$$

$$
g_X(F)=1 \iff \exists k_3,k_4: F_{k_4}(F_{k_3}(X))=C.
$$

Then the decision problem can be written informally as

$$
d\text{-}\mathrm{KE}_4 = \mathrm{SEARCH}\circ (f_0\wedge g_0, f_1\wedge g_1,\ldots,f_M\wedge g_M).
$$

The algorithm is a quantum walk on the complete graph whose vertices are possible middle values $X\in[M]$:

1. prepare the uniform superposition over candidate $X$;
2. use the complete-graph walk, whose spectral gap is $\delta=1-1/(M-1)$;
3. check whether $X$ is marked by running the quantum MITM procedure on the left half and right half;
4. once a marked $X$ is found, recover the four keys by the two MITM subroutines.

The MNRS search bound gives cost

$$
S+\frac{1}{\sqrt{\varepsilon}}\left(\frac{1}{\sqrt{\delta}}U+C\right),
$$

with setup $S=O(1)$, update $U=O(1)$, marked fraction $\varepsilon=1/M$, and checking cost $C=O(N^{2/3})$. When $M$ and $N$ are comparable, this is

$$
O(M^{1/2}N^{2/3})=O(N^{7/6})
$$

time and queries, with $O(N^{2/3})$ memory. See [[Middle-Value Quantum Walk for Iterated-Cipher Dissection]].

## Key results

### Element distinctness and claw finding baseline

For $M\ge N$, element distinctness and claw finding have quantum query and time complexity

$$
\Theta(N^{2/3}),
$$

and the time-efficient algorithm uses memory $O(N^{2/3})$.

### Theorem 2: upper bound for two-key extraction

There is a quantum algorithm solving $\mathrm{KE}_2$ in

$$
T=O(N^{2/3}),\qquad S=O(N^{2/3}).
$$

It is the reduction to claw finding via $G_1(x)=F_x(P)$ and $G_2(y)=F_y^{-1}(C)$.

### Theorem 3: lower bound for two-key extraction

Any quantum algorithm solving $\mathrm{KE}_2$ needs

$$
\Omega(N^{2/3})
$$

quantum queries to $F=\{F_1,\ldots,F_N\}$, including inverse-permutation queries, except with vanishing probability on random inputs.

### Corollary 1: time-space product for the fastest two-encryption attack

The time-efficient quantum MITM attack has

$$
T=O(N^{2/3}),\qquad S=O(N^{2/3}),\qquad TS=O(N^{4/3}).
$$

A slower Grover attack has $T=O(N)$ and only logarithmic memory, so the best time and best time-space tradeoff need not coincide.

### Theorem 4: four-key extraction

Assuming enough plaintext/ciphertext pairs to make the key quadruple unique and $M\asymp N$, there is a quantum algorithm for $\mathrm{KE}_4$ with

$$
T=O(N^{7/6}),\qquad S=O(N^{2/3}),\qquad TS=O(N^{11/6}).
$$

The paper does not prove this is optimal.

## Comparison with prior work

| Work / attack | Classical cost | Quantum cost in this paper | Comment |
|---|---:|---:|---|
| Single ideal block cipher | $O(N)$ | $O(N^{1/2})$ | [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover]] baseline. |
| Double encryption, exhaustive search over pairs | $O(N^2)$ | $O(N)$ | Low memory, not time-optimal quantumly. |
| Double encryption, classical MITM | $O(N)$ time, $O(N)$ memory | — | Classical composition gives almost no time amplification. |
| Double encryption, quantum MITM | — | $\Theta(N^{2/3})$ time and queries | Optimal; reduction to claw finding plus adversary lower bound. |
| Four encryption, pairwise MITM | $O(N^2)$ time, $O(N^2)$ memory | $O(N^{4/3})$ time if treated as double encryption over key-pairs | Poorer than dissection. |
| Four encryption, DDKS dissection | $O(N^2)$ time, $O(N)$ memory | $O(N^{7/6})$ time, $O(N^{2/3})$ memory | Quantum walk over the middle value. |

The nearest follow-on in this vault is [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]], which takes the same lesson into concrete differential, truncated differential, and linear cryptanalysis: the best classical attack need not quantise to the best quantum attack.

## Limits / caveats

- The model is ideal-cipher / black-box. It does not analyse concrete circuit cost for AES-style permutations; for that, see [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]].
- The $2$-encryption result is tight for time/query complexity, but the memory tradeoff is not fully characterised. The fastest algorithm has worse time-space product than low-memory Grover.
- The $4$-encryption quantum dissection attack has no matching lower bound. The paper explicitly says known composition theorems do not apply because the inner predicates share the same oracle input.
- The result assumes enough known plaintext/ciphertext data to make the key tuple unique. For $4$-encryption, this is why the problem definition uses four pairs.
- Constants and implementation overhead are suppressed. This is a query-complexity paper, not a fault-tolerant resource estimate.
- The paper does not solve the general $r$-encryption case. The dissection idea can be iterated classically, but the quantum-walk and adversary-composition tools used here do not scale cleanly with $r$.

## Reusable ideas

1. [[Quantum Meet-in-the-Middle as Claw Finding]] — turn a double-composition key-recovery problem into claw finding between forward and backward middle-value maps.
2. [[Projected Adversary Matrix Lower Bound for Structured Oracles]] — prove a lower bound for a richer oracle by projecting each structured input to the hard subproblem and showing extra query locations do not improve the adversary ratio.
3. [[Middle-Value Quantum Walk for Iterated-Cipher Dissection]] — quantise dissection by walking over candidate middle states and using a quantum MITM checker.

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — baseline quantum exhaustive search.
- Brassard--Høyer--Tapp (1998), *Quantum cryptanalysis of hash and claw-free functions* — early claw/collision-finding cryptanalysis; Zoo #315, no local note in this batch.
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — element-distinctness walk used for the quantum MITM upper bound.
- Aaronson--Shi (2004) — element-distinctness and collision lower bounds.
- Buhrman--Dürr--Heiligman--Høyer--Magniez--Santha--de Wolf (2005), *Quantum algorithms for element distinctness* — alternative element-distinctness algorithms and time-space tradeoffs; local note exists as [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]].
- Høyer--Lee--Špalek (2007), *Negative weights make adversaries stronger* — generalized adversary method.
- Lee--Mittal--Reichardt--Špalek--Szegedy (2011), *Quantum query complexity of state conversion* — tightness of the general adversary bound.
- Magniez--Nayak--Roland--Santha (2011), *Search via quantum walk* — MNRS search theorem used for the $4$-encryption attack.
- Dinur--Dunkelman--Keller--Shamir (2012), *Efficient dissection of composite problems* — classical dissection attack quantised in Section 3.
- Diffie--Hellman (1977) and Merkle--Hellman (1981) — classical MITM background for double DES.
- Zhandry (2013), *A note on the quantum collision and set equality problems* — quantum collision/set-equality context.

## Cross-links

### Paper notes

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes]]
- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]

### Trick cards

- [[Quantum Meet-in-the-Middle as Claw Finding]]
- [[Projected Adversary Matrix Lower Bound for Structured Oracles]]
- [[Middle-Value Quantum Walk for Iterated-Cipher Dissection]]
