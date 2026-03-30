> **Source:** Mark Ettinger, Peter Høyer, Emanuel Knill, *The quantum query complexity of the hidden subgroup problem is polynomial*, Information Processing Letters **91**(1):43–48, 2004; arXiv:quant-ph/0401083
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0401083) · [Journal](https://doi.org/10.1016/j.ipl.2004.01.024)
> **Tags:** #hidden-subgroup-problem #query-complexity #nonabelian #information-theoretic #foundational

---

## The computational problem

**Hidden subgroup problem (HSP):** Given a finite group $G$ and an oracle $f: G \to S$ that is strictly $H$-periodic (constant on left cosets of $H \leq G$, distinct on distinct cosets), find a generating set for $H$.

The paper considers an arbitrary finite group $G$ with $n = \log|G|$ as the input size. The group has $r \leq 2^{O(n^2)}$ distinct subgroups. The oracle acts as $O_f: |g\rangle|0\rangle \mapsto |g\rangle|f(g)\rangle$.

---

## What the paper does

Proves that $O(\log^4 |G|)$ oracle queries suffice to determine the hidden subgroup of *any* finite group with exponentially small failure probability. The algorithm runs in exponential *time* (processing the coset states requires brute-force classical simulation), but uses only polynomially many queries.

This is the clean separation that explains why the non-abelian HSP is hard: **the information is available with few queries; extracting it from the quantum states is the bottleneck.** The coset states $\frac{1}{\sqrt{|H|}}\sum_{h \in H} |th\rangle$ collectively determine $H$, and $O(\text{poly}(\log |G|))$ copies suffice information-theoretically. The challenge — addressed by [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]], [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]], and [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam (2005)]] — is performing the right measurements and post-processing efficiently.

The result rules out query complexity lower bounds as a strategy for proving that the non-abelian HSP is computationally hard.

---

## The algorithm

### Stage 1: Bounded-error algorithm with exponentially small failure

Use $2 + 2s$ registers:
- Register 1: output register (subgroup index $\nu$, from $0$ to $r$)
- Register 2: counter $\ell$ (from $0$ to $r$)
- $s$ "couplet" blocks, each containing a **subgroup register** and a **function register**

**Initial state:**

$$|\Psi_{\text{init}}\rangle = |0\rangle|0\rangle \otimes \left(\frac{1}{\sqrt{N}} \sum_{g \in G} |g\rangle|f(g)\rangle\right)^{\otimes s}$$

This costs $s$ oracle calls. The key observation: each couplet is in the coset state mixture $\sum_{t \in T_H} |tH\rangle|f(t)\rangle$ (where $T_H$ is a transversal for $H$), regardless of $f$'s specific values.

**Testing operator:** Order all $r$ subgroups $K_1, \ldots, K_r$ by decreasing size. Define $\text{Test} = \text{Test}_r \cdots \text{Test}_2 \cdot \text{Test}_1$, where $\text{Test}_\mu$ checks whether $f$ is $K_\mu$-periodic by projecting the $s$ subgroup registers onto coset states of $K_\mu$.

For subgroup $K_\mu$ with transversal $T_\mu$, define the projector:

$$P_{s,\mu} = \left(\sum_{t \in T_\mu} |tK_\mu\rangle\langle tK_\mu| \otimes I\right)^{\otimes s}$$

Then $\text{Test}_\mu = Q_\mu \otimes P_{s,\mu} + I \otimes P_{s,\mu}^\perp$, where $Q_\mu$ records the result in the output register (first match wins, via the counter).

**Why it works (the two lemmas):**

- **Lemma 3:** If $f$ is $K_\mu$-periodic, then $\text{Test}_\mu$ correctly records $\mu$ in the output register (the $s$ couplets are exactly in the $+1$-eigenspace of $P_{s,\mu}$).
- **Lemma 4:** If $f$ is *not* $K_\mu$-periodic, then $\|\text{Test}_\mu|\Psi\rangle - |\Psi\rangle\| \leq 2/2^{s/2}$. This is because the overlap $|P_{1,\mu}|\text{coset state of } H\rangle|^2 = |K_\mu \cap H|/|K_\mu| \leq 1/2$ when $H \neq K_\mu$ (since $K_\mu \not\leq H$ and $|K_\mu| \geq |H|$ by the testing order). Over $s$ copies, this shrinks to $(1/2)^s$.

After all $r$ tests, the accumulated perturbation is at most $2r/2^{s/2}$ (Lemma 5). Setting $s = O(\log^2 |G|)$ — since $r \leq 2^{O(n^2)}$ — makes the error exponentially small. Total queries: $s = O(n^2)$.

### Stage 2: Making the algorithm exact

The bounded-error algorithm produces a conditional probability matrix $M$, where $M_{H,K_\mu} = \Pr[K_\mu | H]$. By construction, $M = I - \Delta$ where each entry of $\Delta$ has magnitude $\leq 1/r^2$. So $M^{-1} = I + \Gamma$ with small $\Gamma$.

