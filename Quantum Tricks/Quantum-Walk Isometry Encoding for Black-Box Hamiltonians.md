
> **Source:** Dominic W. Berry and Andrew M. Childs, *Black-box [[Hamiltonian simulation]] and unitary implementation*, arXiv:0910.4157  
> **Tags:** #trick #quantum-walk #black-box #hamiltonian-simulation

## Idea

Build a discrete-time quantum walk whose eigenphases encode the spectrum of $H$, using only oracle access to matrix elements $H_{jk}$. The walk operator is then used (via phase estimation) to simulate $e^{-iHt}$.

## Construction (Section III of 0910.4157)

Expand the Hilbert space from $\mathbb{C}^M$ to $\mathbb{C}^{M+1} \otimes \mathbb{C}^{M+1}$. Define the isometry

$$
T|j\rangle = |\eta_j\rangle := |j\rangle\left(\sqrt{\frac{\varepsilon}{\|H\|_1}}\sum_{k=1}^M \sqrt{H^*_{jk}}|k\rangle + \sqrt{1 - \frac{\varepsilon\sigma_j}{\|H\|_1}}|M+1\rangle\right),
$$

where $\sigma_j = \sum_k |H_{jk}|$ and $\|H\|_1 = \max_j \sigma_j$ (max column absolute sum). The walk step is:

$$
V = iS(2TT^\dagger - \mathbf{1}),
$$

where $S$ swaps the two registers. Then:

$$
\langle\eta_j|S|\eta_k\rangle = \frac{\varepsilon H_{jk}}{\|H\|_1},
$$

so the walk operator has eigenvalues $e^{\pm i\arcsin(\varepsilon\lambda/\|H\|_1)}$ for each eigenvalue $\lambda$ of $H$. Phase estimation on $V$ recovers $\lambda$, and thus the evolution $e^{-i\lambda t}$ can be applied.

## Note on the norm

The walk construction uses $\|H\|_1 = \max_j \sum_k |H_{jk}|$ (induced 1-norm, max column absolute sum), which is exactly $\|\text{abs}(H)\|_1 \ge \|\text{abs}(H)\| \ge \|H\|$. For non-sparse Hamiltonians, this can be $\sqrt{N}$ times larger than the spectral norm. This norm-gap is what motivated the later [[Qubitization Iterate|qubitization-based approaches]] using [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encodings]].

## Laziness parameter

Setting $\varepsilon < 1$ makes the walk "lazy" — it reduces the phase advance per step, which decreases the approximation error from the $\arcsin$ nonlinearity. The trade-off is more walk steps needed to accumulate the desired phase. See [[Lazy-Walk Phase-Correction for Simulation Accuracy]].

## When to use

This is the natural approach in the entry-oracle model, before block-encodings existed. It works for any Hamiltonian (sparse or not) given oracle access to $H_{jk}$.

## Caveat

Scaling with $\|\text{abs}(H)\|$ rather than $\|H\|$ is fundamental in this access model; [[Qubitization Iterate|qubitization]]-based approaches using [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encodings]] avoid this by encoding the signed matrix structure directly.

## Related notes
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]]
- [[Lazy-Walk Phase-Correction for Simulation Accuracy]]
- [[Amplitude-Amplified State Preparation inside Walk Operators]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
