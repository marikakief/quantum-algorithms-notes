# Norm-Gap Obstruction between H and abs(H) in Oracle Simulation

> **Source:** Andrew M. Childs and Robin Kothari, *Limitations on the simulation of non-sparse Hamiltonians*, arXiv:0908.4398  
> **Tags:** #trick #norms #lower-bounds #hamiltonian-simulation

## The obstruction

In entry-oracle Hamiltonian simulation, the quantum-walk approach (Berry–Childs 2011, and earlier) scales with $\|\text{abs}(H)\|$ (spectral norm of the entrywise-absolute-value matrix), not $\|H\|$ itself. These satisfy:

$$
\|H\| \le \|\text{abs}(H)\| \le \|H\|_1,
$$

and the gap can be as large as $\sqrt{N}$: the Hadamard tensor $H = R^{\otimes \log N}$ where $R = \frac{1}{\sqrt{2}}\begin{pmatrix}1&1\\1&-1\end{pmatrix}$ satisfies $\|H\| = 1$ but $\|\text{abs}(H)\| = \sqrt{N}$.

Childs–Kothari show this is not just a proof artifact: the oracle model for entry-oracle simulation cannot distinguish $H$ from $\text{abs}(H)$ on signed (complex phase) structure. Any algorithm using only $H_{jk}$ magnitudes will effectively simulate $\text{abs}(H)$, paying for $\|\text{abs}(H)\|$ rather than $\|H\|$.

## Why this matters

Suppose $\|H\|$ is small (say $O(1)$) but $\|\text{abs}(H)\| = \Theta(\sqrt{N})$. The entry-oracle simulation cost scales with $\|\text{abs}(H)\|t = \Theta(\sqrt{N}t)$, far exceeding $\|H\|t = O(t)$. The naive hope of simulating any Hamiltonian with cost $\text{poly}(\|H\|t, \log N)$ is ruled out.

The norm chain from Lemma 1 of the paper:

$$
\|H\|_{\max} \le \text{mcn}(H) \le \|H\| \le \|\text{abs}(H)\| \le \|H\|_1 \le \sqrt{N}\,\text{mcn}(H) \le N\|H\|_{\max},
$$

and each inequality is tight. For $k$-sparse Hamiltonians, the chain collapses to within a $\sqrt{k}$ factor, which is why sparse simulation doesn't suffer this problem.

## The fix (historically)

[[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] and [[Linear Combination of Unitaries (LCU)|LCU]] access models encode the *signed* matrix structure (phase information) into the oracle, allowing algorithms to exploit $\|H\|$ or $\|H\|_F$ rather than $\|\text{abs}(H)\|$. This is why post-2012 simulation frameworks ([[Linear Combination of Unitaries (LCU)|LCU]], [[Qubitization Iterate|qubitization]], [[QSVT Meta-Template|QSVT]]) are fundamentally better than walk-based entry-oracle approaches for non-sparse Hamiltonians.

## Related notes
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes]]
- [[Hardness Embedding via Circulant Hamiltonian Eigenphases]]
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
