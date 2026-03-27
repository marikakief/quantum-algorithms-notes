> **Source:** Low & Chuang, arXiv:1610.06546 (2017); applied to quantum chemistry in Babbush, Gidney et al., arXiv:1805.03662, Phys. Rev. X **8**, 041015 (2018)
> **Tags:** #trick #qubitization #phase-estimation #LCU #walk-operator #spectroscopy #quantum-walk

## What it does

Encodes the spectrum of a Hamiltonian $H$ exactly (up to rotation synthesis) in the eigenphases of a unitary walk operator $W$, so that quantum phase estimation on $W$ directly samples $H$'s eigenstates with $O(\lambda/\varepsilon)$ total oracle queries — Heisenberg-limited and optimal.

## The trick

Given $H$ in LCU form

$$H = \sum_\ell w_\ell H_\ell, \quad H_\ell^2 = I, \quad \lambda = \sum_\ell w_\ell$$

construct two oracles:

$$\text{PREPARE}|0\rangle = |L\rangle = \sum_\ell \sqrt{w_\ell/\lambda}\,|\ell\rangle \qquad \text{(block-encodes coefficients)}$$

$$\text{SELECT} = \sum_\ell |\ell\rangle\langle\ell| \otimes H_\ell \qquad \text{(applies Hamiltonians conditioned on index)}$$

These satisfy the **block-encoding relation**:

$$(\langle L| \otimes I)\cdot \text{SELECT} \cdot (|L\rangle \otimes I) = H/\lambda$$

Form the **qubitized walk operator**:

$$W = R_L \cdot \text{SELECT}, \qquad R_L = 2(|L\rangle\langle L| \otimes I) - I$$

$R_L$ is a reflection through $|L\rangle$ (implemented as PREPARE, reflect about $|0\rangle$, PREPARE$^\dagger$).

**Key property:** on the two-dimensional subspace $\text{span}\{|L\rangle|k\rangle, |\phi_k\rangle\}$ associated to each eigenstate $|k\rangle$ of $H$ with eigenvalue $E_k$, $W$ acts as a 2×2 rotation:

$$W\big|_{\text{2D}} = \exp(i \arccos(E_k/\lambda)\,Y)$$

The eigenphases of $W$ are $\pm\arccos(E_k/\lambda)$, so

$$E_k = \lambda \cos(\arg(\text{eigenphase}_k))$$

Run phase estimation on $W$. The number of controlled-$W$ applications required is

$$2^m < \sqrt{2\pi}\,\lambda/\Delta E \implies \text{total queries} = O(\lambda/\varepsilon)$$

This is Heisenberg-limited (optimal) for spectroscopy.

### Comparison with LCU time evolution

The Taylor series / LCU approach simulates $e^{-iHt}$ at cost $O(\lambda t \log(1/\varepsilon))$ queries, then wraps in phase estimation for another $O(1/\varepsilon)$ overhead — total $O(\lambda t / \varepsilon)$ with a large log factor. Qubitization achieves the same $O(\lambda/\varepsilon)$ query count *directly* for the spectroscopy problem, without simulating time evolution explicitly.

## When to reach for it

- Fault-tolerant phase estimation on a Hamiltonian whose LCU decomposition is known.
- When you have explicit PREPARE and SELECT oracles and want the minimum-query spectroscopy algorithm.
- Periodic systems in the plane-wave dual basis, where $\lambda = O(N^2)$ (see [[Plane-Wave Dual Basis]]).
- Hubbard model, where $\lambda = 2Nt + Nu/2$ — linear in $N$.
- Any setting where T-gate minimization matters more than qubit count.

## Complexity

- Queries to $W$: $O(\lambda/\varepsilon)$.
- Total T gates: $O((S + 2P)\lambda/\varepsilon)$ where $S$ = T-cost of SELECT, $P$ = T-cost of PREPARE.
- For electronic structure in dual basis: $S = O(N)$, $P = O(N + \log(1/\varepsilon))$, giving $T = O(N^3/\varepsilon)$.
- Ancilla qubits: $O(\log(\lambda N/\varepsilon))$.

## Caveat

- $O(\lambda/\varepsilon)$ is *tight for spectroscopy* (cannot be improved). If you want *simulation* ($e^{-iHt}$), not spectra, qubitization gives $O(\lambda t)$ — same as Taylor series, no improvement in query count.
- If $\lambda$ is large (e.g., bad basis choice), the $O(\lambda/\varepsilon)$ cost can be worse than Trotter in practice. Basis choice drives everything.
- Eigenphases near $0$ or $\pi$ (i.e., $E_k \approx \pm\lambda$) suffer from the large derivative of $\arccos$ — coefficient discretization errors amplify. See Appendix A of [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush 2018]] for the rigorous bound.
- The QSVT framework (Berry, Babbush et al. 2019) generalizes and improves upon this; qubitization is a special case of block encoding.

## Related notes

- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — first application to quantum chemistry with full T-gate counts
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes]] — introduces [[Asymmetric Qubitization]], a variant using two different state oracles $A$, $B$ instead of a single PREPARE; applied to the SYK model
- [[Truncated Taylor Series Simulation]] — the LCU time-evolution approach this partly supersedes for spectroscopy
- [[QROM (Quantum Read-Only Memory)]] — used inside PREPARE
- [[Coherent Alias Sampling for PREPARE]] — constructs the PREPARE oracle
- [[Unary Iteration]] — constructs the SELECT oracle
- [[Iterative Phase Estimation (Kitaev)]] — the phase estimation wrapper
- [[Plane-Wave Dual Basis]] — the Hamiltonian encoding that makes $\lambda = O(N^2)$
- [[Asymmetric Qubitization]] — generalizes this trick to use asymmetric bra/ket state preparations
