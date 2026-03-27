
> **Source:** Martyn, Rossi, Tan & Chuang, *Grand Unification of Quantum Algorithms*, PRX Quantum **2**, 040203 (2021) — Sections V.A, V.B, V.C. Also [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. 2019]].
> **Tags:** #trick #QSVT #sign-function #amplitude-amplification #phase-estimation #search

## What it does

Uses a polynomial approximation to the **sign function** within QSVT to discriminate between singular values (or eigenvalues) above and below a threshold. The output is a binary measurement result that tells you which side of the threshold you're on.

## The polynomial

Approximate $\text{sign}(x)$ on $[-1,1] \setminus (-\kappa, \kappa)$ for a threshold $\kappa > 0$:
$$P_{\text{sign},\kappa,\epsilon}(x) \approx \begin{cases} +1 & x > \kappa \\ -1 & x < -\kappa \end{cases}$$

This can be achieved with an **odd polynomial** of degree $d = O\!\left(\kappa^{-1} \log(1/\epsilon)\right)$.

The polynomial must be:
- Odd (same parity as its degree, which is odd)
- Bounded: $|P(x)| \leq 1$ for $|x| \leq 1$
- Approximately $\pm 1$ outside $(-\kappa, \kappa)$

See [[Parity-Aware Polynomial Design]] for the parity constraint, and [[FIR-Style Minimax Design for Quantum Responses]] for one construction approach.

## Applications

All three use the same polynomial; they differ only in what gets block-encoded and what the threshold means.

### Search / fixed-point amplitude amplification

Block-encode the **Grover diffusion operator** $G = 2|\psi\rangle\langle\psi| - I$ where $|\psi\rangle = H^{\otimes n}|0\rangle$ is the uniform superposition. The target amplitude is $a = \langle\psi|\text{target}\rangle = 1/\sqrt{N}$.

Set $\kappa = 1/\sqrt{N}$. Apply $P_{\text{sign},\kappa}$ to singular values: this amplifies the target component from $a = \kappa$ to $\approx 1$. Since $P$ is odd and smooth, there's no overshoot — the algorithm succeeds whether or not you know $\kappa$ exactly.

Cost: $O(\sqrt{N} \log(1/\epsilon))$ oracle queries. Compare to standard [[Standard Amplitude Amplification]] which needs $O(\sqrt{N})$ but can fail if $\kappa$ isn't known.

See also: [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]].

### Eigenvalue threshold problem

Block-encode Hermitian $H/\alpha$. Threshold $\lambda^* \in [-\alpha, \alpha]$.

Set $\kappa = $ gap around $\lambda^*/\alpha$. The polynomial maps eigenvalues $> \lambda^*$ to $+1$ and eigenvalues $< \lambda^*$ to $-1$.

Measurement on the ancilla tells you which side of $\lambda^*$ the eigenvalue lies. If you want a spectral projector onto a specific range, see [[Chebyshev Polynomial Spectral Projector]] instead.

### Phase estimation (bit extraction)

Given unitary $U$ with eigenphase $\theta$, construct matrix:
$$A_j(\hat\theta) = \frac{1}{2}\bigl(I + e^{-2\pi i\hat\theta} U^{2^j}\bigr)$$

The singular value is $|\cos(\pi(\theta - \hat\theta)2^j)|$. The **sign** of $\cos(\pi(\theta - \hat\theta)2^j)$ encodes the $j$-th bit of $\theta$ relative to $\hat\theta$. Apply $P_{\text{sign},\kappa}$ with $\kappa$ slightly above zero to extract this bit.

See [[Semiclassical Phase Estimation via QSVT]] for the full iterative construction.

## Circuit cost

For threshold $\kappa$ and precision $\epsilon$:
- **Polynomial degree:** $d = O(\kappa^{-1}\log(1/\epsilon))$
- **Oracle calls:** $O(d)$ calls to $U_A$ (block-encoding)
- **Gate overhead:** $O(d \cdot m)$ for projector-controlled phases, $m$ = ancilla width

For search: $\kappa = 1/\sqrt{N}$, so $d = O(\sqrt{N}\log(1/\epsilon))$ total queries.

## What to remember

The sign function is the workhorse polynomial for **binary decisions** in QSVT. If your problem reduces to "is something above or below a threshold?", this is the trick. The three applications (search, eigenvalue threshold, phase estimation bit) are all the same circuit with different block-encodings.

## Caveat

The discriminator doesn't work near the transition ($|x| < \kappa$). If your singular value is near the threshold, the output is ambiguous. This is a feature, not a bug — you need a spectral gap for the algorithm to be meaningful. For phase estimation this means you need sufficient circuit depth to resolve adjacent bits.

## Related notes

- [[QSP-to-QSVT Lifting Chain]] — where this fits in the framework
- [[QSVT Meta-Template]] — the full recipe
- [[Parity-Aware Polynomial Design]] — odd parity requirement
- [[Fixed-Point Amplitude Amplification with Eigenstate Filtering]] — search application
- [[Chebyshev Polynomial Spectral Projector]] — eigenvalue range version
- [[Semiclassical Phase Estimation via QSVT]] — phase estimation application
- [[Standard Amplitude Amplification]] — classical comparison
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
