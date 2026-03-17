> **Paper:** Block encoding with low gate count for second-quantized Hamiltonians
> **arXiv:** [2510.08644](https://arxiv.org/abs/2510.08644)
> **Authors:** Diyi Liu, Shuchen Zhu, Lin Lin, Guang Hao Low, Chao Yang (Berkeley / Duke / Georgetown / Google)
> **Date read:** 2026-03-13
> **Tags:** #block-encoding #qubitization #qsvt #fermions #resource-estimation

---

## One-Line Take

For second-quantized Hamiltonians with $L$ interaction terms, standard block-encoding methods pay $O(L)$ T gates in the PREPARE oracle. This paper cuts that to $\tilde{O}(\sqrt{L})$ via two ideas: a SWAP-based data lookup for the sparsity oracle, and a comparator-based direct sampling method for the amplitude oracle. A bonus: the $\eta$-particle subspace construction reduces the subnormalization factor from $O(L)$ to $O(\sqrt{L})$, which directly improves downstream simulation cost.

---

## The Input Model

The paper encodes a second-quantized Hamiltonian as:
$$H = \sum_{\text{terms}} h_{pq\ldots} a_p^\dagger a_q \ldots$$

Two oracles are needed for block-encoding via LCU / SELECT-PREPARE:
- **Sparsity oracle $O_C$:** Given a basis state, identify which Hamiltonian terms have nonzero matrix elements. Implemented via a SWAP-based lookup (uses the structure of creation/annihilation operators on the occupation basis).
- **Amplitude oracle $O_A$:** Prepare the state proportional to the coefficient vector $(h_1, h_2, \ldots, h_L)$. Implemented via SELECT-SWAP with direct comparator sampling.

The two oracle architectures are deliberately different: $O_C$ uses SWAP alone (matching the binary-tree structure of sparsity), while $O_A$ uses SELECT-SWAP (the sublinear-T trick for amplitude loading).

---

## The $\tilde{O}(\sqrt{L})$ T-Count Trick

Standard alias sampling for $O_A$ costs $O(L)$ Toffoli gates (one per coefficient). The SELECT-SWAP approach batches $\lambda$ entries per lookup:
- Load $L/\lambda$ "group" pointers: cost $O(L/\lambda)$ T gates
- Resolve within a group of size $\lambda$: cost $O(\lambda \cdot m_b)$ T gates (where $m_b$ = precision bits per coefficient)

Balancing: set $\lambda = \sqrt{L/m_b}$, giving T-count $O(\sqrt{L \cdot m_b})$. Since $m_b = O(\log(L/\varepsilon))$, this gives $\tilde{O}(\sqrt{L})$.

The comparator-based direct sampling for $O_A$ encodes coefficient magnitudes as $m_b$-bit fixed-point numbers and uses coherent comparators (comparing an ancilla uniform superposition against coefficient bits) to implement the amplitude state, without bespoke controlled rotations.

---

## Comparison with Prior Work (from paper Table 1)

### Full Hilbert space, general Hamiltonian ($L \sim n^4$ terms, $n$ spin orbitals):

| Method | T-count | T-depth |
|--------|---------|---------|
| Babbush et al. 2018 | $O(n^4 + \log(n^4/\varepsilon))$ | $O(n^4)$ |
| **This work** | $O(n^2\sqrt{\log(n^4/\varepsilon)})$ | $O(n^2\sqrt{\log(n^4/\varepsilon)})$ |

### $\eta$-particle subspace, general Hamiltonian:

| Method | Subnormalization $\alpha$ | T-count |
|--------|--------------------------|---------|
| Babbush et al. 2018 | $O(n^4)$ | $O(n^4 + \log(n^4/\varepsilon))$ |
| **This work** | $O(n^2\eta^2)$ | $O(n^2\sqrt{\log(n^2\eta^2/\varepsilon)})$ |

The subnormalization reduction ($O(n^4)$ → $O(n^2\eta^2)$) matters because simulation cost scales linearly with $\alpha$. For $\eta \ll n$ (typical in chemistry), this is a significant win.

### Translation-invariant factorized Hamiltonian:

| Method | Subnormalization | T-count |
|--------|-----------------|---------|
| Babbush et al. 2018 | $O(n^2)$ | $O(n + \log(n^2/\varepsilon))$ |
| **This work** | $O(n^2)$ | $O(n + \sqrt{n\log(n^2/\varepsilon)})$ |

---

## The $\eta$-Particle Subspace Construction

For fixed particle number, only states in the $\eta$-particle sector are physically relevant. The full-space block-encoding includes many transitions that violate particle conservation and are never accessed — but their coefficients still contribute to the subnormalization $\alpha$.

The fix: construct the oracles so they only generate transitions that respect particle conservation. This requires:
1. **Indirect diffusion** over the occupied-index set: instead of diffusing over all $n$ orbital indices, diffuse only over the $\eta$ occupied ones. Uses an occupation-detection oracle to map rank $i$ → occupied orbital index.
2. **State-dependent oracles** $O_C$ and $O_A$ conditioned on the occupation pattern.
3. **Occupation oracle $O_\text{occ}$**: Given rank $i$, return the $i$-th occupied spin-orbital index. Implemented via comparators over the occupation register.

The reduced support shrinks the effective PREPARE state from $L$-dimensional to $O(\eta^2 n^2)$-dimensional for two-body interactions, giving the reduced subnormalization.

---

## SWAP-UP Fermionic Signs

Jordan-Wigner encoding assigns fermionic signs via parity strings: $a_p^\dagger = Z^{\otimes p-1} \otimes X^+$. Computing parity for a creation/annihilation operator at position $p$ naively requires an $O(p)$-qubit controlled operation.

The paper's approach: route the target register to a canonical position using SWAP operations (SWAP-UP), apply compact sign/parity logic at that fixed position, then route back. This replaces variable-length parity strings with fixed-size operations at the cost of SWAP depth. For sparse operators on architectures with reasonable connectivity, this saves T gates at the cost of Clifford overhead.

---

## Extensions to Structured Hamiltonians

The paper also handles:
- **Translation-invariant Hamiltonians:** Coefficient tensor has periodic structure → further compression of $O_A$.
- **Hamiltonians with decaying coefficients:** Exploits spatial locality to reduce oracle sizes.
- **Products of number operators:** Special case with efficient dedicated circuits.

Concrete examples: nearest-neighbor Hubbard model, molecular electronic Hamiltonian with localized orbitals, extended Hubbard model.

---

## Caveats

- The T-count improvement trades off against Clifford gate count and routing overhead. Clifford gates are "free" in fault-tolerant resource estimates — but ancilla overhead and routing on constrained hardware can still dominate.
- The $\eta$-sector oracle adds oracle complexity: $O_\text{occ}$ requires an extra pass over the occupation register, and the uncomputation structure is nontrivial.
- Practical improvement depends on $n$ vs $\eta$: for $\eta \sim n$ (half-filling), the subnormalization benefit vanishes.
- Paper focuses on T-count, which is dominant for fault-tolerant architectures. NISQ devices have different cost models.

---

## Reusable Tricks

- [[SELECT-SWAP Grouped Data Lookup for Block-Encoding]]
- [[SWAP-UP Routing for Fermionic Sign Operations]]
- [[Comparator-Based Direct Amplitude Sampling from Fixed-Point Coefficients]]
- [[Indirect Diffusion over Occupied-Index Sets]]
- [[Occupation-Detection Assisted Subspace Block-Encoding]]

---

## References within this paper

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs & Wiebe (2012)]] — [[Linear Combination of Unitaries (LCU)|LCU]] / block-encoding foundation
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization that uses the block-encodings constructed here
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — [[QSVT Meta-Template|QSVT]]
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes|Berry et al. (2018)]] — related fermionic block-encoding techniques
- Lee et al. (2021, PRX Quantum 2, 030305) — double factorization for electronic structure

---

## Cross-References

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
