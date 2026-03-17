# Quantum States as Coset Representatives

> **Source:** Watrous, arXiv:quant-ph/0011023 (2001), §4.2
> **Tags:** #trick #group-theory #factor-groups #state-representation #non-abelian

## What it does

Represents elements of a factor group $G/H$ as quantum states $|gH\rangle = |H|^{-1/2}\sum_{h \in H}|gh\rangle$, enabling quantum algorithms on $G/H$ even when coset elements have no unique classical encoding.

## The trick

For $H \trianglelefteq G$, the state $|gH\rangle$ is a uniform superposition over the coset $gH$. Key properties:

- **Well-defined:** $|gH\rangle = |g'H\rangle$ whenever $gH = g'H$ (same coset → same state)
- **Orthogonal:** $\langle gH | g'H \rangle = 0$ when $gH \neq g'H$ (different cosets → orthogonal states)
- **Group operations work:** The black-box group oracle automatically implements factor group operations:

$$
U_G|gH\rangle|g'H\rangle = |gH\rangle|gg'H\rangle
$$

This means any quantum algorithm that only needs group multiplication and inversion (e.g., [[Order-Finding via QFT and Continued Fractions|Shor's order-finding]], abelian group decomposition) can be run on $G/H$ by preparing $|H\rangle$ and using the $G$-oracle.

## The application in Watrous

For a solvable group $G$ with $H \trianglelefteq G$ and $G/H$ abelian:
1. Prepare $|H\rangle$ using [[Subgroup State Preparation via Subnormal Chain|the subnormal chain algorithm]]
2. Run the abelian group structure algorithm on $G/H$, using $|gH\rangle$ states as "elements"
3. Decompose $G/H \cong \mathbb{Z}_{q_1} \times \cdots \times \mathbb{Z}_{q_m}$ and compute isomorphisms $\theta: G/H \to \mathbb{Z}_{q_1} \times \cdots \times \mathbb{Z}_{q_m}$

## When to reach for it

- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous's algorithm]]: computing structure of abelian factor groups of solvable groups
- Any quantum algorithm on groups where the "elements" are cosets rather than individual group elements
- Representing quotient structures quantumly when no classical normal form exists
- Potential applications to lattice problems, error-correcting codes, or other algebraic structures with natural quotient operations

## Caveat

Requires $H \trianglelefteq G$ (normality), otherwise coset multiplication isn't well-defined. Also requires the ability to prepare $|H\rangle$ efficiently — this is the non-trivial part, handled by Watrous's [[Subgroup State Preparation via Subnormal Chain|subnormal chain algorithm]] for solvable groups. For general groups, preparing $|H\rangle$ may be as hard as computing $|H|$.

## Related notes

- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]
- [[Subgroup State Preparation via Subnormal Chain]]
- [[Coset Sampling via Fourier Transform]]
- [[Order-Finding via QFT and Continued Fractions]]
