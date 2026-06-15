
> **Source:** Simon, FOCS 1994 / SIAM J. Comput. 1997; generalized in [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]], [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]]
> **Tags:** #trick #hidden-subgroup #fourier #period-finding #fundamental

## What it does

Extracts information about a hidden subgroup $H \leq G$ by preparing a uniform superposition over a coset of $H$, applying the quantum Fourier transform over $G$, and measuring. In the exact finite abelian HSP setting, the measurement outcome is a uniformly random character in the annihilator $H^\perp$.

For nonabelian groups, Fourier sampling gives irreducible-representation labels and possibly matrix indices, not scalar elements of a dual subgroup. The simple six-step recipe and efficient reconstruction statement below should be read as the abelian template.

## The trick

For a finite abelian group $G$, given a function $f: G \to S$ that is constant on cosets of $H$ and distinct on different cosets:

1. Prepare $\frac{1}{\sqrt{|G|}} \sum_{g \in G} |g\rangle|0\rangle$
2. Evaluate: $\frac{1}{\sqrt{|G|}} \sum_{g} |g\rangle|f(g)\rangle$
3. Measure the second register → first register collapses to uniform superposition over a coset $g_0 H$
4. Apply QFT over $G$ to the first register
5. Measure → get a random character in $H^\perp$
6. Repeat enough times to generate $H^\perp$ (typically $O(\log |G|)$ samples in the standard finite abelian setting), then recover $H$ by classical abelian-group linear algebra

**For $G = \mathbb{Z}_2^n$ (Simon's problem):** The QFT is just $H^{\otimes n}$. $H^\perp = s^\perp$ is an $(n-1)$-dimensional subspace. Solve linear system over $\mathbb{F}_2$.

**For Shor/order finding:** Factoring reduces to finding the order $r$ of $x \bmod N$. If the Fourier modulus were exactly divisible by $r$, this would look like exact cyclic annihilator sampling. Shor instead uses a power-of-two register $q$ with $N^2 \le q < 2N^2$, so measured values are near multiples of $q/r$; continued fractions recover $r$ from these approximate samples.

## When to reach for it

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon's problem]]: hidden subgroup of $\mathbb{Z}_2^n$
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's abelian stabilizer problem]] and finite abelian HSPs
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor order finding / discrete log]]: factoring first reduces to cyclic period finding; discrete log has an abelian HSP formulation over a product group
- Quantum algorithms that need to extract periodicity or symmetry from a black-box function

## Complexity

For exact finite abelian HSPs, polynomially many samples in $\log |G|$ suffice, and in common vector-space/cyclic cases $O(\log |G|)$ samples are enough to identify the annihilator with high probability. Each sample uses one oracle call and one QFT. The QFT over $\mathbb{Z}_2^n$ costs $O(n)$ gates; over an $n$-bit cyclic modulus it costs $O(n^2)$ gates exactly, or $O(n\log n)$ with an approximate QFT in standard settings.

## Caveat

Only works efficiently for **Abelian** groups. For non-Abelian groups (e.g., the symmetric group $S_n$, relevant to graph isomorphism), coset sampling produces matrix-valued representations rather than scalar phases, and the classical post-processing becomes exponentially hard in general. The non-Abelian hidden subgroup problem remains one of the major open problems in quantum algorithms.

## Related notes

- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Interference-Based Period Finding]]
- [[Gapped Phase Estimation]]
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — extends coset sampling to non-abelian solvable groups via subnormal chains
- [[Subgroup State Preparation via Subnormal Chain]]
