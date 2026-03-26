> **Source:** Ryan Babbush, Nathan Wiebe, Jarrod McClean, James McClain, Hartmut Neven, Garnet Kin-Lic Chan, *Low-Depth Quantum Simulation of Materials*, arXiv:1706.00023, Phys. Rev. X **8**, 011044 (2018)
> **Links:** [arXiv](https://arxiv.org/abs/1706.00023) · [Journal](https://doi.org/10.1103/PhysRevX.8.011044)
> **Tags:** #quantum-simulation #plane-wave #dual-basis #FFFT #second-quantized #Trotter #Taylor-series #LCU #variational #jellium #periodic-systems #materials

---

## The computational problem

Simulate time evolution $e^{-iHt}$ and estimate ground-state energies for the electronic structure Hamiltonian in a periodic second-quantized setting, where $N$ is the number of spin-orbitals (or plane-wave modes). The particular challenge addressed is that the standard Gaussian orbital basis leads to $O(N^4)$ second-quantized Hamiltonian terms, which dominates both gate counts (for simulation) and shot counts (for variational methods). The paper asks whether there exists a basis where the Hamiltonian has substantially fewer terms without sacrificing expressiveness — and whether the resulting circuits can be implemented with low depth on physically realistic qubit layouts.

## What the paper does

This paper introduces the **plane-wave dual basis** for electronic structure simulation: a second-quantized basis where the potential energy operator is diagonal and the kinetic energy is diagonal in the complementary plane-wave basis. The dual Hamiltonian has only $\Theta(N^2)$ distinct second-quantized terms. Using the [[Fermionic Fast Fourier Transform (FFFT)]] to alternate between the two diagonal representations, Trotter steps can be implemented with $O(N)$ gate depth on a planar lattice — single-step depth that is linear in the system size. This gives overall Trotter simulation depth $O(N^{7/2}/\sqrt{\varepsilon})$ and [[Truncated Taylor Series Simulation]]-based depth $\widetilde{O}(N^{8/3})$ for fixed charge density, both substantially better than prior second-quantized algorithms. Variational measurements also become cheaper: $O(N^4/\varepsilon^2)$ shots for fixed absolute precision (bounded in system size), vs the $O(N^8)$-like counts one gets from standard Gaussian-orbital Hamiltonians. The paper concludes with a proposal to use this setup to simulate the uniform electron gas (jellium) on near-term hardware as a NISQ supremacy target.

## The algorithm / construction

### The plane-wave basis and its dual

In a periodic box of volume $\Omega$, the natural basis consists of plane waves $\phi_k(r) = e^{ik \cdot r}/\sqrt{\Omega}$ labeled by $N$ wavevectors $k \in G$ (some finite reciprocal lattice set). The Hamiltonian in the plane-wave basis is:

$$H_{\rm pw} = \sum_{p,\sigma} \frac{k_p^2}{2} a^\dagger_{p\sigma} a_{p\sigma} + \frac{1}{2\Omega} \sum_{\substack{p,q,r,s,\sigma,\tau \\ p+s=q+r}} \tilde{V}_{p-q} a^\dagger_{p\sigma} a^\dagger_{s\tau} a_{r\tau} a_{q\sigma}$$

where $\tilde{V}_\nu$ are the reciprocal-space Coulomb matrix elements. Momentum conservation reduces the two-body terms: the sum only runs over $(p,q,r,s)$ with $p - q = s - r$, giving $O(N^3)$ terms (still many).

The **plane-wave dual basis** is the discrete Fourier transform of the plane-wave basis: functions $\chi_j(r) = \sum_k e^{ik \cdot r_j} \phi_k(r)/\sqrt{N}$ supported on a dual real-space grid $\{r_j\}$ (related to the discrete variable representation / sinc-DVR). The key property is that the potential energy operator is *diagonal* in the dual basis (under the "collocation" approximation):

$$\langle \chi_j | V(\hat{r}) | \chi_{j'} \rangle \approx V(r_j) \delta_{jj'}$$

Similarly, the kinetic energy operator is diagonal in the plane-wave basis. The dual Hamiltonian therefore splits as:

$$H = \underbrace{T}_{\text{diagonal in pw basis}} + \underbrace{U + V}_{\text{diagonal in dual basis}}$$

where $T$ contains $N$ diagonal kinetic terms, $U$ contains $N$ diagonal on-site Hartree terms, and $V$ contains $O(N^2)$ diagonal density-density interaction terms. Total: $\Theta(N^2)$ distinct second-quantized terms, compared to $O(N^4)$ in the Gaussian basis.

See [[Plane-Wave Dual Basis]].

### The Fermionic Fast Fourier Transform (FFFT)

To alternate between simulating $T$ (easy in the plane-wave basis) and $U + V$ (easy in the dual basis), one needs an efficient circuit that maps between the two. The FFFT is the fermionic analogue of the quantum Fourier transform, acting on the second-quantized mode creation operators as:

$$b^\dagger_j = \frac{1}{\sqrt{N}} \sum_k e^{ik \cdot r_j} a^\dagger_k$$

This can be implemented on a planar qubit lattice in $O(N)$ circuit depth using a decomposition into elementary fermionic swap gates and two-mode mixing gates. See [[Fermionic Fast Fourier Transform (FFFT)]].

The FFFT has $O(N \log N)$ gates but the depth is $O(N)$ — no worse asymptotically than the FFFT gate count as a function of system size.

### Trotter-based simulation

The dual-basis Hamiltonian splits cleanly: $H = T + (U + V)$. Both $T$ and $(U+V)$ are sums of commuting diagonal operators in their respective bases. A single second-order Trotter step of $e^{-iHt}$ becomes:

$$e^{-iH\tau} \approx e^{-iT\tau/2} \cdot \text{FFFT}^\dagger \cdot e^{-i(U+V)\tau} \cdot \text{FFFT} \cdot e^{-iT\tau/2}$$

Circuit depth per step:
- $e^{-iT\tau/2}$: $O(1)$ depth (diagonal, all terms commute)
- FFFT: $O(N)$ depth on planar lattice
- $e^{-i(U+V)\tau}$: $O(N)$ depth (the $O(N^2)$ pairwise $ZZ$ terms can be implemented with $O(N)$ depth on a planar lattice using a structured ordering — Appendix H)

Total depth per Trotter step: $O(N)$.

**Trotter error bound (Appendix G, Eq. 19):** Using commutator-based bounds, the number of steps $r$ needed to simulate time $t$ to precision $\varepsilon$ satisfies:

$$r \in \Theta\!\left(\frac{\eta^2 N^{5/6} \Omega^{5/6} t^{3/2}}{\sqrt{\varepsilon}} \sqrt{1 + \frac{\eta \Omega^{1/3}}{N^{1/3}}}\right)$$

The total Trotter simulation depth is $O(Nr)$ (Eq. 20):

$$\text{depth}_{\rm Trotter} \in O\!\left(\frac{\eta^2 N^{11/6} \Omega^{5/6} t^{3/2}}{\sqrt{\varepsilon}} \sqrt{1 + \frac{\eta \Omega^{1/3}}{N^{1/3}}}\right)$$

At fixed charge density $\rho = \eta/\Omega \in O(1)$, this simplifies to $O(N^{5/3} \eta^{11/6} t^{3/2}/\varepsilon^{1/2})$. Taking $\eta \in O(N)$ (the natural scaling for fixed density) gives the headline result:

$$\text{depth}_{\rm Trotter} \in O\!\left(\frac{N^{7/2} t^{3/2}}{\sqrt{\varepsilon}}\right)$$

This is more than a quadratic improvement over $O(N^{11} t / \varepsilon)$ (Aspuru-Guzik 2005) and $O(N^{8+o(1)} t / \varepsilon)$ (Wecker et al. 2015).

### Taylor-series (LCU) simulation

Apply [[Truncated Taylor Series Simulation]] directly to the dual-basis Hamiltonian with $L = \Theta(N^2)$ terms. The two oracles needed are:

- **select($H$)**: Applies the $\ell$-th LCU term. The diagonal structure means this can be done with $O(N)$ gates.
- **prepare($W$)**: Creates superposition $|W\rangle = \sum_\ell \sqrt{h_\ell/\lambda} |\ell\rangle$ over LCU coefficients. The "database" version uses $O(N^2)$ gates; the "on-the-fly" version (Appendix K) computes the needed integrals on-the-fly.

The 1-norm of the Hamiltonian coefficients (the normalization factor $\lambda$) scales as $\lambda \in O(N^{5/3})$ for fixed density (from the Coulomb sum structure). The total gate count and depth:

| Variant | Gate count | Depth |
|---|---|---|
| Database prepare | $\widetilde{O}(N^4 \lambda) = \widetilde{O}(N^{17/3})$ | — |
| On-the-fly prepare | $\widetilde{O}(N^{11/3})$ | $\widetilde{O}(N^{8/3})$ |

The on-the-fly approach (Appendix K) uses arithmetic circuits to compute matrix elements directly on quantum hardware, avoiding classical precomputation of the full $O(N^2)$ database. This is the best asymptotic circuit depth for electronic structure simulation at the time of publication.

Both variants have $O(\log 1/\varepsilon)$ precision dependence (from the Taylor series) vs $O(1/\varepsilon)$ for Trotter.

### Fewer measurements for variational algorithms

In the dual basis, the Hamiltonian splits into two commuting groups: $\{T\}$ (all diagonal in plane-wave basis) and $\{U, V\}$ (all diagonal in dual basis). This means:

- All $O(N^2)$ potential terms $U_p + V_{pq}$ can be simultaneously measured in a single circuit (dual basis). No sampling over Pauli terms.
- All $N$ kinetic terms can be simultaneously measured after applying FFFT (plane-wave basis).

So instead of $O(N^4)$ terms requiring $O(N^4/\varepsilon^2)$ measurements each (standard Gaussian basis), the total measurement count is bounded by just two groups. The total repetitions needed are:

$$M \in O\!\left(\frac{N^4}{\varepsilon^2}\right) \quad \text{(fixed absolute error } \varepsilon\text{)}$$

In the thermodynamic limit at fixed density, for relative error $\mu$:

$$M \in O\!\left(\frac{N^2}{\mu^2}\right)$$

This is a major improvement over variational algorithms in the Gaussian basis, where the number of distinct measurement bases grows as $O(N^4)$.

See [[Variational Measurement Reduction via Dual Basis]].

### The jellium proposal

The uniform electron gas (jellium) — $\eta$ electrons in a periodic box with a uniform positive background — is both physically relevant (model for metals) and classically difficult at low densities (sign problem). The authors argue it is an ideal NISQ supremacy target:
- Periodic system, natural plane-wave basis
- No pseudopotentials needed
- Quantum Monte Carlo fails at $r_s \gtrsim 10$ (low density)
- A simple low-depth variational ansatz suffices:

```
|ref⟩ = |Slater determinant in dual basis⟩
   → apply FFFT
   → apply e^{-i(U+V)τ} (one layer of ZZ rotations)
   → (optionally repeat with FFFT + kinetic evolution)
```

Translational symmetry reduces the independent parameters from $O(N^2)$ to $O(N)$ (interactions depend only on distance, not absolute position). The ansatz is an adiabatic one-step (or few-step) approach: close enough to the ground state for low-density jellium that quantum advantage might be demonstrable before fault tolerance.

## Key results

**Main result 1 (Trotter depth):** Single Trotter step of the dual-basis electronic structure Hamiltonian in depth $O(N)$ on a planar qubit lattice, using $O(N^2)$ gates. Total simulation depth at fixed density:

$$\text{depth}_{\rm Trotter} \in O\!\left(\frac{N^{7/2} t^{3/2}}{\sqrt{\varepsilon}}\right)$$

**Main result 2 (Taylor-series depth):** Total simulation depth using LCU + on-the-fly prepare:

$$\text{depth}_{\rm LCU} \in \widetilde{O}\!\left(N^{8/3}\right)$$

(fixed density, $O(\log 1/\varepsilon)$ precision scaling)

**Main result 3 (Measurement reduction):** Variational energy estimation in the dual basis requires:

$$M \in O\!\left(\frac{N^4}{\varepsilon^2}\right) \quad \text{or} \quad M \in O\!\left(\frac{N^2}{\mu^2}\right) \text{ (relative, thermodynamic limit)}$$

shots total — regardless of the number of Hamiltonian terms, because all terms in each basis are measured simultaneously.

**Main result 4 (FFFT):** 3D fermionic discrete Fourier transform implementable on a planar qubit lattice in $O(N)$ circuit depth.

## Comparison with prior work

| Paper | Year | Representation | Qubit basis | Gate complexity | Depth | Precision |
|---|---|---|---|---|---|---|
| Aspuru-Guzik et al. (2005) | 2005 | Gaussian orbitals, JW, Trotter | Second-quant | $\widetilde{O}(N^{11} t/\varepsilon)$ | $\widetilde{O}(N^{11}/\varepsilon)$ | $O(1/\varepsilon)$ |
| Wecker et al. (2015) | 2015 | Gaussian orbitals, BK, Trotter | Second-quant | $O(N^{8+o(1)} t/\varepsilon)$ | $O(N^{8+o(1)}/\varepsilon)$ | $O(1/\varepsilon)$ |
| Babbush et al. (2016, NJP) on-the-fly | 2016 | Gaussian orbitals, Taylor series | Second-quant | $\widetilde{O}(N^5 t)$ | — | $O(\log 1/\varepsilon)$ |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018 (CI)]] | 2018 | First-quantized CI matrix | First-quant | $\widetilde{O}(\eta^2 N^3 t)$ | — | $O(\log 1/\varepsilon)$ |
| [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes\|Kivlichan et al. 2018 (swap)]] | 2018 | Gaussian orbitals, JW, Trotter | Second-quant, linear | $N(N-1)/2$ per step | $N$ per step | $O(1/\varepsilon)$ |
| **This work (Trotter, fixed density)** | **2018** | **Plane-wave dual, JW** | **Second-quant, planar** | $\widetilde{O}(N^{7/2}/\sqrt{\varepsilon})$ depth | $\widetilde{O}(N^{7/2}/\sqrt{\varepsilon})$ | $O(1/\sqrt{\varepsilon})$ |
| **This work (LCU, on-the-fly)** | **2018** | **Plane-wave dual, JW** | **Second-quant, planar** | $\widetilde{O}(N^{11/3})$ gates | $\widetilde{O}(N^{8/3})$ | $O(\log 1/\varepsilon)$ |

