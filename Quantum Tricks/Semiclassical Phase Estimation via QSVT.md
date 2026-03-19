# Semiclassical Phase Estimation via QSVT

> **Source:** Martyn, Rossi, Tan & Chuang, *Grand Unification of Quantum Algorithms*, PRX Quantum **2**, 040203 (2021) — Section V.C. Related to [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev 1995]] semiclassical QPE.
> **Tags:** #trick #phase-estimation #QSVT #semiclassical #bit-extraction

## What it does

Extracts the eigenphase $\theta$ of a unitary $U$ (where $U|\psi\rangle = e^{2\pi i\theta}|\psi\rangle$) one bit at a time, using the [[QSVT Sign-Function Discriminator]] to perform the binary decisions. No quantum Fourier transform required.

## The key construction

For bit $j$ with current phase estimate $\hat\theta$, define the matrix:
$$A_j(\hat\theta) = \frac{1}{2}\bigl(I + e^{-2\pi i\hat\theta} U^{2^j}\bigr)$$

This is a linear combination of $I$ and $U^{2^j}$, normalized so it's blockable.

**Singular value analysis:** On eigenstate $|\psi\rangle$ with eigenphase $\theta$:
$$A_j(\hat\theta)|\psi\rangle = \frac{1}{2}(1 + e^{2\pi i(\theta - \hat\theta)2^j})|\psi\rangle$$

The coefficient has magnitude $|\cos(\pi(\theta - \hat\theta)2^j)|$, which is the singular value of $A_j(\hat\theta)$ restricted to $|\psi\rangle$.

**Bit encoding:** The sign of $\cos(\pi(\theta - \hat\theta)2^j)$ is:
$$\text{sign}(\cos(\pi(\theta - \hat\theta)2^j)) = +1 \iff \lfloor 2^j\theta \rfloor_{\hat\theta} = 0 \pmod{2}$$

In other words, the sign of the cosine encodes the $j$-th bit of $\theta$ relative to the current estimate $\hat\theta$.

## The iterative algorithm

1. **Initialize:** $\hat\theta = 0$, start from the most significant bit $j = \ell - 1$ (for $\ell$-bit precision).

2. **Bit extraction:** Block-encode $A_j(\hat\theta)$ (using controlled-$U^{2^j}$ and classical phases). Apply [[QSVT Sign-Function Discriminator]] with $\kappa$ slightly above zero. Measure. The outcome gives bit $j$ of $\theta$.

3. **Update:** $\hat\theta \leftarrow \hat\theta + (\text{bit}_j) \cdot 2^{-j-1}$. This shifts the phase to center the next bit's transition region at $x = 0$.

4. **Repeat:** For $j = \ell-2, \ell-3, \ldots, 0$.

After $\ell$ rounds, $\hat\theta$ is an $\ell$-bit approximation to $\theta$.

## Block-encoding $A_j(\hat\theta)$

$$A_j(\hat\theta) = \frac{1}{2}(I + e^{-2\pi i\hat\theta} U^{2^j}) = \frac{1}{2}\sum_{b \in \{0,1\}} e^{-2\pi i\hat\theta b} U^{b \cdot 2^j}$$

This is an LCU with two terms. Prepare: $|\text{PREP}\rangle = \frac{1}{\sqrt{2}}(|0\rangle + e^{-2\pi i\hat\theta}|1\rangle)$. Select: controlled-$U^{2^j}$ on the $|1\rangle$ branch. The classical phase $e^{-2\pi i\hat\theta}$ is just a phase gate on the prepare qubit. See [[Linear Combination of Unitaries (LCU)]].

Cost of $U^{2^j}$: repeated squaring, so $O(j)$ applications of $U$, or $O(2^j)$ if $U$ can't be squared efficiently.

## Comparison to standard QPE

| Feature | Standard QPE | Semiclassical QSVT |
|---|---|---|
| Control register | $\ell$ qubits (quantum) | 1 qubit (classical adaptation) |
| Final step | QFT on $\ell$ qubits | None |
| Adaptivity | None | Yes — $\hat\theta$ updated each round |
| Total $U$ uses | $O(2^\ell)$ worst case | $O(2^\ell)$ worst case |
| Ancilla overhead | $O(\ell)$ | $O(m)$ per round ($m$ = block-encoding width) |

The key difference: classical feedback replaces the quantum Fourier transform. This was first proposed by Kitaev (1995) in a different form; here it falls out naturally as a QSVT instance.

## Error analysis

Each bit extraction succeeds with probability $\geq 1 - \delta$ using $O(\log(1/\delta))$ repetitions and a [[QSVT Sign-Function Discriminator]] with $\epsilon = O(\delta)$.

Total failure probability over $\ell$ bits: $O(\ell \delta)$ by union bound.

The sign discriminator needs $\kappa$ small enough that the transition region doesn't include the signal. At round $j$, the signal is $\cos(\pi(\theta - \hat\theta)2^j)$, and if the previous bits are correct, $|(\theta - \hat\theta)2^j| \leq 1/2$, so the cosine argument is in $[-\pi/2, \pi/2]$ and the cosine is positive. Any $\kappa < \cos(\pi/2 - \delta) \approx \delta$ works, so $\kappa = O(\delta)$ suffices.

## Why this is in the QSVT paper

It demonstrates that phase estimation is not a separate primitive but an instance of the sign-function discriminator pattern. The fact that QPE emerges from QSVT without any special structure is one of the more satisfying parts of the "grand unification" — even the measurement-based classical-feedback protocol falls out of the block-encoding framework.

## Related notes

- [[QSVT Sign-Function Discriminator]] — the binary discrimination step
- [[QSP-to-QSVT Lifting Chain]] — where this fits
- [[Linear Combination of Unitaries (LCU)]] — how $A_j(\hat\theta)$ is block-encoded
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — related semiclassical phase estimation
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
