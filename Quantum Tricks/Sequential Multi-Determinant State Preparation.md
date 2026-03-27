
> **Source:** Tubman, Mejuto-Zaera, Epstein, Hait, Levine, Huggins, Jiang, McClean, Babbush, Head-Gordon, Whaley, arXiv:1809.05523 (2018)
> **Tags:** #trick #state-preparation #multi-determinant #initial-state #phase-estimation #circuit

## What it does

Prepares an arbitrary superposition of $L$ Slater determinants $|\psi_\text{in}\rangle = \sum_{\ell=1}^{L} \alpha_\ell |D_\ell\rangle$ on an $n$-qubit register using $O(nL)$ gates and a single ancilla qubit. No compressed index register required.

## The trick

Each $|D_\ell\rangle$ is an $n$-bit occupation-number bitstring (specifying which orbitals are occupied). The protocol builds the target superposition inductively, maintaining an $n+1$ qubit state where the auxiliary qubit tracks which "branch" is still being constructed.

**Invariant after step $\ell$:**

$$|\psi_\ell\rangle = \beta_\ell |D_\ell\rangle|1\rangle + \sum_{\ell'=1}^{\ell-1} \alpha_{\ell'} |D_{\ell'}\rangle|0\rangle$$

where $|\beta_\ell|^2 = 1 - \sum_{\ell' < \ell} |\alpha_{\ell'}|^2$.

**Inductive step $\ell \to \ell+1$:**

1. **Rotation.** Choose any qubit $k$ where $D_\ell$ and $D_{\ell+1}$ differ. Apply a single-qubit rotation on qubit $k$ controlled by the auxiliary $|1\rangle$, splitting $\beta_\ell$ into $\alpha_\ell$ and $\beta_{\ell+1}$:

$$\beta_\ell |D_\ell\rangle|1\rangle \;\;\mapsto\;\; \bigl(\alpha_\ell |D_\ell\rangle + \beta_{\ell+1} X_k |D_\ell\rangle\bigr)|1\rangle$$

The rotation angle is $\theta_\ell = \arccos(|\alpha_\ell|/|\beta_\ell|)$.

2. **Erase auxiliary on $|D_\ell\rangle$ branch.** Controlled on the $n$-qubit register equalling $D_\ell$, flip the auxiliary qubit from $|1\rangle$ to $|0\rangle$. This costs $O(n)$ gates (multi-controlled-NOT decomposition).

3. **Flip remaining differing bits.** Apply X gates, controlled by the auxiliary, on all other positions $j \neq k$ where $D_\ell$ and $D_{\ell+1}$ differ. Combined effect:

$$\beta_\ell |D_\ell\rangle|1\rangle \;\;\mapsto\;\; \alpha_\ell |D_\ell\rangle|0\rangle + \beta_{\ell+1}|D_{\ell+1}\rangle|1\rangle$$

**Start:** $|\psi_1\rangle = |D_1\rangle|1\rangle$ (trivially prepared by setting appropriate bits and auxiliary to $|1\rangle$).

**End:** After $L-1$ inductive steps, one final rotation and auxiliary cleanup gives $\sum_\ell \alpha_\ell |D_\ell\rangle|0\rangle$. The auxiliary can be discarded.

**Hamming-distance ordering:** The $O(n)$ cost of step 2 depends on the Hamming distance between $D_\ell$ and the erase target. Ordering determinants so that Hamming distances between successive $D_\ell, D_{\ell+1}$ are small reduces the constant. Any ordering that minimises $\sum_\ell \text{d}_H(D_\ell, D_{\ell+1})$ is useful; this is related to the Hamiltonian-cycle-on-Hamming-cube problem but greedy ordering suffices in practice.

## When to reach for it

- Preparing a multi-determinant initial state for phase estimation on strongly-correlated molecules (e.g. stretched bonds, transition metal complexes, FeMoco, DMFT impurity Hamiltonians).
- Any setting where ASCI or another selected CI method gives a compact classical wavefunction approximation with $L \ll 2^n$ important determinants, and you want to load it into a quantum register.
- When you want to avoid the overhead of a compressed index register (and the associated QROM / select-unitary circuit).

## Complexity

- **Gates:** $O(nL)$ total; each of $L-1$ inductive steps costs $O(n)$ for the erase (multi-controlled NOT) and $O(H)$ for bit-flip cleanup, where $H$ is the Hamming distance between successive determinants.
- **Ancilla:** 1 qubit.
- **Circuit depth:** $O(nL)$ in the sequential decomposition. Can be partially parallelised when independent steps don't overlap on the same qubits.
- **Classical preprocessing:** Need $\beta_\ell$ values (a single pass over the sorted determinant list) and ASCI-derived $\alpha_\ell$ amplitudes. $O(L)$ classical work.

For comparison, Method A (compressed register + QROM) uses $O(L)$ gate depth for the register prep and $O(n + L)$ for the QROM isometry, but requires $\lceil \log L \rceil$ extra ancilla qubits and a more complex circuit.

## Caveat

- **Not fault-tolerant optimised.** The paper does not give T-gate counts; the multi-controlled NOTs in the erase step are expensive in Clifford+T when $n$ is large.
- **$O(nL)$ may dominate QPE.** For $n \sim 100$ orbitals and $L \sim 100$ determinants, gate count is $\sim 10^4$, fine relative to QPE circuit. For $n, L \sim 10^3$, state prep itself is $\sim 10^6$ gates.
- **The erase step requires knowing $D_\ell$ exactly.** The multi-controlled NOT that erases the auxiliary uses a classical description of the bitstring $D_\ell$ to construct a matching circuit. This is fine — the circuit is classically precomputed — but it means the full circuit depends on all $L$ determinant bitstrings.

## Related notes
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — introduced here
- [[ASCI Overlap Estimation for QPE Initial State]] — provides the $\alpha_\ell$ coefficients and determines which $L$ determinants to include
- [[Hamming-Distance Ordering for Multi-Determinant Preparation]] — the ordering heuristic that reduces the constant in $O(nL)$
- [[Givens Rotation Slater Determinant Preparation]] — single-determinant special case ($L=1$); complementary technique
- [[QROM (Quantum Read-Only Memory)]] — used in the alternative Method A construction
- [[Iterative Phase Estimation (Kitaev)]] — the target algorithm that uses this state as input