**Note on comparison:** The Babbush 2018 CI result and this work measure complexity differently (qubits, gate count, depth). The $\widetilde{O}(N^{8/3})$ depth here is the headline number for depth-constrained (NISQ-adjacent) devices; CI result is better in total gate count for fault-tolerant regimes.

## Limits / caveats

**Periodic systems only.** The plane-wave dual basis assumes periodic boundary conditions. It doesn't directly apply to isolated molecules in Gaussian basis sets. The extension to open boundary conditions requires modification.

**The $O(N^2)$ ZZ-rotation layer is not free.** Implementing all $O(N^2)$ pairwise $ZZ$ rotations in $O(N)$ depth on a planar lattice requires a specific parallel schedule (Appendix H). This relies on planar (2D grid) connectivity — not available on all hardware. A linear chain would need $O(N^2)$ depth for this layer, partially negating the advantage.

**Discretization error not the same.** The plane-wave basis has its own basis-set incompleteness error, scaling as $O(1/N)$ for smooth potentials (Appendix E). Near the electron-electron cusp, both Gaussian and plane-wave bases have $O(1/N)$ error — neither is asymptotically better at describing cusps.

**LCU normalization factor $\lambda$.** The $\widetilde{O}(N^{8/3})$ depth relies on the claim that $\lambda \in O(N^{5/3})$ for fixed density. This scaling comes from bounding the sum of Coulomb matrix element magnitudes, which relies on the $r_s$ being held fixed. For molecular (non-periodic) calculations, $\lambda$ scaling may differ.

