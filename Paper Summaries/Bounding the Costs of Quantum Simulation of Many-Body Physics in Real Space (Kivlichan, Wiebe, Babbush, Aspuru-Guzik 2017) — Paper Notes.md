> **Source:** Ian D. Kivlichan, Nathan Wiebe, Ryan Babbush, Alán Aspuru-Guzik, *Bounding the costs of quantum simulation of many-body physics in real space*, arXiv:1608.05696, J. Phys. A: Math. Theor. **50**, 305301 (2017)
> **Links:** [arXiv](https://arxiv.org/abs/1608.05696) · [Journal](https://doi.org/10.1088/1751-8121/aa7b2b)
> **Tags:** #quantum-simulation #first-quantized #real-space #position-basis #LCU #taylor-series #finite-difference #many-body #discretization-error

---

## The computational problem

Simulate time evolution $U(t) = e^{-iHt}$ for $\eta$ interacting particles in $D$ spatial dimensions, with positions $x \in [0, L]^{\eta D}$ (a hypertorus with periodic boundary conditions). The Hamiltonian is:

$$H = T + V, \quad T = -\sum_{i=1}^\eta \frac{\nabla_i^2}{2m_i}, \quad V = V(x)$$

This is a **first-quantized real-space** (position-basis) formulation, as opposed to the second-quantized or CI-orbital approaches of [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. (2018)]] and most quantum chemistry simulation papers.

The space is discretized: each of the $D$ spatial directions is divided into $b$ bins of side length $h = L/b$. The $\eta$-particle position is stored in $\eta D \lceil \log b \rceil$ qubits encoding which bin each coordinate falls in.

**Cost model:** Two primitive operations are costed at 1 query each: (1) a query to the potential energy oracle $\tilde{V}(x)$; (2) a unitary adder circuit. All other resources (state preparation, classical logic, ancilla qubits) are treated as free.

## What the paper does

It applies the [[Truncated Taylor Series Simulation]] (BCCKS) method to simulate real-space many-body dynamics, achieving $\tilde{O}(\eta^2)$ pairwise potential oracle queries for fixed grid spacing — significantly better than the $\tilde{O}(\eta^5)$ scaling of contemporary second-quantized methods. The improvement comes from a controlled swap network that reduces the $\eta D a$ distinct adder terms to a single adder call. However, the paper also proves a sobering discretization analysis showing that, in the worst case, the grid spacing $h$ must be exponentially small in $\eta D$, which can negate the speedup entirely. The honest contribution is: a cleaner complexity analysis with a tight upper bound, plus a warning that the naive comparison with second-quantized methods is incomplete.

## The algorithm / construction

### 1. Discretized Hamiltonian

The continuous kinetic operator is approximated by a $(2a+1)$-point centered finite difference of order $2a$:

$$\tilde{T} = \sum_{i,n} \left( S_{i,n} - \frac{d_{2a+1,j=0}}{2m_i h^2} \mathbf{1} \right)$$

where the finite difference coefficients are:

$$d_{2a+1, j \neq 0} = \frac{2(-1)^{a+j+1}(a!)^2}{(a+j)!(a-j)!j^2}$$

The identity shift $d_{2a+1,j=0}\mathbf{1}$ changes only the global phase, so it is dropped. The key insight (**Lemma 6**) is that the sum of the absolute values of these coefficients is bounded by a *constant* regardless of $a$:

$$\sum_{j=-a, j\neq 0}^{a} |d_{2a+1,j}| < \frac{2\pi^2}{3}$$

This constant bound on the LCU 1-norm is what enables the favourable query complexity. See [[High-Order Finite Difference Kinetic Energy]].

The potential is regularized by replacing Coulomb singularities:

$$V_\text{Coulomb}(x) = \sum_{i<j} \frac{q_i q_j}{\sqrt{\|x_i - x_j\|^2 + \Delta^2}}$$

for some cutoff $\Delta > 0$. This bounds the potential: $\|V_\text{Coulomb}\|_\infty \leq \eta(\eta-1)q^2 / (2\Delta)$. See [[Regularized Coulomb Potential (Delta Cutoff)]].

### 2. LCU decomposition

Approximate the scaled Hamiltonian $M\tilde{H}$ (for integer $M \geq V_\text{max}/\delta$) as a linear combination of unitaries (**Lemma 8**):

$$M\tilde{H} \approx \sum_\chi d_\chi V_\chi$$

