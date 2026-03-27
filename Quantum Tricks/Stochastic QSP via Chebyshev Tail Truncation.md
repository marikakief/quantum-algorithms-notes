
> **Source:** John M. Martyn, Patrick Rall, *Halving the Cost of Quantum Algorithms with Randomization*, arXiv:2409.03744, npj Quantum Information **11**, 51 (2025)
> **Tags:** #trick #QSP #QSVT #randomization #channel-approximation #Chebyshev

## What it does

Cuts the average query count of any QSP-based algorithm roughly in half (asymptotically) by randomizing over a family of lower-degree approximating polynomials. The output is a quantum channel rather than a single unitary.

## The trick

Given a degree-$d$ Chebyshev approximation $P(x) = \sum_{n=0}^d c_n T_n(x)$ to target function $F(x)$ with error $\|P-F\|_\infty \le \epsilon$, construct an ensemble $\{(P_j, p_j)\}$ as follows:

**1. Exploit exponential coefficient decay.** For smooth functions, Chebyshev coefficients satisfy $|c_n| \le C e^{-qn}$ for $n \ge d/2$. This means the high-degree tail carries little weight.

**2. Partition into blocks.** Group Chebyshev indices $\{0,\ldots,d\}$ into blocks by degree range (roughly doubling: $[0, d/2]$, $[d/2, 3d/4]$, etc.).

**3. Build block polynomials.** For each block $B_j$, define $P_j(x) = \sum_{n \le \max B_j} c_n T_n(x)$ — a truncation of $P$ at the block's upper degree.

**4. Set sampling probabilities.** Choose $p_j$ proportional to the block's contribution weight. Because high-degree blocks have exponentially smaller coefficients, $p_j$ decays exponentially with $j$.

**5. The guarantee (Theorem 3):**
- Individual error: $|P_j(x) - F(x)| \le 2\sqrt{\epsilon}$
- Average error: $\sum_j p_j(P_j(x) - F(x)) \le \epsilon$
- Average degree: $d_{\mathrm{avg}} = \sum_j p_j \deg(P_j) \le d/2 + O(1)$

**6. Apply the [[Mixing Lemma for Block-Encodings]].** With spectral error $a = 2\sqrt{\epsilon}$ (individual) and average error $b = \epsilon$:
$$\|\Lambda_A - \mathcal{F}_A\|_\diamond \le a^2 + 2b = 6\epsilon$$

The channel achieves the same diamond-norm error as the deterministic degree-$d$ approximation, using average degree only $d/2 + O(1)$.

**Why the average degree halves:** Exponentially decaying $p_j$ and geometrically growing block degrees combine to give average degree $\approx d/2$. Intuitively: you're sampling from a distribution that heavily favors the cheaper half of the polynomial.

## When to reach for it

- You need a quantum channel (not a unitary): observable estimation, simulation as an end goal, or feeding into a protocol that accepts channel input.
- You're in a precision-dominated regime where $\log(1/\epsilon)$ is the cost bottleneck.
- You can tolerate classical pre-computation (sampling the ensemble, synthesizing QSP phases for the chosen polynomial).

Specific algorithms that benefit: [[Hamiltonian simulation]] $e^{-iHt}$, imaginary time evolution, phase estimation, ground state preparation, matrix inversion — anywhere [[QSVT Meta-Template|QSP/QSVT]] is used with a smooth approximation target.

## Complexity

Average query count: $d/2 + O(1)$ where $d$ is the deterministic QSP degree. Explicit cost reduction factor:
$$\frac{2d_{\mathrm{avg}}}{d} \lesssim 1 + \frac{\log C}{qd} \xrightarrow{d \to \infty} \frac{1}{2}$$

For real-time simulation: reduces from $O(\alpha t + \log(1/\epsilon)/\log\log(1/\epsilon))$ to $O(\alpha t + \frac{1}{2}\log(1/\epsilon)/\log\log(1/\epsilon))$.

## Caveat

- **Channel, not unitary.** Cannot be used as a coherent subroutine inside amplitude amplification or other protocols requiring a single unitary. The fully-coherent alternative is [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes|Martyn et al. 2023]].
- **Asymptotic.** The factor-of-2 is approached asymptotically as $d \to \infty$. For moderate precision, the improvement is real but less than $2\times$.
- **Parity.** QSP requires definite-parity polynomials. Corollary 4 of the paper ensures the ensemble can be chosen with the correct parity — but this constraint must be handled in the construction.

## Related notes

- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]]
- [[Mixing Lemma for Block-Encodings]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[QSVT Meta-Template]]
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]] — same randomization philosophy at the Hamiltonian term level
- [[Randomized Multi-Product Sampling]] — same philosophy at the multi-[[product formula]] level
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] — the coherent alternative
