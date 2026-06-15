# Two-Dimensional POVM for q-Hedral Hidden Conjugates

> **Source:** Moore--Rockmore--Russell--Schulman, arXiv:quant-ph/0211124
> **Tags:** #trick #hidden-subgroup-problem #povm #fourier-sampling #affine-groups

## What it does

Turns a high-dimensional Fourier sample for $\mathbb{Z}_q\ltimes\mathbb{Z}_p$ into a two-outcome statistic that separates different hidden conjugates by constant total variation distance.

## The trick

For a hidden conjugate $H_a^b$ in a $q$-hedral group, a $q$-dimensional irrep sample contains phases

$$
\omega_p^{k a^s b}.
$$

Instead of trying to decode all row amplitudes, perform a POVM that keeps only adjacent row labels $u$ and $u+1$. Up to a global phase, the remaining state is

$$
\frac{1}{\sqrt2}
\begin{pmatrix}
\omega_p^{k a^u b}\\
\omega_p^{k a^{u+1} b}
\end{pmatrix}.
$$

Apply a Hadamard transform. The measurement probabilities become

$$
\cos^2\left(\frac{\pi m b}{p}\right),
\qquad
\sin^2\left(\frac{\pi m b}{p}\right),
$$

where $m$ is uniform over $\mathbb{Z}_p^*$ after the random irrep and row choices.

For any $b\neq b'$, the induced distributions have total variation distance bounded below by a constant. Hence $O(\log p)$ samples identify $b$ information-theoretically.

## When to reach for it

Use it when a full high-dimensional Fourier sample is hard to decode, but adjacent coordinates carry a relative phase depending on the hidden conjugating parameter.

## Complexity

The quantum measurement has polynomial size. The sample count is $O(\log p)$ for information-theoretic identification among $p$ choices.

## Caveat

The known classical maximum-likelihood decoding for general $q$ may be exponential. This is a measurement-reconstruction result, not always a full polynomial-time HSP algorithm.

## Related notes

- [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes]]