**Jellium proposal is a variational upper bound.** The near-term jellium simulation is a variational calculation: it gives an upper bound on the ground state energy, not an exact value. The accuracy depends on how well the low-depth ansatz approximates the true ground state. Whether the ansatz is accurate enough to show genuine advantage over classical QMC remains an open question.

**Not directly comparable to [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan et al. 2018 (swap network)]]:** That paper minimises Trotter step depth for Gaussian-orbital second-quantized Hamiltonians on linear chains. This paper minimises total simulation depth for periodic systems on planar lattices. Different settings, different hardware assumptions.

## Reusable ideas

1. [[Plane-Wave Dual Basis]] — Choose a basis where the potential is diagonal and the kinetic energy is diagonal in the Fourier-dual basis, reducing Hamiltonian terms from $O(N^4)$ to $\Theta(N^2)$ with no loss of accuracy for periodic systems.

2. [[Fermionic Fast Fourier Transform (FFFT)]] — Second-quantized analogue of the quantum Fourier transform, mapping between plane-wave and dual bases using fermionic swap gates and mixing unitaries; implementable in $O(N)$ depth on a planar qubit lattice.

3. [[Variational Measurement Reduction via Dual Basis]] — When all Hamiltonian terms in one basis commute (e.g., all potential terms diagonal in the dual basis), measure all of them simultaneously in a single circuit; use the complementary basis for kinetic terms. Reduces required measurement circuits from $O(N^4)$ to 2, cutting shot counts from $O(N^8/\varepsilon^2)$ to $O(N^4/\varepsilon^2)$.

