> **Source:** Peter W. Shor, *Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer*, SIAM J. Comput. **26**(5), 1484–1509 (1997); Proc. 35th FOCS, 124–134 (1994); arXiv:quant-ph/9508027
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9508027) · [SIAM](https://doi.org/10.1137/S0097539795293172)
> **Tags:** #factoring #discrete-log #QFT #period-finding #cryptography #foundational

---

## The computational problems

**Factoring:** Given an integer $N$, find its prime factorisation. Best classical: number field sieve, time $\exp(O((\log N)^{1/3}(\log\log N)^{2/3}))$ — sub-exponential but super-polynomial.

**Discrete logarithm:** Given a prime $p$, a generator $g$ of $(\mathbb{Z}/p\mathbb{Z})^*$, and $x$, find $r$ such that $g^r \equiv x \pmod{p}$. Best classical: Gordon's number field sieve adaptation, same asymptotic cost.

---

## What the paper does

Gives polynomial-time quantum algorithms for both problems. Factoring an $n$-bit integer takes $O(n^2 \log n \log\log n)$ gates. This is **exponentially faster** than any known classical algorithm, and breaks RSA, Diffie-Hellman, and most deployed public-key cryptography.

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

Choose $q = 2^l$ with $N^2 \leq q < 2N^2$. Use two quantum registers.

### The circuit

**Step 1:** Prepare uniform superposition in register 1:

$$
\frac{1}{\sqrt{q}} \sum_{a=0}^{q-1} |a\rangle|0\rangle
$$

**Step 2:** Compute $x^a \bmod N$ into register 2:

$$
\frac{1}{\sqrt{q}} \sum_{a=0}^{q-1} |a\rangle|x^a \bmod N\rangle
$$

This is the expensive step: reversible modular exponentiation using $O(n^3)$ gates (repeated squaring with Toffoli/Fredkin gates and Bennett's garbage cleanup).

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

**Success probability per trial:** $\geq \phi(r)/(3r) = \Omega(1/\log\log N)$.

After $O(\log\log N)$ repetitions, $r$ is found with high probability. (In practice, also try nearby values $c \pm 1, c \pm 2, \ldots$ to improve the constant.)

---

## The quantum Fourier transform

The QFT over $\mathbb{Z}_q$ with $q = 2^l$:

$$
|a\rangle \mapsto \frac{1}{\sqrt{q}} \sum_{c=0}^{q-1} e^{2\pi i ac/q}|c\rangle
$$

**Implementation:** $l(l-1)/2$ two-qubit gates plus $l$ single-qubit gates:

- $R_j$: Hadamard on qubit $j$
- $S_{j,k}$: controlled phase $e^{i\pi/2^{k-j}}$ on qubits $j,k$

Applied in order: $R_{l-1}, S_{l-2,l-1}, R_{l-2}, S_{l-3,l-1}, S_{l-3,l-2}, R_{l-3}, \ldots, R_0$

Output is bit-reversed (reverse qubit order or read backwards).

**Gate count:** $O(n^2)$ for exact QFT. Coppersmith's approximate QFT drops the small phases $S_{j,k}$ for large $k - j$, reducing to $O(n \log n)$ gates with negligible error.

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

**Gate count:** $O(n^3)$ using schoolbook multiplication (each of $O(n)$ multiplications costs $O(n^2)$ gates). With Schönhage-Strassen: $O(n^2 \log n \log\log n)$.

**Shor's error-detection trick:** After garbage cleanup, $b$ should be 0. Measuring $b$ and checking it's 0 implements a **quantum watchdog** — projecting out any amplitude that's drifted into error states. This is an early instance of using mid-circuit measurement for error detection.

---

## Complexity summary

| Component | Gate count |
|---|---|
| QFT (exact) | $O(n^2)$ |
| QFT (approximate, Coppersmith) | $O(n \log n)$ |
| Modular exponentiation (schoolbook) | $O(n^3)$ |
| Modular exponentiation (Schönhage-Strassen) | $O(n^2 \log n \log\log n)$ |
| **Total factoring** | $O(n^2 \log n \log\log n)$ gates, $O(\log\log n)$ repetitions |
| Classical post-processing (continued fractions, GCD) | $O(n^2)$ |

**Space:** $O(n)$ qubits for the two registers + workspace.

---

## Historical and conceptual significance

This paper is where quantum computing became real — not as hardware, but as a field. Before Shor:
- Deutsch-Jozsa and Bernstein-Vazirani gave oracle separations (interesting but not practically motivated)
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] gave an exponential oracle separation that directly inspired Shor
- There was no exponential quantum speedup for a natural, non-oracle problem

After Shor: billions of dollars in quantum computing investment, post-quantum cryptography standardisation (NIST), and a generation of researchers entering the field.

**The Simon → Shor pipeline:** Shor explicitly acknowledges Simon's influence. The structure is identical — prepare superposition, evaluate periodic function, Fourier transform, extract period from samples — but Simon's group is $\mathbb{Z}_2^n$ (Hadamard = QFT) while Shor's is $\mathbb{Z}_N$ (requires the full QFT). The continued fraction step replaces Simon's linear algebra over $\mathbb{F}_2$.

---

## Reusable ideas

1. **[[Quantum Fourier Transform Circuit]]:** The $O(n^2)$-gate circuit for QFT over $\mathbb{Z}_{2^n}$ using Hadamards and controlled phase gates. Foundation for [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]], period finding, and virtually every "precision" quantum algorithm.

2. **[[Order-Finding via QFT and Continued Fractions]]:** Reduce factoring to order-finding. Use QFT to sample near multiples of $q/r$, then extract $r$ via continued fraction expansion of $c/q$. The template for Fourier-sampling on cyclic groups.

3. **[[Reversible Modular Exponentiation with Garbage Cleanup]]:** Compute $x^a \bmod N$ reversibly using repeated squaring + [[Reversible Computation via Compute-Copy-Uncompute|Bennett's method]]. Clean up garbage using modular inverses. The mid-circuit measurement of garbage bits as an error-detection mechanism (quantum watchdog).

---

## References within this paper

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — direct inspiration; exponential oracle separation via hidden subgroup of $\mathbb{Z}_2^n$
- Bennett (1973) — reversible computation (compute-copy-uncompute for garbage cleanup)
- Bennett et al. (1994) — precision requirements for quantum gates; $O(1/t)$ precision suffices for $t$ steps
- Coppersmith (1994) — approximate QFT dropping small phases; reduces gate count to $O(n \log n)$
- Boneh & Lipton (1995) — generalised discrete log to non-cyclic abelian groups
- Deutsch (1985, 1989) — quantum Turing machines and circuits
- Bernstein & Vazirani (1993) — polynomial quantum-classical separation
- Feynman (1982) — first suggestion that quantum mechanics might confer computational advantage
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985, 1989)]] — defined quantum Turing machines and quantum circuits; first quantum algorithm

---

## Cross-links

### Paper notes
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — the direct precursor
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — reformulates the approach as [[Gapped Phase Estimation|phase estimation]], generalises to any Abelian group
- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]] — compiles the continuous rotations in the QFT into a discrete fault-tolerant gate set
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — complementary speedup paradigm (quadratic via search, vs exponential via Fourier sampling)

### Trick cards
- [[Quantum Fourier Transform Circuit]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Reversible Modular Exponentiation with Garbage Cleanup]]
- [[Coset Sampling via Fourier Transform]] — the general framework
- [[Interference-Based Period Finding]]
- [[Reversible Computation via Compute-Copy-Uncompute]] — Bennett's method, used for the modular exponentiation cleanup
