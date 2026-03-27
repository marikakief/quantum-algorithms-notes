
> **Source:** Babbush, Berry, Neven, arXiv:1806.02793, Phys. Rev. A **99**, 040301(R) (2019)
> **Tags:** #trick #qubitization #LCU #block-encoding #quantum-signal-processing #asymmetric

## What it does

Generalizes [[Qubitization (Quantum Walk for Spectral Encoding)|standard (symmetric) qubitization]] to allow two different state preparation oracles for the bra and ket sides of the block encoding, so that $H/\lambda = \langle B | V | A \rangle$ instead of $H/\lambda = \langle G | U | G \rangle$.

## The trick

Given $H = \sum_\ell w_\ell H_\ell$ with $H_\ell$ self-inverse, define two state preparations:

$$A|0\rangle = |A\rangle = \sum_\ell \alpha_\ell |\ell\rangle, \qquad B|0\rangle = |B\rangle = \sum_\ell \beta_\ell |\ell\rangle$$

and a signal oracle $V = \sum_\ell |\ell\rangle\langle\ell| \otimes H_\ell$. Then $\langle B | V | A \rangle = \sum_\ell \alpha_\ell \beta_\ell^* H_\ell$, and we need $\lambda \alpha_\ell \beta_\ell^* = w_\ell$.

The normalization constraint forces $\lambda \geq \sum_\ell |w_\ell|$, with equality in the symmetric case $|\alpha_\ell| = |\beta_\ell|$. In the maximally asymmetric case where $\beta_\ell = 1/\sqrt{L}$ (equal superposition) and $\alpha_\ell \propto w_\ell$:

$$\lambda = \sqrt{L \sum_\ell |w_\ell|^2} = L\sqrt{\langle |w_\ell|^2 \rangle}$$

The overhead factor relative to symmetric qubitization is:

$$\frac{\lambda_{\text{asym}}}{\lambda_{\text{sym}}} = \frac{\sqrt{\langle |w_\ell|^2 \rangle}}{\langle |w_\ell| \rangle}$$

For Gaussian-distributed weights: $\sqrt{\pi/2} \approx 1.25$.

**Making it self-inverse:** Since $B^\dagger V A$ is not self-inverse, introduce an ancilla qubit and construct

$$U = \begin{pmatrix} 0 & A^\dagger V B \\ B^\dagger V A & 0 \end{pmatrix}$$

This *is* self-inverse ($U^2 = I$). The reflection $R = 2|G\rangle\langle G| - I$ uses $G$ = Hadamard on the ancilla. Then $W = RU$ has eigenvalues $e^{\pm i\arccos(h/\lambda)}$ as in standard qubitization, and quantum signal processing proceeds identically.

The ancilla selects whether $A$ or $B$ is applied; $V$ is applied once per step regardless. Since $V$ typically dominates the cost (it's the SELECT oracle), the overhead from the extra ancilla and doubled state preparation is negligible.

## When to reach for it

- The PREPARE oracle $G|0\rangle = \sum_\ell \sqrt{w_\ell/\lambda}\,|\ell\rangle$ is hard to implement (requires square roots of the coefficients), but a state proportional to $w_\ell$ itself is easy. This happens when the $w_\ell$ arise naturally as amplitudes of some known quantum state — e.g., output of a random circuit.
- Hamiltonians with random Gaussian couplings (SYK model, random matrix models, disordered systems). Random orthogonal circuits produce Gaussian amplitudes, so $A$ is a random circuit and $B$ is Hadamards.
- Any setting where the coefficient distribution makes $\sqrt{\langle |w_\ell|^2\rangle}/\langle |w_\ell|\rangle$ close to 1 — i.e., when the coefficients are roughly uniform in magnitude.

## Complexity

- Overhead factor: $\sqrt{\langle |w_\ell|^2\rangle}/\langle |w_\ell|\rangle$ relative to symmetric [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]].
- Per-step cost: one application of $V$ (the SELECT oracle) plus one each of $A$, $A^\dagger$, $B$, $B^\dagger$.
- Total simulation cost: $O((\lambda t + \log(1/\varepsilon)/\log\log(1/\varepsilon)) \cdot (C_V + C_A + C_B))$ via quantum signal processing.

## Caveat

- The overhead can be large if the weight distribution is heavy-tailed. For weights drawn from a distribution with $\langle w^2 \rangle \gg \langle |w| \rangle^2$, the asymmetric $\lambda$ blows up relative to the symmetric case. Only use when the distribution has bounded ratio $\sqrt{\langle w^2 \rangle}/\langle |w| \rangle$.
- The alternative approach via oblivious amplitude amplification (Appendix A of the source paper) triples the cost of $V$ per step. The ancilla-based construction in the main text is strictly better.

## Related notes

- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — source paper; applies this to the SYK model
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the symmetric version; this generalizes it
- [[Random Circuit State Preparation (Gaussian Amplitudes)]] — the $A$ oracle implementation for Gaussian-coupled Hamiltonians
- [[Coherent Alias Sampling for PREPARE]] — the standard method for symmetric PREPARE; asymmetric qubitization avoids the need for this
