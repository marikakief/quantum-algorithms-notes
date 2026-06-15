# Pretty-Good Measurement for Wildcard Subset States

> **Source:** Ambainis and Montanaro, arXiv:1210.1148
> **Tags:** #trick #PGM #state-discrimination #Fourier-analysis #Krawtchouk

## What it does

Distinguishes subset states $|\psi_x^k\rangle$ well enough to recover all but $O(1)$ bits of $x$ when $k=n-O(\sqrt n)$.

## The trick

The states are

$$
|\psi_x^k\rangle=\binom nk^{-1/2}\sum_{S\subseteq[n], |S|=k}|S\rangle|x_S\rangle.
$$

Their Gram matrix is

$$
G_{xy}=\langle\psi_x^k|\psi_y^k\rangle
=\frac{\binom{n-d(x,y)}{k}}{\binom nk}.
$$

Since $G_{xy}$ depends only on $x\oplus y$, $G$ is a group-circulant matrix over $\mathbb{Z}_2^n$. The Fourier transform diagonalises it. The eigenvalues indexed by $s\in\{0,1\}^n$ are

$$
\lambda(s)=2^{n-k}\frac{\binom{n-|s|}{n-k}}{\binom nk}.
$$

For geometrically uniform states, the pretty-good measurement is optimal for average error. The PGM outcome probabilities are entries of $\sqrt G$; Fourier analysis and Plancherel reduce the expected Hamming error to the weight-0 and weight-1 Fourier coefficients. For $k=n-O(\sqrt n)$ this gives

$$
\mathbb{E}[d(x,\tilde x)]=O(1).
$$

## When to reach for it

- State families indexed by a finite abelian group.
- Gram matrices whose entries depend only on group difference.
- Query algorithms where approximate reconstruction plus verification is enough.
- Problems involving Krawtchouk-polynomial spectra on Hamming space.

## Complexity

One use of the measurement after the state has been prepared. In query complexity the measurement is free; in circuit complexity, implementing it requires realising the Fourier-diagonal square-root measurement.

## Caveat

This is not black-box amplitude amplification. The gain comes from exploiting the full Gram structure of the state family. If the state family is not group-circulant, this clean Fourier analysis may disappear.

## Related notes

- [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]]
- [[PGM Bootstrapping for Wildcard Search]]
- [[Pretty Good Measurement for State Discrimination]]
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]