## References within this paper

| Ref | What it is | Vault note? |
|---|---|---|
| Berry, Childs, Cleve, Kothari, Somma (2015), FOCS | Truncated Taylor series simulation — LCU method applied in Sec. II.B | [[Truncated Taylor Series Simulation]] |
| Babbush, Wecker, Aspuru-Guzik, Whitfield (2015) | On-the-fly molecular integral evaluation — basis for Appendix K | [[On-the-fly Molecular Integral Evaluation]] |
| Babbush et al. (2016, NJP 18, 033032) | Second-quantized Taylor series simulation with Gaussian orbitals, $\widetilde{O}(N^5)$ — what this paper improves upon | No (not in vault) |
| Kivlichan, Wiebe, Babbush, Aspuru-Guzik (2017) | Real-space first-quantized simulation; companion work in position basis | [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] |
| Aspuru-Guzik, Dutoi, Love, Head-Gordon (2005), Science | Original quantum chemistry simulation proposal; $\widetilde{O}(N^{11})$ baseline | No |
| Wecker, Bauer, Clark, Hastings, Troyer (2015) | Improved Trotter bounds for Gaussian orbitals; $O(N^8)$ | No |
| Kivlichan, McClean, Wiebe et al. (2018), PRL 120 | [[Fermionic Swap Network]] — Trotter step in depth $N$, linear connectivity | [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] |
| Babbush et al. (2018, CI) | First-quantized CI representation; Taylor series; $\widetilde{O}(\eta^2 N^3 t)$ | [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] |
| Light, Hamilton, Lill (1985) | Discrete variable representation (DVR) — origin of the dual basis construction | No |
| Slater (1930); Kato (1951) | Electron cusp conditions; relevant to basis-set error analysis in Appendix E | No |

