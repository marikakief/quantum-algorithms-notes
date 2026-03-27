
> **Paper:** Shadow Hamiltonian Simulation  
> **DOI:** [Nature Comms 16, 3486 (2025)](https://www.nature.com/articles/s41467-025-57451-z)  
> **Date read:** 2026-03-12  
> **Tags:** #hamiltonian-simulation #shadow-states #lie-algebra #quantum-algorithms

---

## One-line take

Instead of simulating the full quantum state, encode only the observable expectations you care about into a smaller "shadow state" that evolves under its own (simpler) Hamiltonian — exponential compression when the operator set has Lie algebra structure.

---

## Shadow states

Given a state $\rho$ and a set of operators $S = \{O_1, \ldots, O_M\}$, define the **shadow state**:
$$
|\rho; S\rangle := \frac{1}{\sqrt{A}} \sum_{m=1}^{M} \langle O_m \rangle |m\rangle, \qquad A = \sum_m \langle O_m \rangle^2.
$$
This is an $M$-dimensional quantum state encoding all observable expectations, regardless of the (possibly infinite) dimension of the underlying system.

**Invariance Property (IP):** $H$ and $S$ satisfy the IP if
$$
[H, O_m] = -\sum_{m'} h_{mm'} O_{m'} \quad \forall m,
$$
i.e., commutators of $H$ with operators in $S$ close within $S$. This holds automatically when $S$ spans a Lie algebra containing $H$.

**Theorem 1:** If the IP holds and the $M \times M$ matrix $H_S$ with entries $h_{mm'}$ is Hermitian, then
$$
\frac{d}{dt} |\rho(t); S\rangle = -iH_S |\rho(t); S\rangle.
$$
Shadow [[Hamiltonian simulation]] then reduces to standard [[Hamiltonian simulation]] of $H_S$.

## When $H_S$ is Hermitian

- **Orthogonal operator sets** in finite-dimensional systems: $\mathrm{tr}(O_m^\dagger O_{m'}) = \lambda\delta_{mm'}$ — automatic.
- **Free fermions:** $S$ = degree-$k$ Majorana products — orthogonal, Hermitian.
- **Pauli operators** on qubits — orthogonal, Hermitian.
- **Free bosons:** Requires a factorisation $\Gamma = \Gamma_1\Gamma_2$ and basis change $O = \Gamma_2 Y$, giving $H_S = i\Gamma_2\Omega\Gamma_1$. Hermitian when $\Gamma \succeq 0$ (e.g., spring-coupled oscillators).

## Applications

### Exponentially large free-fermion systems

$n = 2^r$ fermionic modes, $S$ = degree-2 Majorana operators → $M = O(n^2)$ shadow state dimension.
- Shadow state uses $O(\log n) = O(r)$ qubits.
- Simulates dynamics of exponentially many modes.
- BQP-complete — genuine quantum advantage.

### Exponentially many coupled oscillators

Recovers the Babbush et al. (arXiv:2303.13012) result as a special case and extends to quantum oscillators and higher-order correlators.

For a $D$-dimensional lattice: quantum cost $\tilde{O}(t)$ vs classical $O(t^{1+D})$. In 3D this is about a cubic speedup in $t$.

### Qubit systems

$S$ = all $n$-qubit Pauli operators → $M = 4^n$. Shadow state is $|\psi\rangle \otimes \overline{|\psi\rangle}$ up to a Bell rotation.

For non-interacting qubits: $S = \{X_1, Y_1, Z_1, \ldots\}$, $M = 3n$ → exponential compression.

### Two-time correlators and Green's functions

**Theorem 2:** Encode $\langle O_m(t) O_{m'}(t') \rangle$ in a two-register shadow state evolving under $H_S \otimes \mathbb{1} + \mathbb{1} \otimes H_S$. Extends to $q$-time correlators and to operators evolving under different Hamiltonians.

### Operator growth / scrambling

Encode time-evolved operator $Z(t) = \sum_m z_m(t) O_m$ as state $|Z(t)\rangle \propto \sum_m z_m(t)|m\rangle$, which evolves under $\overline{H_S}$. Measuring operator weight growth gives access to scrambling and light cones.

### Learning unitaries

For Clifford operations: the shadow approach gives a full characterisation using $2n$ shadow evolution experiments.

## Efficiency conditions

For efficient simulation of $H_S$:
1. **Sparsity:** At most $d = \mathrm{polylog}(M)$ operators $O_{m'}$ appear in each $[H, O_m]$.
2. **Efficient coefficient access:** Each $h_{mm'}$ computable in poly time given $(m, m')$.
3. **Efficient neighbour access:** Nonzero positions in each row of $H_S$ findable efficiently.

Under these conditions: block-encode $H_S / (d\|H_S\|_{\max})$ and simulate via QSP with cost $O(d(t \cdot d\|H_S\|_{\max} + \log(1/\varepsilon)))$ queries.

## Relation to other "shadow" frameworks

| Framework | What it compresses | When efficient |
|---|---|---|
| Shadow tomography (Aaronson 2018) | Measurement samples | Few observables, many copies |
| Shadow [[Hamiltonian simulation]] (this paper) | Time evolution | IP satisfied, $H_S$ sparse |
| Classical shadows (Huang et al. 2020) | State representation | Local observables |

Note: the Invariance Property is the key condition — it is satisfied for free theories but breaks as soon as interactions generate operators outside $S$.

## References within this paper

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low & Chuang (2017)]] — QSP used to simulate the shadow Hamiltonian $H_S$
- Babbush et al. (2023, arXiv:2303.13012) — exponential speedup for coupled oscillators; recovered as special case
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2018)]] — shadow tomography (different "shadow" framework; see that note for the comparison)
- Huang, Kueng & Preskill (2020) — classical shadows (another "shadow" framework)
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes|Berry et al. (2018)]] — fermionic techniques relevant to free-fermion shadow states

---

## Cross-references

- [[Hamiltonian Simulation — Comparison Tables]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Shadow State Compression via Invariance Property]]
- [[Two-Time Correlator Encoding in Shadow States]]
- [[Operator-to-State Mapping for Heisenberg Evolution]]
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
