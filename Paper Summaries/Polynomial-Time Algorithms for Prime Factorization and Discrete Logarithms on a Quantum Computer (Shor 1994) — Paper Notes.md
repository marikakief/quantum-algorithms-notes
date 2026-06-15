> **Source:** Peter W. Shor, *Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer*, SIAM J. Comput. **26**(5), 1484–1509 (1997); Proc. 35th FOCS, 124–134 (1994); arXiv:quant-ph/9508027
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9508027) · [SIAM](https://doi.org/10.1137/S0097539795293172)
> **Tags:** #factoring #discrete-log #QFT #period-finding #cryptography #foundational

---

## The computational problems

**Notation used in this note:** $N$ is the integer to be factored, $m=\lceil \log_2 N\rceil$ is the input bit length, $q=2^\ell$ is the Fourier modulus with $N^2 \le q < 2N^2$ and $\ell=O(m)$, and $r$ is the multiplicative order. Shor's paper often writes the integer itself as $n$; asymptotics below are translated to the bit length $m$ unless explicitly stated otherwise.

**Factoring:** Given an integer $N$, find its prime factorisation. Best classical: number field sieve, time $\exp(O((\log N)^{1/3}(\log\log N)^{2/3}))$ — sub-exponential but super-polynomial.

**Discrete logarithm:** Given a prime $p$, a generator $g$ of $(\mathbb{Z}/p\mathbb{Z})^*$, and $x$, find $r$ such that $g^r \equiv x \pmod{p}$. Best classical: Gordon's number field sieve adaptation, same asymptotic cost.

---

## What the paper does

Gives polynomial-time quantum algorithms for both problems. In bit-length notation, Shor's stated asymptotic for the quantum part of factoring is $O(m^2 \log m \log\log m)$ steps using fast multiplication, plus polynomial-time classical post-processing. This is **exponentially faster** than any known classical algorithm, and would break RSA and discrete-log-based public-key cryptosystems at cryptographic sizes if implemented fault-tolerantly at scale.

This is the paper that launched quantum computing as a serious field. Before Shor, quantum speedups were either polynomial (Deutsch-Jozsa, Bernstein-Vazirani) or exponential but for oracle problems ([[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon]]). Shor showed an exponential speedup for a concrete, important, classical problem.

---

## The reduction: factoring → order-finding

The quantum algorithm doesn't factor directly. It finds the **order** of a random element $x$ modulo $N$.

**The number theory (all classical):**
1. Pick random $x$ with $1 < x < N$, $\gcd(x, N) = 1$
2. Find $r = \mathrm{ord}(x \bmod N)$, the smallest $r$ with $x^r \equiv 1 \pmod{N}$
3. If $r$ is even, compute $\gcd(x^{r/2} \pm 1, N)$
4. With probability $\geq 1 - 1/2^{k-1}$ (where $k$ = number of distinct odd prime factors of $N$), this gives a non-trivial factor

**Why it works:** By the Chinese Remainder Theorem, choosing $x \bmod N$ randomly is equivalent to choosing $x_i \bmod p_i^{a_i}$ independently. The orders $r_i = \mathrm{ord}(x \bmod p_i^{a_i})$ combine as $r = \mathrm{lcm}(r_i)$. The algorithm fails only if the largest powers of 2 dividing each $r_i$ all agree — this happens with probability $\leq 1/2^{k-1}$.

The hard step is finding $r$. Classically, this requires computing $x, x^2, x^3, \ldots$ until a repeat — exponential time. Quantumly, this is where the QFT comes in.

---

## The quantum order-finding algorithm

### Setup

Choose $q = 2^\ell$ with $N^2 \leq q < 2N^2$. Use two quantum registers.

### The circuit

**Step 1:** Prepare uniform superposition in register 1:

$$
\frac{1}{\sqrt{q}} \sum_{a=0}^{q-1} |a\rangle|0\rangle
$$

**Step 2:** Compute $x^a \bmod N$ into register 2:

$$
\frac{1}{\sqrt{q}} \sum_{a=0}^{q-1} |a\rangle|x^a \bmod N\rangle
$$

This is the expensive step: reversible modular exponentiation using $O(m^3)$ gates with schoolbook multiplication, or $O(m^2 \log m \log\log m)$ with fast multiplication in Shor's asymptotic accounting.

**Step 3:** Apply the quantum Fourier transform $A_q$ to register 1:

$$
\frac{1}{q} \sum_{a=0}^{q-1} \sum_{c=0}^{q-1} e^{2\pi i ac/q} |c\rangle|x^a \bmod N\rangle
$$

**Step 4:** Measure both registers. Observe $(c, x^k \bmod N)$ for some $0 \leq k < r$.

### The probability analysis

The probability of observing $(c, x^k \bmod N)$ is:

$$
\left|\frac{1}{q} \sum_{\substack{a \geq 0 \\ a \equiv k \pmod{r}}} e^{2\pi i ac/q}\right|^2
$$

Writing $a = br + k$ and simplifying: the probability is large when $|\{rc\}_q| \leq r/2$, i.e., when $c/q$ is close to some $d/r$. Specifically, if $|c/q - d/r| \leq 1/(2q)$, the probability is $\geq 1/(3r^2)$.

### Extracting $r$ from $c$

Since $q > N^2 > r^2$, there's **at most one** fraction $d/r$ with $r < N$ satisfying $|c/q - d/r| \leq 1/(2q)$. Find it by computing the continued fraction expansion of $c/q$ — this yields $d/r$ in lowest terms in polynomial time.

If $\gcd(d, r) = 1$, we recover $r$ directly. The probability that a random $d \in \{0, \ldots, r-1\}$ is coprime to $r$ is $\phi(r)/r > \delta/\log\log r$.

**Success probability per order-finding experiment:** $\geq \phi(r)/(3r) = \Omega(1/\log\log r)=\Omega(1/\log m)$.

The direct proof gives high success probability after $O(\log\log r)=O(\log m)$ repetitions. Shor also describes classical post-processing tricks — trying nearby $c$ values, testing small multiples of the reduced denominator, and combining candidate orders by lcm — that can reduce the expected number of quantum experiments to a constant in the intended accounting.

---

## The quantum Fourier transform

The QFT over $\mathbb{Z}_q$ with $q = 2^\ell$:

$$
|a\rangle \mapsto \frac{1}{\sqrt{q}} \sum_{c=0}^{q-1} e^{2\pi i ac/q}|c\rangle
$$

**Implementation:** $l(l-1)/2$ two-qubit gates plus $l$ single-qubit gates:

- $R_j$: Hadamard on qubit $j$
- $S_{j,k}$: controlled phase $e^{i\pi/2^{k-j}}$ on qubits $j,k$

Applied in order: $R_{l-1}, S_{l-2,l-1}, R_{l-2}, S_{l-3,l-1}, S_{l-3,l-2}, R_{l-3}, \ldots, R_0$

Output is bit-reversed (reverse qubit order or read backwards).

**Gate count:** $O(m^2)$ for exact QFT on $\ell=O(m)$ qubits. Coppersmith's approximate QFT drops the small phases $S_{j,k}$ for large $k - j$, reducing to $O(m \log m)$ gates with negligible error.

---

## Discrete logarithms

Uses three registers. Given $g, x, p$ with $g^r \equiv x \pmod{p}$:

1. Prepare uniform superposition $\frac{1}{p-1}\sum_{a,b=0}^{p-2} |a\rangle|b\rangle|g^a x^{-b} \bmod p\rangle$
2. QFT both registers: $|a\rangle \to |c\rangle$, $|b\rangle \to |d\rangle$
3. Measure $(c, d, g^k \bmod p)$
4. From a "good" pair $(c, d)$: recover $r$ from $d/q + r \cdot (\text{known function of } c)/(p-1)$

After a constant number of trials ($\sim 480t$ experiments for constant $t$), recover $r$ with high probability via Chinese Remainder Theorem.

**Key difference from factoring:** two QFTs instead of one, and the analysis is more involved (conditions (6.10) and (6.11) in the paper). But the structure is the same: Fourier sampling recovers hidden linear structure.

---

## Reversible modular exponentiation

The most space/time-consuming subroutine. Shor's implementation:

1. Compute $x^{2^i} \bmod N$ classically for all $i < l$ (build into circuit structure)
2. Controlled multiplication: if $a_i = 1$, multiply running product by $x^{2^i} \bmod N$
3. Clean up garbage via Bennett's compute-copy-uncompute: compute $(b, bc \bmod N)$, then use $c^{-1} \bmod N$ to erase $b$

**Gate count:** $O(m^3)$ using schoolbook multiplication (each of $O(m)$ multiplications costs $O(m^2)$ gates). With Schönhage-Strassen-style fast multiplication in Shor's asymptotic estimate: $O(m^2 \log m \log\log m)$.

**Optional watchdog measurement:** After garbage cleanup, $b$ should be 0. Shor observes that one could measure $b$ and restart if it is nonzero; conditioned on observing 0, amplitude on nonzero-$b$ error states is projected away. He explicitly presents this as a possible stabilizing idea whose benefit depends on the error model and measurement cost, not as a full fault-tolerant error-correction scheme.

---

## Complexity summary

| Component | Gate count / overhead in bit length $m=\lceil\log_2 N\rceil$ |
|---|---|
| QFT (exact) | $O(m^2)$ |
| QFT (approximate, Coppersmith) | $O(m \log m)$ |
| One modular exponentiation (schoolbook) | $O(m^3)$ |
| One modular exponentiation (fast multiplication) | $O(m^2 \log m \log\log m)$ |
| One order-finding experiment | dominated by modular exponentiation; QFT is lower order |
| Direct high-probability repetition overhead | $O(\log\log r)=O(\log m)$ experiments |
| Shor's stated factoring asymptotic | $O(m^2 \log m \log\log m)$ quantum steps with the post-processing improvements discussed in the paper |
| Classical post-processing (continued fractions, GCD, candidate checks) | polynomial in $m$ |

**Space:** $O(m)$ qubits for the two registers plus workspace in the schoolbook modular-exponentiation construction; the fast-multiplication asymptotic uses larger $O(m \log m \log\log m)$ workspace in Shor's estimate.

---

## Historical and conceptual significance

This paper is where quantum computing became real — not as hardware, but as a field. Before Shor:
- Deutsch-Jozsa and Bernstein-Vazirani gave oracle separations (interesting but not practically motivated)
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] gave an exponential oracle separation that directly inspired Shor
- There was no exponential quantum speedup for a natural, non-oracle problem

