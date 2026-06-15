> **Source:** Daniel R. Simon, *On the Power of Quantum Computation*, Proc. 35th FOCS, 116–123 (1994); SIAM J. Comput. **26**(5), 1474–1483 (1997)
> **Links:** [SIAM](https://doi.org/10.1137/S0097539796298637) · [Semantic Scholar](https://www.semanticscholar.org/paper/d34a810719d6c132d9fb697df1b5a38e2fd0dc05)
> **Tags:** #oracle-separation #hidden-subgroup #exponential-speedup #foundational #BQP

---

## The problem

**Simon's Problem:** Given a black-box function $f: \{0,1\}^n \to \{0,1\}^n$ with the promise that there exists $s \in \{0,1\}^n$ such that:

$$
f(x) = f(y) \iff x \oplus y \in \{0^n, s\}
$$

Find $s$. (If $s = 0^n$, the function is 1-to-1; otherwise it's 2-to-1 with period $s$.)

---

## What the paper does

Gives a quantum algorithm that finds the hidden mask $s$ using $O(n)$ queries to $f$ in the nonzero-mask case, and equivalently distinguishes the promised injective and two-to-one function classes. This is the first **exponential bounded-error quantum versus probabilistic classical oracle separation**: classical algorithms require $\Omega(2^{n/2})$ queries by a birthday-bound argument.

The oracle separation it establishes: there exists an oracle $\mathcal{O}$ relative to which $\mathsf{BPP}^\mathcal{O} \neq \mathsf{BQP}^\mathcal{O}$. This was the strongest evidence at the time that quantum computers are strictly more powerful than classical probabilistic ones.

In later language, Simon's problem is the abelian hidden subgroup problem over $\mathbb{Z}_2^n$, with hidden subgroup $\{0^n,s\}$. The paper predates that unified HSP vocabulary.

---

## The algorithm

**Step 1: Superposition.** Start with $|0^n\rangle|0^n\rangle$. Apply $H^{\otimes n}$ to the first register:

$$
\frac{1}{\sqrt{2^n}} \sum_{x \in \{0,1\}^n} |x\rangle|0^n\rangle
$$

**Step 2: Query.** Evaluate $f$ into the second register:

$$
\frac{1}{\sqrt{2^n}} \sum_{x} |x\rangle|f(x)\rangle
$$

**Step 3: Measure the second register.** Get some value $f(x_0)$. If $s \neq 0^n$, the first register collapses to:

$$
\frac{1}{\sqrt{2}}(|x_0\rangle + |x_0 \oplus s\rangle)
$$

This is a uniform superposition over the coset $\{x_0, x_0 \oplus s\}$ of the hidden subgroup $\{0^n, s\}$.

**Step 4: Hadamard.** Apply $H^{\otimes n}$ to the first register:

$$
H^{\otimes n}\left(\frac{|x_0\rangle + |x_0 \oplus s\rangle}{\sqrt{2}}\right) = \frac{1}{\sqrt{2^{n-1}}} \sum_{\substack{y \in \{0,1\}^n \\ y \cdot s = 0}} (-1)^{y \cdot x_0} |y\rangle
$$

The interference is constructive when $y \cdot s = 0 \pmod{2}$ and destructive when $y \cdot s = 1$.

**Step 5: Measure.** Get a uniformly random $y \in s^\perp$ (the $(n-1)$-dimensional subspace orthogonal to $s$).

**Step 6: Repeat** $O(n)$ times. Collect $n-1$ linearly independent vectors $y_1, \ldots, y_{n-1} \in s^\perp$. Solve the linear system $y_i \cdot s = 0$ over $\mathbb{F}_2$ to find $s$.

This recovery discussion assumes $s \neq 0^n$. The $s=0^n$ case is the injective promise case and is usually handled through the decision version: distinguish injective functions from two-to-one functions with a nonzero XOR mask.

### Probability of success

After $n + c$ rounds, the probability of having $n-1$ linearly independent vectors is at least $1 - 2^{-c}$ (each new vector is linearly independent with probability $\geq 1 - 2^{-(n-k)}$ when $k$ independent vectors are already collected). $O(n)$ rounds suffice for overwhelming success probability.

---

## The classical lower bound

Any classical deterministic algorithm making $q$ queries can distinguish at most $\binom{q}{2}$ collisions from the $2^{n-1}$ possible collisions. For probabilistic algorithms, $\Omega(2^{n/2})$ queries are needed — this is the birthday bound.

More precisely: after $q$ classical queries, the distribution of observed values is statistically close to uniform for both the 1-to-1 and 2-to-1 cases until $q = \Omega(2^{n/2})$.

The quantum algorithm uses $O(n)$ queries → **exponential separation**.

---

## Why this matters

### The road to Shor

Simon's algorithm is the direct conceptual predecessor to [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's phase estimation]] and [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor's factoring algorithm]]:

| [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] | [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] |
|---|---|
| Exact hidden subgroup of $\mathbb{Z}_2^n$ | Period/order finding over a cyclic group, implemented with a power-of-two Fourier register |
| Hadamard transform = Fourier over $\mathbb{Z}_2^n$ | Approximate QFT/Fourier sampling because the period need not divide the register size |
| Exact coset sampling → linear equations over $\mathbb{F}_2$ | Peaks near multiples of the reciprocal period → continued fractions |
| Exact period recovery from $O(n)$ samples | Period recovery from $O(\log N)$ samples |
| Oracle problem | Concrete number-theoretic problem |

Shor has acknowledged that Simon's result was a direct inspiration. The structure — prepare a superposition, evaluate, Fourier transform, sample frequency information — became the template for the entire [[Hidden Subgroup Problem]] framework, but Shor's order finding uses approximate Fourier sampling and classical continued-fraction postprocessing rather than the exact finite-group coset sampling of Simon's problem.

### Oracle separations in context

| Separation | Paper | Type |
|---|---|---|
| $\mathsf{EQP} \neq \mathsf{P}$ relative to oracle | Deutsch-Jozsa (1992) | Total function, but $\mathsf{EQP}$ (exact) vs $\mathsf{P}$ |
| $\mathsf{BQP} \neq \mathsf{BPP}$ relative to oracle | Bernstein-Vazirani (1993) | Superpolynomial recursive Fourier-sampling separation; the basic hidden-linear-string problem is only polynomial |
| $\mathsf{BQP} \neq \mathsf{BPP}$ relative to oracle | **[[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]]** | **Exponential separation** |
| Concrete factoring speedup | [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] | Under hardness assumptions, not oracle |

Simon's is the first **exponential** oracle separation between bounded-error quantum and classical.

---

## Reusable ideas

1. **[[Coset Sampling via Fourier Transform]]:** Prepare a uniform superposition over a coset of a hidden subgroup, apply the Fourier transform (Hadamard for $\mathbb{Z}_2^n$, QFT for cyclic groups), and measure to get a uniformly random element of the dual subgroup. This is the engine behind Simon's algorithm, Shor's algorithm, and the general [[Hidden Subgroup Problem]] approach.

2. **[[Interference-Based Period Finding]]:** Use quantum interference to distinguish periodic from non-periodic functions. The Hadamard/QFT converts a periodic state in position space into a peaked state in frequency space. Classical algorithms can't extract this period without $\Omega(2^{n/2})$ samples.

---

## References within this paper

- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes|Deutsch & Jozsa (1992)]] — first quantum oracle speedup (promised constant vs balanced)
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein & Vazirani (1993)]] — superpolynomial recursive Fourier-sampling oracle separation; the simple hidden-linear-string algorithm gives only a polynomial query gap
- Berthiaume & Brassard (1992) — oracle separations for quantum complexity classes
- Yao (1993) — quantum circuit model
- Bennett, Bernstein, Brassard & Vazirani (1997) — $\Omega(\sqrt{N})$ lower bound for search, placing limits on quantum speedups
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — exponential speedup for factoring and discrete log; directly inspired by Simon's approach
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — defined the quantum Turing machine and gave the first quantum algorithm
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard, Høyer & Tapp (1997)]] — the unstructured collision problem; achieves $O(N^{1/3})$ vs Simon's $O(\log N)$ for the structured case

---

## Cross-links

### Paper notes
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — generalizes Simon's Fourier sampling to the Abelian stabilizer problem (which subsumes factoring and discrete log)
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — [[Standard Amplitude Amplification|amplitude amplification]], a different paradigm for quantum speedups (quadratic, not exponential)
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — walk-based quantum speedups, complementary to the Fourier-sampling approach
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — modern unifying framework that connects both search-type and Fourier-type quantum speedups

### Trick cards
- [[Coset Sampling via Fourier Transform]]
- [[Interference-Based Period Finding]]
- [[Gapped Phase Estimation]] — the QPE technique that generalizes Simon's bitwise extraction
