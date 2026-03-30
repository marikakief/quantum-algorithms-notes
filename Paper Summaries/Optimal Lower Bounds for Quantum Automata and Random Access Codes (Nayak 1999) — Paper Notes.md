> **Source:** Ashwin Nayak, "Optimal lower bounds for quantum automata and random access codes", *FOCS 1999*
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9904093)
> **Tags:** #quantum-information #lower-bounds #random-access-codes #holevo-bound #quantum-automata #entropy

---

## The computational problem

**Quantum random access codes (QRACs):** An $(n, m, p)$-random access code is a function mapping $n$-bit strings to mixed states over $m$ qubits such that for every index $i \in \{1, \ldots, n\}$, there exists a measurement $O_i$ recovering $x_i$ with probability at least $p$ for all $x \in \{0,1\}^n$.

**Quantum finite automata (QFAs):** Consider the finite regular language $L_n = \{w0 \mid w \in \{0,1\}^*, |w| \leq n\}$, recognisable by a classical DFA of size $O(n)$. How many states does a QFA need?

## What the paper does

Proves two clean, optimal lower bounds using entropy arguments based on Holevo's theorem:

1. **Nayak's bound for random access codes:** Any $(n, m, p)$-QRAC satisfies $m \geq (1 - H(p))n$, where $H$ is the binary entropy function. This is tight up to an additive $O(\log n)$ term.

2. **QFA lower bound:** Any QFA (even with intermediate measurements) recognising $L_n$ with probability $p > 1/2$ has $2^{\Omega(n)}$ states.

The QRAC bound resolves an open question from Ambainis, Nayak, Ta-Shma, Vazirani (1998), who introduced random access codes and showed only a logarithmic compression was achievable. Nayak's bound makes this precise: quantum encoding offers **no asymptotic advantage** over classical encoding for random access.

## The key lemma

The entire paper rests on one lemma, which is a direct corollary of Holevo's theorem:

**Lemma 4.1:** Let $\sigma_0, \sigma_1$ be two density matrices and $\sigma = \frac{1}{2}(\sigma_0 + \sigma_1)$ their equal mixture. If a measurement $O$ distinguishes them correctly with average probability $p$, then:

$$S(\sigma) \geq \frac{1}{2}[S(\sigma_0) + S(\sigma_1)] + (1 - H(p))$$

That is, mixing two distinguishable states increases the von Neumann entropy by at least $1 - H(p)$ bits beyond the average entropy of the components.

**Proof:** By Holevo's theorem, the mutual information $I(X:Y) \leq S(\sigma) - \frac{1}{2}[S(\sigma_0) + S(\sigma_1)]$. Since $\Pr[Y = X] = p$ for an unbiased bit $X$, Fano's inequality gives $I(X:Y) \geq 1 - H(p)$.

## The QRAC lower bound

For a random access code with parameters $(n, m, p)$:

Let $\rho_y = \frac{1}{2^{n-k}} \sum_{z \in \{0,1\}^{n-k}} \rho_{zy}$ for $y \in \{0,1\}^k$. Then by downward induction on $k$:

$$S(\rho_y) \geq (1 - H(p))(n - k)$$

**Base case:** $k = n$, so $\rho_y$ is the encoding of a single string, and $S(\rho_y) \geq 0$.

**Inductive step:** $\rho_y = \frac{1}{2}(\rho_{0y} + \rho_{1y})$, and measurement $O_{n-k}$ distinguishes $\rho_{0y}$ from $\rho_{1y}$ with probability $p$. Apply Lemma 4.1.

Setting $y$ to the empty string: $S(\rho) \geq (1 - H(p))n$. Since $\rho$ has support in a $2^m$-dimensional space, $S(\rho) \leq m$. Therefore $m \geq (1 - H(p))n$.

The argument extends immediately to **serial codes** (where measurement $O_i$ may depend on subsequent bits $x_{i+1}, \ldots, x_n$).

## The QFA lower bound

For an $n$-restricted QFA for $L_n$ with acceptance probability $p$:

Consider the QFA state $\rho_k$ after reading $k$ random input bits. After each bit $b \in \{0,1\}$:

$$\rho_k = \frac{1}{2}(U_0 \rho_{k-1} U_0^\dagger + U_1 \rho_{k-1} U_1^\dagger)$$

The two states $U_b \rho_{k-1} U_b^\dagger$ are distinguishable with probability $p$ (by applying the end-marker unitary $U_{\text{end}}$ and measuring acceptance). By Lemma 4.1 and the fact that unitary evolution preserves entropy (Fact 3.2):

$$S(\rho_k) \geq S(\rho_{k-1}) + (1 - H(p))$$

