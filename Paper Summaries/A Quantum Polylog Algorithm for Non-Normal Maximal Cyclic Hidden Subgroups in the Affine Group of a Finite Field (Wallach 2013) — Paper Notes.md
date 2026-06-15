# A Quantum Polylog Algorithm for Non-Normal Maximal Cyclic Hidden Subgroups in the Affine Group of a Finite Field (Wallach 2013) — Paper Notes

> **Source:** Nolan Wallach, *A quantum polylog algorithm for non-normal maximal cyclic hidden subgroups in the affine group of a finite field*, arXiv:1308.1415
> **Links:** [arXiv](https://arxiv.org/abs/1308.1415)
> **Tags:** #hidden-subgroup-problem #affine-groups #nonabelian

---

## Metadata

- **Author:** Nolan Wallach
- **Year:** 2013 arXiv preprint; later version dated 2018
- **arXiv:** [1308.1415](https://arxiv.org/abs/1308.1415)
- **Zoo entry:** Quantum Algorithm Zoo #207
- **Topic:** HSP over finite-field affine groups; maximal cyclic nonnormal subgroups; wavelet states.
- **Status in vault:** Added from Algorithm Zoo HSP batch.

## Computational problem

Let $F$ be a finite field of order $q$, with affine group

$$G=F^\times\ltimes F=\left\{\begin{pmatrix}a&b\\0&1\end{pmatrix}:a\in F^\times,b\in F\right\}.$$

For a fixed generator $u$ of $F^\times$, define the cyclic subgroup

$$C_b=\left\langle \begin{pmatrix}u&b\\0&1\end{pmatrix}\right\rangle,$$

which has order $q-1$. The promise is that the hidden subgroup is one of these maximal nonnormal cyclic subgroups $C_b$. The task is to recover $b\in F$.

## What the paper does

Wallach gives a polylogarithmic-time quantum algorithm for this restricted affine-group HSP. The key observation is representation-theoretic: the affine group has one large irreducible representation of dimension $q-1$, and the hidden cyclic subgroup fixes a nearly point-localized wavelet vector inside it. Sampling the right representation component and measuring the associated wavelet recovers the subgroup parameter with high probability.

This is not a general affine-HSP solution. It solves a sharply defined family: maximal cyclic nonnormal subgroups.

## Algorithmic structure

1. **Use the affine representation.**
   - The group has $q-1$ one-dimensional representations and one nontrivial irreducible representation $\pi_0$ of dimension $q-1$ acting on $L^2_0(F)$.
   - The relevant hidden subgroup information sits in $\pi_0$.

2. **Characterize hidden subgroup fixed vectors.**
   - For each $C_b$, there is a vector $\psi_{b/(1-u)}$ fixed by $C_b$ under $\pi_0$.
   - Lemma 2 shows uniqueness up to phase for these fixed vectors, and measuring $\psi_v$ in the computational basis yields $v$ with probability $(q-1)/q$.

3. **Generate the right state from a coset sample.**
   - A coset state for $C_b$ is Fourier-transformed over the multiplicative coordinate $F^\times$.
   - The calculation splits into cases depending on the coset offset; the desired wavelet appears with substantial probability.
   - The one-dimensional irreps are discarded when they carry no useful $b$ information; repeated coset samples give enough successful large-irrep components with high probability.

4. **Recover $b$.**
   - Once $v=b/(1-u)$ is measured, compute $b=(1-u)v$.
   - Repetition boosts success probability to $1-\varepsilon$.

## Key results

- Every maximal cyclic nonnormal subgroup of the affine group is some $C_b$.
- The algorithm finds $b$ with probability $1-\varepsilon$ in complexity
  $$O\!\left((\log q)^R(\log(1/\varepsilon))^2\right)$$
  for a constant $R$ independent of $q$.
- The method covers finite fields of general prime-power order, not only prime fields.
- The case $q=2^n$ is highlighted as especially interesting.

## Assessment

This is a nice special-case win for affine HSP, but the scope is narrow. Compared with Moore--Rockmore--Russell--Schulman, which studies basis selection and Fourier sampling for affine groups, Wallach exploits a very specific subgroup family and a fixed-vector/wavelet structure. The result is cleaner algorithmically but less general.

For Marika's purposes, the reusable part is the fixed-vector trick: if a nonnormal subgroup fixes a state that is nearly a computational-basis delta after a known representation transform, HSP can become parameter recovery rather than generic nonabelian Fourier analysis.

## Reusable ideas

- **Fixed-vector recovery:** characterize the hidden subgroup by the unique vector it fixes in a large irrep.
- **Wavelet basis as measurement basis:** choose a basis adapted to affine transformations, not merely the standard Fourier basis.
- **Parameterization before measurement:** prove every target subgroup is $C_b$ before designing the quantum measurement.

## Cross-links

- [[Hidden Subgroup Problem]]
- [[Affine HSP via Fixed Wavelet Vectors]]
- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes|Moore--Rockmore--Russell--Schulman (2003)]]
- [[Basis-Selected Strong Fourier Sampling for Affine HSP]]
- [[On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998) — Paper Notes|Ettinger--Høyer (1998)]]

## References

- Nolan Wallach, *A quantum polylog algorithm for non-normal maximal cyclic hidden subgroups in the affine group of a finite field*, arXiv:1308.1415.
