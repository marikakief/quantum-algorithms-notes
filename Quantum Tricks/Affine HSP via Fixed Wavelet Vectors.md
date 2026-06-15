# Affine HSP via Fixed Wavelet Vectors

## Pattern

For the affine group $F^\times\ltimes F$, a maximal cyclic nonnormal hidden subgroup can be encoded by the unique vector it fixes in the large irreducible representation on $L^2_0(F)$.

If the target subgroup is

$$C_b=\left\langle\begin{pmatrix}u&b\\0&1\end{pmatrix}\right\rangle,$$

then the fixed vector is a wavelet-like state $\psi_{b/(1-u)}$. Measuring that vector in the computational basis recovers $b/(1-u)$ with high probability.

## How to use it

1. Classify the subgroup family first: show every target is parameterized by $b$.
2. Identify an irrep where $C_b$ has a one-dimensional fixed space.
3. Convert coset states into that irrep component.
4. Measure in the wavelet/computational basis that turns the fixed vector into the parameter.

## Why it matters

This avoids a generic nonabelian Fourier-sampling problem. The subgroup is found because its invariant vector is almost a delta state after the right representation-theoretic transform.

## Source papers

- [[A Quantum Polylog Algorithm for Non-Normal Maximal Cyclic Hidden Subgroups in the Affine Group of a Finite Field (Wallach 2013) — Paper Notes|Wallach (2013)]].
- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes|Moore--Rockmore--Russell--Schulman (2003)]].
