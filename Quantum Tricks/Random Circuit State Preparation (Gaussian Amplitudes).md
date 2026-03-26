# Random Circuit State Preparation (Gaussian Amplitudes)

> **Source:** Babbush, Berry, Neven, arXiv:1806.02793, Phys. Rev. A **99**, 040301(R) (2019)
> **Tags:** #trick #state-preparation #random-circuits #Gaussian-distribution #SYK-model

## What it does

Prepares a quantum state whose amplitudes are Gaussian distributed by running a random orthogonal quantum circuit. Useful as the PREPARE oracle in [[Asymmetric Qubitization]] when the Hamiltonian has random Gaussian couplings.

## The trick

A random orthogonal quantum circuit on $\log L$ qubits produces a state $|A\rangle = \sum_{\ell=0}^{L-1} \alpha_\ell |\ell\rangle$ where the amplitudes $\alpha_\ell$ are (approximately) i.i.d. Gaussian with zero mean and variance $1/L$ — a consequence of the Porter-Thomas distribution for random orthogonal evolutions.

For a Hamiltonian $H = \sum_\ell w_\ell H_\ell$ with Gaussian-distributed weights $w_\ell$, using [[Asymmetric Qubitization]] with $|A\rangle$ as above and $|B\rangle = (1/\sqrt{L})\sum_\ell |\ell\rangle$ (equal superposition) gives:

$$\langle B | V | A \rangle = \frac{1}{\sqrt{L}} \sum_\ell \alpha_\ell H_\ell \propto \sum_\ell w_\ell H_\ell = H$$

The proportionality constant is absorbed into $\lambda$. The key insight: the random circuit naturally samples from the correct distribution, so no explicit encoding of the $O(L)$ individual weights is needed.

The circuit is drawn once (classically sample the random gates, then implement them coherently). The same circuit is used at every step of the quantum signal processing.

## When to reach for it

- SYK model simulation: $L = N^4$ four-body Majorana terms with Gaussian couplings $J_{pqrs} \sim \mathcal{N}(0, 3!J^2/N^3)$.
- Any Hamiltonian with random i.i.d. Gaussian coefficients — disordered spin models, random matrix Hamiltonians, models from condensed matter with quenched disorder.
- When you want to avoid explicitly loading $L$ coefficients into a quantum register (which would cost $O(L)$ via [[QROM (Quantum Read-Only Memory)|QROM]] or [[Coherent Alias Sampling for PREPARE|alias sampling]]).

## Complexity

- Circuit depth: $O(\mathrm{polylog}(L/\varepsilon))$ for convergence to within $\varepsilon$ of the Gaussian amplitude distribution.
- Rigorous bound: 1D random circuits on $\log L$ qubits converge to $\varepsilon$-approximate 2-designs in $O(\log^2 L + \log(L/\varepsilon))$ gates (Harrow & Low 2009). Higher-dimensional layouts converge in $O(\log(L/\varepsilon))$.
- Compare with [[Coherent Alias Sampling for PREPARE]]: $O(L + \log(L/\varepsilon))$ T gates. The random circuit approach is exponentially cheaper when applicable.

## Caveat

- Only works when the target amplitudes are Gaussian distributed. If the Hamiltonian coefficients have a different distribution, the random circuit won't match.
- The $O(\mathrm{polylog}(L/\varepsilon))$ cost relies on an assumption — not a theorem — about convergence of random circuit amplitudes to a Gaussian distribution. The paper's evidence is numerical (Boixo et al. 2018, Nature Physics) and theoretical bounds for approximate designs. A rigorous amplitude-distribution convergence bound at this polylogarithmic cost is still open.
- Uses orthogonal (real-valued) random circuits specifically to ensure real amplitudes matching the real couplings of the SYK model. For complex Gaussian couplings, use unitary random circuits instead.

## Related notes

- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — source paper
- [[Asymmetric Qubitization]] — the qubitization variant that uses this as the $A$ oracle
- [[Coherent Alias Sampling for PREPARE]] — the standard (deterministic) approach to PREPARE; this trick replaces it for Gaussian-coupled Hamiltonians
- [[QROM (Quantum Read-Only Memory)]] — another deterministic coefficient-loading method this avoids