## Cross-links

### Paper notes
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — provides the general commutator-scaling bound for product-formula error; the $O(N^3)$ Trotter error norm for the split-operator jellium step (used in Kivlichan 2020) is the concrete input that the Childs et al. framework would analyse; the $O(N^{7/2})$ total T complexity for jellium follows from combining this step's error structure with the commutator bounds
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — earlier work by the same group (Babbush, McClean, Wiebe) on the looseness of Trotter norm bounds for chemical Hamiltonians; the split-operator potential-energy step here has translation-invariant coefficients that lead to analogous cancellation effects
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes]] — symmetry-protection technique that exploits the translation invariance of the jellium/Hubbard Hamiltonians targeted here; the FFFT-based basis changes in the split-operator step are natural candidates for symmetry kicks
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — directly extends and optimises the split-operator Trotter step introduced here; adds Hamming weight phasing and achieves $O(N^2)$ T complexity for jellium with extensive error
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — companion low-depth Trotter approach using the fermionic swap network on linear chains; compared in Kivlichan 2020 where the split-operator step consistently shows $2$–$3\times$ lower Trotter error norm
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — uses the plane-wave dual basis from this paper as the Hamiltonian encoding inside qubitization; achieves T complexity $O(N^3/\varepsilon)$ for spectroscopy; the dual basis is the key ingredient that makes the qubitization approach efficient
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — companion work using Taylor series for first-quantized CI representation; achieves better gate count ($\widetilde{O}(\eta^2 N^3 t)$) at cost of larger qubit count ($\widetilde{O}(\eta)$); the two approaches suit different hardware regimes
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — real-space (position-grid) first-quantized simulation; also aims at periodic systems; compare Theorem 3 ($\widetilde{O}(\eta^2)$ oracle queries, fixed $h$) vs. this work's $\widetilde{O}(N^{8/3})$ depth (fixed density)
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — [[Fermionic Swap Network]] achieves depth $N$ per Trotter step for Gaussian-orbital Hamiltonians on linear chains; complementary setting to this paper's planar-lattice plane-wave approach
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — hardware experiment; motivates low-depth circuits and measurement reduction for near-term devices
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — measurement reduction strategies for VQE in Gaussian basis; compare with dual-basis approach here
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]] — orthogonal measurement reduction strategy (LP over $n$-representability); both are approaches to cutting VQE shot cost
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — "conceptually dual" to this paper: where this paper uses the dual basis in second quantization with $A = U+V$ (large) and $B = T$ (small) in the interaction picture, that paper uses first quantization in momentum space with $A = T$ (large) and $B = U+V$ (small); achieves $\widetilde{O}(\eta^{8/3} N^{1/3} t)$ — sublinear in $N$ and better for molecular systems
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — addresses the state-preparation assumption made in this paper's QPE resource estimates; validates that HF overlap is adequate for most chemical systems
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]] — replicates the diagonal Coulomb structure of this paper's [[Plane-Wave Dual Basis]] for arbitrary molecular orbital bases using THC factorization; matches the $\widetilde{O}(N)$ per-step scaling in arbitrary bases that previously required plane waves
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]] — Generalizes the [[Variational Measurement Reduction via Dual Basis]] idea from $L=1$ (plane-wave dual) to $L = O(N)$ (arbitrary molecular orbital basis) via [[Basis Rotation Grouping for VQE Measurement|double factorization-based measurement grouping]]
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — Uses the [[Plane-Wave Dual Basis]] from this paper as one of two primitive bases for [[DG Blocking for Block-Diagonal Hamiltonians|DG blocking]]; constructs block-diagonal Hamiltonians that interpolate between the $O(N^2)$-term diagonal regime here and the $O(N^4)$-term MO regime
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush 2021]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al 2020]]
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al 2018]]
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven 2019]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush 2019]]
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — extends qubitization to periodic systems in atom-centered (Gaussian) Bloch orbital bases; complementary to this paper's plane-wave approach — Bloch orbitals use fewer basis functions but with more complex integrals
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al 2022]]
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al 2023]]

### Trick cards
- [[Plane-Wave Dual Basis]] — core representation introduced in this paper
- [[Fermionic Fast Fourier Transform (FFFT)]] — key circuit that makes the dual-basis approach practical
- [[Variational Measurement Reduction via Dual Basis]] — measurement efficiency result for variational algorithms
- [[Truncated Taylor Series Simulation]] — LCU method applied in Sec. II.B
- [[On-the-fly Molecular Integral Evaluation]] — used in the on-the-fly prepare variant (Appendix K)
- [[Trotterized Time Evolution for Chemistry]] — Trotter approach in Sec. II.A; compare Trotter depth $O(N^{7/2}/\sqrt{\varepsilon})$ here vs. $N$ per step in [[Fermionic Swap Network]]
- [[Fermionic Swap Network]] — analogous low-depth Trotter idea for linear chains and Gaussian orbitals
- [[Variance Reduction via N-Representability Constraints (LP Method)]] — orthogonal shot-reduction strategy