where:
- **Kinetic terms** ($2\eta Da$ terms): each $V_\chi$ is a unitary adder $A_j$ that shifts coordinate $x_{i,n}$ by $j$, with $d_\chi = Mh^{-2} d_{2a+1,j}/(2m_i)$
- **Potential terms** ($M$ terms): each $V_\chi$ is a signature matrix (diagonal $\pm 1$), with $d_\chi = V_\text{max}$

Total number of LCU terms: $2\eta Da + M \geq V_\text{max}/\delta$.

The operator 1-norm is $\sum_\chi d_\chi \leq \pi^2\eta D/(3mh^2) + V_\text{max}$.

### 3. Implementing select(V) in O(1) queries

Despite having $2\eta Da + M$ LCU terms, $\text{select}(V)$ can be implemented with $O(1)$ queries via a **controlled swap network** (**Theorem 3 proof**). See [[Controlled Swap Network for Select Oracle]].

For the kinetic terms: the index register $|chi\rangle = |i, n, j\rangle$ encodes which particle, dimension, and shift $j$. A $\lceil \log \eta \rceil$-stage swap tree routes particle $i$'s coordinate to register position 1 (analogous to binary search). Similarly for dimension $n$. Then a single adder by $j$ is applied. Swap tree is then reversed. Total depth: $O(\log \eta D)$; total gates: $O(\eta D \log(L/h))$.

For the potential terms: the same swap technique routes particles $i, j$ to common registers, then a single pairwise oracle call suffices.

### 4. Taylor series simulation

Apply [[Truncated Taylor Series Simulation]] to $M\tilde{H}$ for time $t/M$:

- Split into $r$ segments; for each segment, approximate $e^{-i\tilde{H}t/r}$ by truncated Taylor series to order $K$
- $K \in O(\log(r/\varepsilon) / \log\log(r/\varepsilon))$
- $r \leq \left(\frac{\pi^2\eta D}{3mh^2} + V_\text{max}\right)\frac{t}{\ln 2} + 1$
- Total queries: $O(Kr)$ via the oblivious amplitude amplification of $G = -ARA^\dagger RA$

### 5. Three-layer error accounting

The paper carefully distinguishes three dynamical systems (Fig. 2 in the paper):

```
Continuous H  --[Theorem 4/Cor. 5]--> Discretized H̃  --[Theorem 3]--> Simulated H̃
```

Most prior work only analyzed the Theorem 3 layer. This paper analyzes both, finding that the Theorem 4 layer can dominate.

## Key results

**Theorem 3 (Discrete simulation):** For fixed grid spacing $h$ and bounded $V_\text{max}$:

$$\text{queries} \in O\!\left(\left(\frac{\eta D}{mh^2} + V_\text{max}\right)t \cdot \frac{\log\left(\frac{\eta D t}{mh^2\varepsilon} + \frac{V_\text{max} t}{\varepsilon}\right)}{\log\log(\cdots)}\right)$$

For the regularized Coulomb ($V_\text{max} = \eta(\eta-1)q^2/(2\Delta)$):

$$\text{queries} \in O\!\left(\left(\frac{\eta D}{mh^2} + \frac{\eta^2 q^2}{\Delta}\right)t \cdot \log\left(\frac{\eta Dt}{mh^2\varepsilon} + \frac{\eta^2 q^2 t}{\Delta\varepsilon}\right)\bigg/\log\log(\cdots)\right)$$

**If** $h \in \omega(\eta^{-1})$ then the $\eta^2$ scaling beats the $\tilde{O}(\eta^5)$ of second-quantized methods.

**Corollary 9 (Pairwise oracle):** Replacing the total potential oracle with a pairwise oracle $V_p$:

$$\tilde{O}\!\left(\left(\frac{\eta D}{mh^2} + V_\text{max}\right)t \log(1/\varepsilon)\right)$$

queries — same scaling, confirming that pairwise decomposition doesn't add an extra $\eta^2$ factor.

**Theorem 4 (Worst-case discretization error):** To simulate the *continuous* Hamiltonian $H$ to error $\varepsilon$, requires:

