# Collimation Sieve for Dihedral Hidden Shift

## Pattern

Start from phase vectors

$$|\psi\rangle={1\over\sqrt{\ell}}\sum_j e^{2\pi i b_js/N}|j\rangle,$$

where the coefficients $b_j$ are known classically and the hidden shift $s$ is unknown. Combine vectors and partially measure coefficient sums modulo powers of 2 so that the surviving phase coefficients become more aligned in their low-order bits.

## How to use it

1. Generate many random Fourier-mode phase states from the hidden-shift oracle.
2. Pair phase vectors and tensor them.
3. Measure a coarse residue class of $b_j+b'_k$.
4. Keep the branch whose coefficients share more low-order zero bits.
5. Iterate until a two-dimensional state reveals one bit of $s$.

## Why it matters

The sieve turns weak phase information into an extractable bit without needing polynomial-time post-processing of raw samples. Kuperberg's later form keeps the quantum state small by tracking coefficient lists classically.

## Source papers

- [[Another Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2013) — Paper Notes|Kuperberg (2013)]].
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]].
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]].