So $S(\rho_n) \geq (1 - H(p))n$. Since $S(\rho_n) \leq \log|Q|$, the QFA needs $|Q| \geq 2^{(1-H(p))n}$ states.

The bound extends to **enhanced QFA** (with intermediate measurements) because measurements can only increase entropy (Fact 3.3).

## An alternative to Holevo's theorem

The paper also gives a tighter, more transparent in-probability bound:

**Theorem 2.4:** For any encoding of a random variable $X$ into $m$ qubits, any decoding function $D$ satisfies:

$$\Pr[D(Y) = X] \leq P(X, 2^m)$$

where $P(X, 2^m)$ is the probability mass of the $2^m$ most likely values of $X$. When $X$ is uniform over $2^n$ strings, this gives $\Pr[\text{correct}] \leq 2^{m-n}$.

**Proof:** Uses a clean dimension-counting argument. The codewords $|\phi_x\rangle$ span a subspace $E$ of dimension at most $2^m$. Any decoding measurement $\{P_x\}$ satisfies $\sum_x \|P_x |\phi_x\rangle\|^2 \leq 2^m$ because $\sum_x \|P_x |\phi_x\rangle\|^2 \leq \sum_i \sum_{x,j} |\langle e_i | \hat{e}_{x,j} \rangle|^2 = \sum_i \|e_i\|^2 \leq 2^m$.

## Key results

| Result | Bound |
|---|---|
| QRAC bound | $m \geq (1 - H(p))n$ for $(n,m,p)$-QRAC |
| QFA for $L_n$ | $\|Q\| \geq 2^{(1-H(p))n}$ states |
| Enhanced QFA | Same bound (measurements don't help) |
| In-probability decoding | $\Pr[\text{correct}] \leq P(X, 2^m)$ |
| Corollary: non-regular languages | Enhanced QFA accept only a strict subset of regular languages |

## Comparison with prior work

| Result | Prior | This paper |
|---|---|---|
| QFA lower bound for $L_n$ | $2^{\Omega(n/\log n)}$ (Ambainis-Nayak-Ta-Shma-Vazirani 1998) | $2^{\Omega(n)}$ — tight |
| QRAC compression | $O(\log n)$ factor (ANTV 1998) | $(1 - H(p))n$ — tight to $O(\log n)$ |
| Holevo bound applications | Via entropy chain + Fano | Direct in-probability bound, sometimes tighter |

## Limits / caveats

- Nayak's bound applies to *worst-case* recovery probability $p$ across all bits. If only average-case recovery is required, better compression may be possible.
- The QFA results are for *one-way* automata only. Two-way quantum automata have much greater power.
- The in-probability bound (Theorem 2.4) does not generalise to entanglement-assisted communication, unlike Holevo's theorem.
- For QRACs with $p = 1$ (perfect recovery), the bound gives $m \geq n$ — you can't compress at all. The interesting regime is $p$ slightly above $1/2$.

## Reusable ideas

1. [[Entropy Accumulation via Holevo's Theorem]] — each distinguishable quantum operation on a system increases the von Neumann entropy by at least $1 - H(p)$; this "entropy ratchet" converts distinguishability into dimension lower bounds
2. [[Dimension Counting for Quantum Decoding]] — bounding decoding probability by the dimension of the codespace via $\sum_x \|P_x |\phi_x\rangle\|^2 \leq \dim(E)$; cleaner than going through mutual information in many applications

## References within this paper

- Holevo, "Some estimates of the information transmitted by quantum communication channels", 1973 — the foundational theorem
- Ambainis, Nayak, Ta-Shma, Vazirani, "Dense quantum coding and quantum finite automata", STOC 1998 — introduced QRACs, proved $2^{\Omega(n/\log n)}$ QFA bound
- Kondacs-Watrous, "On the power of quantum finite state automata", FOCS 1997 — defined QFA model
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett-Bernstein-Brassard-Vazirani (1997)]] — variational distance lemma used in QFA analysis
- Kremer, "Quantum communication", 1995 — lemma connecting oblivious protocols to fixed subspaces

## Cross-links

### Paper notes
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — the variational distance lemma
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]] — quantum communication advantage also using Holevo-type arguments
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes]] — uses Nayak's bound via [[Fat-Shattering Dimension of Quantum States via Random Access Codes|fat-shattering connection]]
- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]] — quantum Shannon theory context
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]] — communication complexity setting where Nayak's bound applies

### Trick cards
- [[Entropy Accumulation via Holevo's Theorem]]
- [[Dimension Counting for Quantum Decoding]]
- [[Fat-Shattering Dimension of Quantum States via Random Access Codes]] — applies Nayak's bound to quantum learning theory