$$h \leq \frac{2\varepsilon}{3\eta D(k_\text{max} + V'_\text{max} t)} \left(\frac{k_\text{max} L}{\pi}\right)^{-\eta D/2}$$

The dependence $(k_\text{max}L/\pi)^{-\eta D/2}$ is **exponential in $\eta D$**. Since complexity scales as $O(h^{-2})$, this gives complexity that can be doubly-exponential in $\eta D$ in the worst case.

**Corollary 5 (With bounded derivatives):** Under the assumption $|\psi^{(r)}(x)| \leq \beta k_\text{max}^r / (\sqrt{2r+1} L^{\eta D/2})$ (satisfied by plane waves; expected for dilute electron gases):

$$h \in O\!\left(\frac{\varepsilon}{\eta D(k_\text{max} + V'_\text{max} t)}\right), \quad a \in O\!\left(\log \frac{\eta^2 D^2 t k_\text{max}(k_\text{max} + V'_\text{max} t)}{m\varepsilon^2}\right)$$

Under this assumption, the discretization cost is polynomial, and for chemistry with fixed $D, k_\text{max}, \Delta$:

$$\text{queries} \in \tilde{O}(\eta^7 t^3 / \varepsilon^2)$$

This is worse than $\tilde{O}(\eta^5)$ from the CI approach. But the error metrics differ — see Limits below.

## Comparison with prior work

| Paper | Year | Approach | Representation | Scaling (fixed h or orbitals ∝ η) | Discretization addressed? |
|---|---|---|---|---|---|
| [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes|Kassal et al. (2008)]] | 2008 | Real-space Trotter | First-quantized, grid | $\tilde{O}(\eta^5)$ | Partially |
| Babbush et al. 2016 (NJP) | 2016 | Taylor series, second-quant, on-the-fly | Second-quantized | $\tilde{O}(\eta^5 t)$ | No |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018]] | 2018 | Taylor series, CI matrix | First-quantized, orbital basis | $\tilde{O}(\eta^5 t)$ (η orbitals ∝ η) | No |
| **This work (fixed h)** | **2017** | **Taylor series, real space** | **First-quantized, position grid** | **$\tilde{O}(\eta^2)$** | **Theorem 3 only** |
| **This work (Corollary 5)** | **2017** | **With discretization** | **First-quantized, position grid** | **$\tilde{O}(\eta^7 t^3/\varepsilon^2)$** | **Yes** |

The $\tilde{O}(\eta^2)$ claim is real but conditional on a fixed grid spacing. Once discretization error is folded in, the comparison becomes unfavorable. The CI and second-quantized methods implicitly handle discretization by choosing basis functions adapted to the system.

## Limits / caveats

**The exponential discretization warning is the key result.** The title says "bounding the costs" — and one of those bounds is a lower bound: you need exponentially many grid points in the worst case.

- **Different error metrics:** Theorem 3 vs. Theorem 4 measure different things. Theorem 3 error is within the fixed discrete Hilbert space; Theorem 4 measures how close the discrete simulation is to the true continuous dynamics. Comparing $\tilde{O}(\eta^2)$ (Theorem 3) with $\tilde{O}(\eta^5)$ (CI methods, also Theorem-3-type) is not comparing the same thing.

- **Plane-wave assumption for benign scaling:** Corollary 5 requires wave function derivatives to be bounded in a way consistent with plane waves. Electrons in a Coulomb potential do *not* satisfy this near a nucleus (cusps). The physically relevant case may be closer to the worst case.

- **$\tilde{O}$ hides $D, k_\text{max}, \Delta$ dependence:** For 3D electrons with $D = 3$, these constants matter significantly.

- **Many qubits for modest precision:** Even with 32 bits per coordinate per particle, storing a single electron's position requires $3 \times 32 = 96$ qubits. For $\eta = 10$ electrons: $\sim 1000$ qubits just for positions. Second-quantized methods need $\sim 100$ qubits for interesting molecules.

- **No state preparation analysis:** The cost model treats state preparation as free. Preparing a good initial state in the position basis (e.g., Hartree-Fock) in this encoding is non-trivial.

- **Coulomb singularities require $\Delta$:** Regularizing the Coulomb potential with $\Delta$ introduces an error of order $q^2/\Delta$ in the energy. Matching this to the overall precision ε constrains $\Delta$ and feeds into the final resource estimates.

## Reusable ideas

1. [[Real-Space Grid Encoding for Many-Body Simulation]] — Encode $\eta$ particle positions on a uniform spatial mesh using $\eta D \lceil \log b \rceil$ qubits; enables first-quantized simulation directly in position space without a second-quantized orbital basis.

2. [[High-Order Finite Difference Kinetic Energy]] — Approximate the Laplacian $\nabla^2$ using a $(2a+1)$-point centered finite difference with exponentially decreasing error in $a$. The finite difference coefficients have LCU 1-norm bounded by $2\pi^2/3$ (independent of $a$), enabling favourable query complexity.

3. [[Regularized Coulomb Potential (Delta Cutoff)]] — Replace $\|x_i - x_j\|^{-1}$ with $(\|x_i - x_j\|^2 + \Delta^2)^{-1/2}$ to bound the potential operator norm at $\eta(\eta-1)q^2/(2\Delta)$. Required for any LCU decomposition of a real-space Coulomb Hamiltonian.

