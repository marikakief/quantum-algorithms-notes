
> **Source:** Su, Berry, Wiebe, Rubin, Babbush, arXiv:2105.12767 (2021)
> **Tags:** #trick #state-preparation #amplitude-amplification #LCU #qubitization #block-encoding

## What it does

Avoids the $3\times$ overhead of amplitude amplification in LCU-based block encodings by applying a different Hamiltonian term in the failure branch of a probabilistic state preparation, maintaining the correct normalization without any amplification step.

## The trick

Suppose, in an idealized two-term model, $H = H_1 + H_2$ is block-encoded via separate PREPARE circuits for $H_1$ and $H_2$, and the PREPARE for $H_2$ succeeds with probability $p \approx 1/4$. Normally, a single round of amplitude amplification boosts $p$ to $\sim 1$ but triples the state-preparation cost.

Instead: when PREPARE for $H_2$ fails (probability $1 - p \approx 3/4$), apply SELECT for $H_1$. The resulting block encoding is

$$\frac{H_2}{4\lambda_2} + \frac{3H_1}{4\lambda_1}$$

If $\lambda_1 = 3\lambda_2$, this equals $(H_1 + H_2)/(4\lambda_2) = (H_1 + H_2)/(\lambda_1 + \lambda_2)$ — the correct Hamiltonian with optimal normalization.

When $\lambda_1 \neq 3\lambda_2$, an ancilla qubit rotated to $\cos\theta|0\rangle + \sin\theta|1\rangle$ interpolates between the two cases:
- $\lambda_1 < 3\lambda_2$: apply SEL for $H_1$ only when PREP fails AND ancilla is $|0\rangle$
- $\lambda_1 > 3\lambda_2$: apply SEL for $H_1$ when PREP fails OR ancilla is $|0\rangle$

Either way, $\theta$ is chosen so the effective block encoding has the desired normalization in the theorem's adjusted cost model (for example, involving $(\lambda_U+\lambda_V)/p_\nu$ in the regime $\lambda_T < 3(\lambda_U+\lambda_V)$).

In the first-quantized chemistry application: $H_1 = T$ (kinetic) and $H_2 = U + V$ (potential). The nested-box state preparation for the $1/\|\nu\|$ weights has success probability near $1/4$, with finite-precision and equality-test corrections. The fallback applies SEL for $T$ in the failure branch. This avoids tripling the expensive PREPARE branch, although total savings are smaller if other block-encoding costs dominate.

## When to reach for it

- LCU block encoding where $H = H_1 + H_2$ with separate PREPARE circuits
- One PREPARE has success probability significantly below 1
- The $\lambda$ values of the two terms allow rebalancing (not too far from $\lambda_1 = 3\lambda_2$, or adjustable via an ancilla rotation)
- Amplitude amplification is otherwise the bottleneck

The trick is most valuable when the probabilistic PREPARE is expensive (e.g., nested arithmetic + inequality tests) and would need to be tripled.

## Complexity

Saves up to a factor of 3 on the expensive probabilistic PREPARE branch compared to one round of amplitude amplification. The ancilla rotation costs $n_T - 3$ Toffolis in the source construction, and the flag logic adds a small constant overhead.

## Caveat

Requires that SEL for $H_1$ is cheap enough that using it in the failure branch is worthwhile. If SEL for $H_1$ is the dominant cost, the savings are reduced. In the chemistry application, SEL for $T$ is cheap because of [[Kinetic Energy as Bit-Product LCU]], which is why the fallback is useful.

The effective $\lambda$ is governed by the paper's exact success probabilities, $p_\nu$-amplification choice, and $P_{\mathrm{eq}}$ correction factor. The $p\approx 1/4$ picture is intuition, not the exact formula in all parameter regimes.

## Related notes
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]]
- [[Kinetic Energy as Bit-Product LCU]] — makes SEL for $T$ cheap enough that the fallback costs almost nothing
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the framework where this is applied
- [[Coherent Alias Sampling for PREPARE]] — an alternative PREPARE technique that achieves near-unit success probability (but at higher cost)