Define $\text{ExactTest}$: run Test, then conditionally rotate an ancilla qubit by an angle determined by $x = M^{-1}y$, where $y$ encodes a binary partition of the subgroups into sets $Y_{3/4}$ and $Y_{1/4}$. This produces measurement probabilities of exactly $3/4$ or $1/4$ depending on which set $H$ belongs to.

Apply [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] to convert $3/4$ vs $1/4$ to $1$ vs $0$ (exact discrimination). Binary search over subgroup partitions identifies $H$ in $\lceil \log_2 r \rceil = O(n^2)$ rounds.

Total queries for exact version: $O(n^2) \times O(n^2) = O(n^4) = O(\log^4 |G|)$.

---

## Key results

**Theorem 1:** There exists a quantum algorithm using $O(\log^4 |G|)$ oracle calls that outputs a generating set for $H$ with failure probability exponentially small in $\log|G|$.

**Corollary (decision version):** For the decision problem "is $H$ non-trivial?":
- Exact query complexity: $Q_E(\text{HSP}_{\text{decision}}) = O(\log^2 |G|)$ (only one round of amplification needed)
- One-sided error: $Q_1(\text{HSP}_{\text{decision}}) = O(\log |G|)$ (test only cyclic subgroups, of which there are $\leq |G|$)

---

## The query-computation separation

This is the paper's real contribution — not the algorithm itself (which is exponential-time and unimplementable), but what it tells us about the structure of the HSP:

| Aspect | Complexity |
|---|---|
| **Query complexity** | $O(\text{poly}(\log |G|))$ — polynomial |
| **Computational complexity** (abelian $G$) | $O(\text{poly}(\log |G|))$ — efficient |
| **Computational complexity** (general $G$) | Unknown — likely exponential for some groups |
| **Computational complexity** (dihedral $G$) | $2^{O(\sqrt{\log N})}$ — subexponential |

The query complexity is never the bottleneck. For abelian groups, the QFT provides an efficient measurement. For non-abelian groups, the states carry enough information, but:
- The coset states have exponentially many components in the representation basis
- Distinguishing between exponentially many candidate subgroups requires processing these components
- The pretty good measurement is optimal ([[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam 2005]]) but implementing it efficiently is the open problem

---

## Limits / caveats

- The algorithm is completely impractical — it requires exponential time for the classical preprocessing (computing the conditional probability matrix $M$ and the rotation angles).
- It doesn't give any new algorithmic ideas for *efficiently* solving the non-abelian HSP. Its value is structural, not algorithmic.
- The exact version requires arbitrary one-qubit gates (exact angle rotations). In a restricted gate set model, the exact query complexity might be higher.
- The result doesn't extend to the **graph isomorphism** variant of HSP (over the symmetric group $S_n$), in the sense that it doesn't help solve GI efficiently. It only says the query barrier isn't the obstruction.

---

## Reusable ideas

1. **[[Sequential Periodicity Testing via Projective Filtering]]:** Test $r$ candidate subgroups sequentially by projecting onto coset states of each, ordered by decreasing size. If the correct subgroup is $H$, the projection onto $H$-coset states succeeds with certainty, while projections onto non-subgroups perturb the state by only $O(2^{-s/2})$ per test (where $s$ = number of copies). The ordering guarantees that larger subgroups are tested first, preventing false positives from proper subgroups of $H$.

2. **[[Error-to-Exact Conversion via Conditional Rotation]]:** Given a bounded-error algorithm whose success probability depends on the hidden answer, compute the probability matrix $M$ offline, invert it, and use $M^{-1}$ to define conditional rotations that map success probabilities to exact values ($3/4$ or $1/4$). Then [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude amplification]] converts these to $0/1$. Binary search over answer partitions identifies the answer exactly.

---

## References within this paper

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — HSP for $\mathbb{Z}_2^n$, the original quantum speedup via HSP
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — HSP for abelian groups (factoring = HSP over $\mathbb{Z}$)
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — abelian HSP via phase estimation
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard, Høyer, Mosca & Tapp (2002)]] — amplitude amplification used for exact conversion
- Ettinger & Høyer (2000) — earlier work on non-commutative HSP
- Grigni, Schulman, Vazirani & Vazirani (2001) — non-abelian HSP algorithms
- Hallgren, Russell & Ta-Shma (2000) — HSP via group representations

---

## Cross-links

### Paper notes
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]] — addresses the computational complexity that this paper shows is the real barrier
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]] — polynomial-space variant for dihedral groups
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]] — optimal measurements for semidirect product HSP
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — efficient HSP for solvable groups
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — abelian HSP as a special case
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]] — dihedral HSP ↔ lattice problems connection

### Trick cards
- [[Sequential Periodicity Testing via Projective Filtering]]
- [[Error-to-Exact Conversion via Conditional Rotation]]
- [[Standard Amplitude Amplification]]