4. [[Controlled Swap Network for Select Oracle]] — Use a $\lceil \log \eta \rceil$-depth binary-search swap tree to route any particle register to a common position, reducing $O(\eta D)$ distinct adder terms to a single adder call. The circuit is $O(\eta D \log(L/h))$ gates but depth $O(\log \eta D)$.

## References within this paper

| Ref | What it is | Vault note? |
|---|---|---|
| Berry, Childs, Cleve, Kothari, Somma (2015), FOCS | Truncated Taylor series simulation — the core LCU method applied here | No (described in [[Truncated Taylor Series Simulation]]) |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush et al. (2018)]], arXiv:1506.01029 | CI-representation Taylor series simulation (first-quantized orbital basis) — companion paper using same LCU technique | [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] |
| Kassal, Whitfield, et al. (2008) | Earlier real-space simulation via quantum Fourier transform and Jordan's algorithm; $\tilde{O}(\eta^5)$ | No |
| Wiesner (1996), Zalka (1998) | Original grid-based real-space encoding; the encoding this paper uses | No |
| Lidar & Wang (1999) | Position-space simulation of the Hubbard model using Wiesner encoding | No |
| Somma (2015, 2016) | Simulation of harmonic/quartic oscillators; highlights difficulty of LCU for unbounded Hamiltonians | No |
| Babbush et al. (2016), NJP 18, 033032 | Second-quantized Taylor series simulation (on-the-fly, $\tilde{O}(\eta^5)$) — what this paper improves upon (for fixed h) | No |
| Kato (1951) | Kato's theorem on electron gas density of states; physical argument for benign discretization scaling | No |
| Aspuru-Guzik et al. (2005), Science 309 | Original quantum chemistry simulation proposal | No |

## Cross-links

### Paper notes
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — companion paper using same [[Truncated Taylor Series Simulation]] for first-quantized simulation but in the orbital/CI basis rather than real space; achieves $\tilde{O}(\eta^2 N^3 t)$ with $\tilde{O}(\eta)$ qubits
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — hardware experiment using Trotter; motivates why better simulation algorithms (both LCU-based) are needed
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — overlapping authors (Babbush, Wiebe); sister paper using second-quantized plane-wave dual basis for periodic systems; both apply LCU/Taylor series to periodic-boundary Hamiltonians but use entirely different representations (first-quantized position grid here vs. second-quantized dual basis there)
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — qubitization-based approach; cites this paper's real-space cost analysis as motivation for better algorithms
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]] — first-quantized plane-wave simulation that achieves $\widetilde{O}(\eta^{8/3} N^{1/3} t)$; notes that real-space grids lack the Galerkin variational error bounds that plane-wave bases provide
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — same first author (Kivlichan); provides efficient Trotter circuits for the second-quantized setting that complement this paper's first-quantized LCU approach
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush 2021]]
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes|Babbush, Berry, Kothari, Somma, Wiebe 2023]]
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al 2023]]
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — third-generation successor in the first-quantized plane-wave line; adds GTH pseudopotentials and non-cubic cells to the block encoding established via the first-quantized framework this paper helped motivate
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes]] — takes the second-quantized Bloch orbital route to periodic materials; the controlled swap network idea from this paper's SELECT oracle reappears as momentum-register swaps for periodic block encodings
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]] — applies SVD-based basis compression to the plane-wave dual (the momentum-space analogue of this paper's position-space grid), compressing it to a block-local form that shares the $O(N^2)$ operator scaling of this paper's diagonal Hamiltonian

### Trick cards
- [[Real-Space Grid Encoding for Many-Body Simulation]] — foundational encoding for this paper
- [[High-Order Finite Difference Kinetic Energy]] — key technique for treating kinetic energy as LCU of adders
- [[Regularized Coulomb Potential (Delta Cutoff)]] — bounding the Coulomb singularity for LCU
- [[Controlled Swap Network for Select Oracle]] — implementation trick enabling O(1)-query select(V)
- [[Truncated Taylor Series Simulation]] — core simulation method applied throughout
- [[Trotterized Time Evolution for Chemistry]] — what this paper improves on (for fixed h); the Trotter approach to real-space simulation had $\tilde{O}(\eta^5)$
- [[1-Sparse Hamiltonian Decomposition via Graph Coloring]] — analogous LCU decomposition strategy in the orbital basis; compare with the finite-difference + adder decomposition used here
