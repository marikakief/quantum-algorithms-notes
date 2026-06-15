# Exact Weight Recovery by Divisibility-Gap Rounding

> **Source:** Geraci-Lidar, arXiv:quant-ph/0703023
> **Tags:** #trick #Gauss-sums #coding-theory #finite-fields #rounding

## What it does

Turns approximate Gauss-sum phase estimates into exact irreducible-cyclic-code weights by rounding to an arithmetic lattice of allowed weights.

## The trick

For an irreducible cyclic $[n,k]$ code over $\mathbb F_q$, the McEliece formula writes the Hamming weight of a codeword indexed by $y\in\mathbb F_{q^k}^*$ as

$$
w(y)=\frac{q^k(q-1)}{qN}-\frac{q-1}{qN}\sum_{a=1}^{d-1}\bar\chi(y)^{-a}G_{\mathbb F_{q^k}}(\bar\chi^a,1),
$$

where $q^k-1=nN$ and

$$
d=\gcd\!\left(N,\frac{q^k-1}{q-1}\right).
$$

A quantum computer estimates each Gauss sum as

$$
G_{\mathbb F_{q^k}}(\chi,\beta)=\sqrt{q^k}\,e^{i\gamma}.
$$

Approximate phases give an approximate weight $\widetilde w(y)$. The exact-recovery step uses the theorem that all weights of the code are divisible by

$$
q^{\theta_{n,k}-1},
$$

where

$$
\theta_{n,k}=\frac{1}{q-1}\min_{0<j\leq N}S'(jn)
$$

and $S'(x)$ is the base-$q$ digit sum. If

$$
|w(y)-\widetilde w(y)| < \frac{q^{\theta_{n,k}-1}}{2},
$$

then the nearest allowed multiple identifies $w(y)$ exactly. Geraci-Lidar give the sufficient phase-error bound

$$
\varepsilon < \frac{q^{\theta_{n,k}-1-k/2}}{4}.
$$

## When to reach for it

Use it when a quantum subroutine estimates phases or amplitudes approximately, but the final desired quantity is known to lie on a sparse arithmetic lattice. The gap between allowed values can convert approximation into exact recovery.

## Complexity

The cost multiplies the underlying phase-estimation cost by $1/\varepsilon$. With [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes|van Dam-Seroussi Gauss-sum estimation]], each required phase costs

$$
O\!\left(\frac{1}{\varepsilon}(\log(q^k))^2\right).
$$

## Caveat

The lattice gap must be known and large enough. If $q^{\theta_{n,k}-1}$ is too small relative to the phase-estimation error, rounding can assign the wrong weight; then the exactness claim fails.

## Related notes

- [[On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes]]
- [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes]]
- [[Gauss-Sum Phase from Multiplicative-Character Fourier Eigenstates]]
- [[Potts Partition Function via Cocycle-Code Weight Enumerators]]