After Shor: billions of dollars in quantum computing investment, post-quantum cryptography standardisation (NIST), and a generation of researchers entering the field.

**The Simon → Shor pipeline:** Shor explicitly acknowledges Simon's influence. The structure is similar — prepare superposition, evaluate a periodic function, Fourier transform, extract period information from samples — but not literally identical. Simon has exact coset sampling over $\mathbb{Z}_2^n$; Shor uses a power-of-two Fourier modulus $q$ with $N^2 \le q < 2N^2$, so the period $r$ need not divide the register size and the continued-fraction step handles approximate samples near multiples of $q/r$.

---

## Reusable ideas

1. **[[Quantum Fourier Transform Circuit]]:** The $O(\ell^2)=O(m^2)$-gate circuit for QFT over $\mathbb{Z}_{2^\ell}$ using Hadamards and controlled phase gates. Foundation for [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]], period finding, and virtually every "precision" quantum algorithm.

2. **[[Order-Finding via QFT and Continued Fractions]]:** Reduce factoring to order-finding. Use a power-of-two QFT to sample near multiples of $q/r$, then extract $r$ via continued fraction expansion of $c/q$. The template for approximate Fourier-sampling on cyclic groups.

3. **[[Reversible Modular Exponentiation with Garbage Cleanup]]:** Compute $x^a \bmod N$ reversibly using repeated squaring + [[Reversible Computation via Compute-Copy-Uncompute|Bennett's method]]. Clean up garbage using modular inverses. Shor also notes an optional watchdog-style measurement of garbage bits, but this is not a substitute for fault-tolerant error correction.

---

## References within this paper

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — direct inspiration; exponential oracle separation via hidden subgroup of $\mathbb{Z}_2^n$
- Bennett (1973) — reversible computation (compute-copy-uncompute for garbage cleanup)
- Bennett et al. (1994) — precision requirements for quantum gates; $O(1/t)$ precision suffices for $t$ steps
- Coppersmith (1994) — approximate QFT dropping small phases; reduces gate count to $O(m \log m)$ in this note's bit-length notation
- Boneh & Lipton (1995) — generalised discrete log to non-cyclic abelian groups
- Deutsch (1985, 1989) — quantum Turing machines and circuits
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein & Vazirani (1993)]] — superpolynomial recursive Fourier-sampling oracle separation and the hidden-linear-string algorithm
- Feynman (1982) — first suggestion that quantum mechanics might confer computational advantage
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985, 1989)]] — defined quantum Turing machines and quantum circuits; first quantum algorithm

---

## Cross-links

### Paper notes
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — the direct precursor
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — reformulates the approach as [[Gapped Phase Estimation|phase estimation]], generalises to any Abelian group
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]] — compiles the continuous rotations in the QFT into a discrete fault-tolerant gate set
- [[Optimal Ancilla-Free Clifford+T Approximation of Z-Rotations (Ross-Selinger 2014) — Paper Notes]] — uses Shor factoring as an oracle for exact optimality in Clifford+T $R_z$ synthesis
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — complementary speedup paradigm (quadratic via search, vs exponential via Fourier sampling)
- [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]] — concrete small-space implementation of Shor's discrete-log algorithm for prime-field elliptic curves

### Trick cards
- [[Quantum Fourier Transform Circuit]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Reversible Modular Exponentiation with Garbage Cleanup]]
- [[Coset Sampling via Fourier Transform]] — the general framework
- [[Interference-Based Period Finding]]
- [[Reversible Computation via Compute-Copy-Uncompute]] — Bennett's method, used for the modular exponentiation cleanup
