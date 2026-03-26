> **Source:** Ryan Babbush, Dominic W. Berry, Jarrod R. McClean, Hartmut Neven, *Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size*, arXiv:1807.09802, npj Quantum Information **5**, 92 (2019)
> **Links:** [arXiv](https://arxiv.org/abs/1807.09802) · [Journal](https://doi.org/10.1038/s41534-019-0199-y)
> **Tags:** #quantum-simulation #quantum-chemistry #first-quantized #interaction-picture #plane-wave #LCU #momentum-space #fault-tolerant #sublinear

---

## The computational problem

Simulate the electronic structure Hamiltonian $H = T + U + V$ in a first-quantized, momentum-space (plane-wave orbital) representation. The system has $\eta$ electrons and $N$ plane-wave orbitals. The state is encoded in $\eta$ registers, each of size $\log N$, representing the occupied orbitals for each particle:

$$|\psi\rangle = \sum_{p_\ell \in G} \alpha_{p_1 \cdots p_\eta} |p_1 \cdots p_\eta\rangle$$

where $G = [-N^{1/3}/2, N^{1/3}/2]^3 \subset \mathbb{Z}^3$ labels the $N$ plane-wave modes. Antisymmetry is enforced in the wavefunction rather than the operators (as in [[CI Matrix Simulation (First-Quantized Encoding)|first-quantized CI]]), so swapping electron registers induces a phase of $-1$.

The Galerkin plane-wave Hamiltonian has three parts:

$$T = \sum_{j=1}^\eta \sum_{p \in G} \frac{\|k_p\|^2}{2} |p\rangle\langle p|_j$$

$$U = -\frac{4\pi}{\Omega} \sum_{\ell=1}^L \sum_{j=1}^\eta \sum_{\substack{p,q \in G \\ p \neq q}} \frac{\zeta_\ell e^{ik_{q-p} \cdot R_\ell}}{\|k_{p-q}\|^2} |p\rangle\langle q|_j$$

$$V = \frac{2\pi}{\Omega} \sum_{\substack{i,j=1 \\ i \neq j}}^\eta \sum_{p,q \in G} \sum_{\substack{\nu \in G_0 \\ (p+\nu) \in G \\ (q-\nu) \in G}} \frac{1}{\|k_\nu\|^2} |p+\nu\rangle\langle p|_i \otimes |q-\nu\rangle\langle q|_j$$

where $k_p = 2\pi p / \Omega^{1/3}$, $\Omega$ is the computational cell volume, $R_\ell$ and $\zeta_\ell$ are nuclear positions and charges, $L$ is the number of nuclei, and $G_0 = [-N^{1/3}, N^{1/3}]^3 \setminus \{(0,0,0)\}$ is the set of possible momentum transfers.

Goal: simulate time evolution $e^{-iHt}$ (or perform phase estimation) to precision $\varepsilon$ with gate complexity sublinear in $N$.

---

## What the paper does

This paper achieves gate complexity $\widetilde{O}(N^{1/3} \eta^{8/3})$ for electronic structure simulation — sublinear in the number of plane-wave orbitals $N$. The prior best using plane waves was $\widetilde{O}(N^{8/3}/\eta^{2/3})$ (Low & Wiebe 2018, second-quantized). The ratio of old to new complexity is $N^{7/3}/\eta^{10/3}$, which is enormous in the regime $N \gg \eta$ necessary for chemical accuracy in molecules.

The improvement comes from combining two choices: (1) working in **first quantization** so the qubit count is $O(\eta \log N)$ instead of $N$, and (2) applying the **interaction picture** with the kinetic operator $T$ as the large piece and the potential $B = U+V$ as the perturbation. In first-quantized momentum space, $\|T\| \gg \|U+V\|$ when $N \gg \eta$ — the reverse of the second-quantized setting exploited by Low & Wiebe. The LCU 1-norm of the potential is $\lambda = O(\eta^{5/3} N^{1/3})$, much smaller than the kinetic norm $\|T\| = O(\eta N^{2/3}/\Omega^{2/3})$. Since simulation cost scales with $\lambda t$ (not $\|T\|t$), this gives the sublinear-in-$N$ result.

The improvement over the [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|CI matrix approach]] (which achieves $\widetilde{O}(\eta^2 N^3)$) is: ratio = $\eta^2 N^3 / (N^{1/3} \eta^{8/3}) = \eta^{2/3} N^{8/3}$. For $N=100\eta$: ratio $= \eta^{2/3} (100\eta)^{8/3} = 100^{8/3} \eta^{10/3} \approx 2\times10^5 \cdot \eta^{10/3}$. Enormous for any realistic $\eta$ and $N$.

---

## The algorithm / construction

### 1. First-quantized momentum-space encoding

The $\eta$-electron state is encoded in $\eta$ registers, each holding $\log N$ qubits (a 3D momentum vector index). Antisymmetry is maintained by initializing in an antisymmetric state (preparable with $O(\eta \log\eta \log N)$ gates, using Berry et al. 2018) and observing that fermionic Hamiltonians commute with the permutation operator, so antisymmetry is preserved under time evolution.

This is fundamentally different from second-quantized encodings: no Jordan–Wigner or [[Bravyi-Kitaev Transformation]] mapping is needed. The qubit count is $O(\eta \log N)$ rather than $N$.

See [[First-Quantized Plane-Wave Chemistry Encoding]].

### 2. Interaction picture decomposition

Set $A = T$ (kinetic) and $B = U + V$ (external + two-body potential). The key observation: in first-quantized momentum space with $\Omega \propto \eta$ (volume proportional to electron count, as appropriate for molecules),

$$\|T\| = O\!\left(\frac{\eta N^{2/3}}{\Omega^{2/3}}\right) = O\!\left(\frac{N^{2/3}}{\eta^{1/3}}\right) \quad \text{and} \quad \lambda_{B} = O\!\left(\frac{\eta^{5/3} N^{1/3}}{1}\right)$$

so $\|T\| \gg \|B\|$ when $N \gg \eta$. Simulating in the rotating frame of $T$ (the kinetic frame) avoids paying cost proportional to $\|T\|$.

The interaction-picture approach (Low & Wiebe 2018) approximates $e^{-i(A+B)t}$ via the time-ordered Dyson expansion:

$$e^{-i(A+B)t} \approx \sum_{k=0}^{K-1} (-i)^k \int_0^t dt_1 \int_{t_1}^t dt_2 \cdots \int_{t_{k-1}}^t dt_k\, \mathcal{I}_k$$

$$\mathcal{I}_k = e^{-iA(t-t_k)} B\, e^{-iA(t_k - t_{k-1})} B \cdots e^{-iA(t_2-t_1)} B\, e^{-iAt_1}$$

This is implemented via LCU with oblivious amplitude amplification, using $K = O(\log(\lambda t/\varepsilon)/\log\log(\lambda t/\varepsilon))$. The number of time segments is $O(\lambda t)$, giving total complexity (number of $e^{-iA\tau}$ and LCU applications of $B$):

$$O\!\left(\lambda t \frac{\log(\lambda t/\varepsilon)}{\log\log(\lambda t/\varepsilon)}\right)$$

See [[Interaction Picture Simulation (Kinetic Frame)]].

### 3. LCU decomposition of $B = U + V$

Write $B = \sum_s w_s H_s$ with $H_s$ unitary and $w_s \geq 0$. The terms in $U$ and $V$ would naturally not be unitary when the momentum shift $\nu$ would push an index outside $G$. This is handled by a sign-parity cancellation trick: sum over an extra bit $x \in \{0,1\}$ with alternating sign $(-1)^x$, and activate the phase flip when the index leaves $G$. For example, for $U$:

$$U = \sum_{\nu \in G_0} \sum_\ell \frac{2\pi\zeta_\ell}{\Omega \|k_\nu\|^2} \sum_{j=1}^\eta \sum_{x \in \{0,1\}} \left(-e^{-ik_\nu \cdot R_\ell} \sum_{p \in G} (-1)^{x[(p-\nu)\notin G]} |p-\nu\rangle\langle p|_j\right)$$

When $(p-\nu) \notin G$, the two $x$ terms contribute $+1 - 1 = 0$, canceling the invalid term. The operators inside the parentheses are unitary (they act on all of $G$ with appropriate wrapping/cancellation). Similarly for $V$.

See [[LCU Sign-Parity Cancellation for Grid Boundaries]].

### 4. Bounding $\lambda$ via a 3D Coulomb integral

The 1-norm of $B$ is dominated by the two-electron term $V$. The weight for each $\nu \in G_0$ is proportional to $1/\|k_\nu\|^2 \propto \Omega^{2/3}/\|\nu\|^2$. Summing over all $\nu$ in $G_0$ and bounding by a radial integral:

$$\frac{1}{\Omega} \sum_{\nu \in G_0} \frac{\Omega^{2/3}}{\nu^2} \leq \frac{1}{\Omega} \int_0^{2\pi} d\phi \int_0^\pi d\theta \int_0^{N^{1/3}} dr\, \frac{\Omega^{2/3} r^2 \sin\theta}{r^2} = O\!\left(\frac{N^{1/3}}{\Omega^{1/3}}\right) = O\!\left(\frac{N^{1/3}}{\eta^{1/3}}\right)$$

The $r^2$ from the Jacobian exactly cancels the $r^2$ in the $1/\nu^2$ denominator, so the integral picks up only the $N^{1/3}$ from the upper limit. Multiplied by $\eta^2$ (from the $\eta(\eta-1)$ electron pairs) gives:

$$\lambda = O\!\left(\eta^{5/3} N^{1/3}\right)$$

This is the key scaling result. The $N^{1/3}$ dependence — instead of $N^{5/3}$ in the second-quantized dual-basis setting — is why the algorithm achieves sublinear scaling in $N$.

### 5. Implementing $e^{-iT\tau}$

In the momentum-space first-quantized representation, $T$ is diagonal:

$$e^{-iT\tau} = \sum_{p_\ell \in G} \exp\!\left[-\frac{i\tau}{2} \sum_{j=1}^\eta \|k_{p_j}\|^2\right] |p_1\rangle\langle p_1|_1 \cdots |p_\eta\rangle\langle p_\eta|_\eta$$

Implementation: iterate through the $\eta$ electron registers, compute $\sum_j \|k_{p_j}\|^2$ into an accumulator (each squaring costs $O(\log^2 N)$ with elementary multiplication), then apply a controlled phase rotation. Total cost: $O(\eta \log^2 N)$ gates.

### 6. Select oracles for $U$ and $V$

**For $V$ (two-electron term):** The SELECT oracle acts as

$$\text{SELECT}\, |0\rangle|i\rangle|j\rangle|\nu\rangle|p_1\rangle_1 \cdots |p_\eta\rangle_\eta \mapsto |0\rangle|i\rangle|j\rangle|\nu\rangle |p_1\rangle_1 \cdots |p_i+\nu\rangle_i \cdots |p_j-\nu\rangle_j \cdots |p_\eta\rangle_\eta$$

Iterate through all $\eta$ registers, check if index equals $i$ or $j$, and conditionally add or subtract $\nu$. Complexity: $O(\eta \log N)$.

**For $U$ (nuclear term):** The SELECT oracle applies

$$\text{SELECT}\, |1\rangle|\ell\rangle|j\rangle|\nu\rangle|p_1\rangle_1 \cdots |p_\eta\rangle_\eta \mapsto -e^{-ik_\nu \cdot R_\ell} |1\rangle|\ell\rangle|j\rangle|\nu\rangle |p_1\rangle_1 \cdots |p_j - \nu\rangle_j \cdots |p_\eta\rangle_\eta$$

Additional cost: $O(\log N \log(1/\delta_R))$ to compute the dot product $k_\nu \cdot R_\ell$ and apply the phase rotation; $O(L \log(1/\delta_R))$ to access nuclear positions. Total: $O(\eta \log N + L\log(1/\delta_R))$.

**PREPARE oracle:** Creates the weighted superposition over index register $(s, \nu, i, j/\ell)$ with weights proportional to $\|k_\nu\|^{-2}$. This requires a uniform superposition over $\nu \in G_0$ (using $O(\log N)$ qubits for the 3D index) and a QROM lookup for the weights. The total PREPARE cost is absorbed into the $O(\eta \log N)$ per oracle call.

### 7. Total complexity

Combining $\lambda = O(\eta^{5/3} N^{1/3})$ with $O(\lambda t)$ segments and per-segment cost $O(\eta \log^2 N)$:

$$\text{Gate complexity} = \widetilde{O}\!\left(\eta^{8/3} N^{1/3} t\right)$$

The qubit count is $O(\eta \log N)$ for the particle registers, plus $O(\log N)$ ancilla for the LCU control and orbital arithmetic.

### 8. Qubitization alternative

The paper briefly describes how [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] could achieve a different scaling:

$$\widetilde{O}\!\left(\eta^{4/3} N^{2/3} + \eta^{8/3} N^{1/3}\right)$$

The first term dominates when $N \ll \eta^{4/3}/\eta = \eta^{1/3}$, the second when $N \gg \eta$. For molecular applications with $N \gg \eta$, the interaction picture approach gives the same $\eta^{8/3} N^{1/3}$ scaling, so neither is uniformly better — but the interaction picture version has cleaner constants and a simpler implementation.

---

## Key results

**Main result:** Gate complexity for simulating electronic structure in a first-quantized plane-wave basis:

$$\boxed{\widetilde{O}\!\left(N^{1/3} \eta^{8/3} t\right)}$$

with qubit count $O(\eta \log N)$.

**LCU 1-norm:**

$$\lambda = O\!\left(\eta^{5/3} N^{1/3}\right)$$

derived by bounding the 3D Coulomb lattice sum via a radial integral (Eq. 13).

**Per-oracle costs:**
- $e^{-iT\tau}$: $O(\eta \log^2 N)$ gates
- SELECT: $O(\eta \log N)$ gates
- PREPARE: $O(\eta \log N + L\log(1/\delta_R))$ gates

**Alternative (qubitization):**

$$\widetilde{O}\!\left(\eta^{4/3} N^{2/3} + \eta^{8/3} N^{1/3}\right)$$

---

## Comparison with prior work

| Paper | Representation | Qubits | Gate complexity | Prior / This work |
|---|---|---|---|---|
| Aspuru-Guzik et al. 2005 | Second-quant, JW, Trotter | $O(N)$ | $\widetilde{O}(N^{11} t/\varepsilon)$ | Prior |
| Wecker et al. 2015 | Second-quant, BK, Trotter | $O(N)$ | $O(N^{8+o(1)} t/\varepsilon)$ | Prior |
| Berry et al. (QSVT) 2019 | Second-quant, Gaussian | $O(N)$ | $\widetilde{O}(N^4 t)$ | Concurrent |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018]] | Second-quant, dual basis, qubitization | $O(\log(\lambda N/\varepsilon))$ | $O(N^3/\varepsilon)$ T gates | Prior (same group) |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018 (dual basis)]] | Second-quant, dual basis, LCU | $N$ | depth $\widetilde{O}(N^{8/3})$ | Prior (same group) |
| Low & Wiebe 2018 | Second-quant, plane-wave, interaction picture | $O(N \log N)$ | $\widetilde{O}(N^{8/3}/\eta^{2/3})$ | **Prior best for plane waves** |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018 (CI)]] | First-quant, CI matrix, Taylor series | $\widetilde{O}(\eta)$ | $\widetilde{O}(\eta^2 N^3 t)$ | Prior (same group) |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes\|Kivlichan et al. 2017]] | First-quant, real-space grid, Taylor | $\eta D \lceil\log b\rceil$ | $\widetilde{O}(\eta^2 t)$ (pairwise oracle) | Prior |
| **This work** | **First-quant, plane-wave, interaction picture** | **$O(\eta \log N)$** | **$\widetilde{O}(N^{1/3} \eta^{8/3} t)$** | **—** |

**Key comparison:** Against Low & Wiebe 2018 (the immediate predecessor for plane-wave simulation), this paper improves by a factor of $N^{7/3}/\eta^{10/3}$. For molecules where $N \approx 100\eta$ (roughly 100 plane waves per electron for chemical accuracy), this is an improvement of $100^{7/3}/\eta^{10/3-1} = 100^{7/3} \cdot \eta^{-7/3}$. For $\eta = 100$ electrons, that's a speedup of about $100^{7/3}/100^{7/3} = 1$... actually: old is $N^{8/3}/\eta^{2/3}$, new is $N^{1/3}\eta^{8/3}$, ratio is $N^{7/3}/\eta^{10/3}$. With $N=100\eta$: ratio = $(100\eta)^{7/3}/\eta^{10/3} = 100^{7/3} \cdot \eta^{7/3}/\eta^{10/3} = 100^{7/3}/\eta$. So for $\eta=100$, the improvement is $100^{7/3}/100 = 100^{4/3} \approx 460$. For $\eta=50$ it's $100^{7/3}/50 \approx 930$. This is not incremental — and the advantage grows as $\eta$ decreases (fewer electrons, more improvement).

Against the [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|CI matrix approach]], this paper wins when $N \gg \eta^{2/3}$: setting the two complexities equal, $\eta^{8/3} N^{1/3} = \eta^2 N^3$, gives $N^{8/3} = \eta^{2/3}$, i.e., $N = \eta^{1/4}$. Since physical applications have $N \gg \eta \gg \eta^{1/4}$, this paper wins by an enormous margin for all realistic problems.

The intuitive reason: second-quantized encodings waste qubits and norm on states with $\eta \neq $ const. First-quantized momentum-space pins the electron count at $\eta$, which is why $\|T\| \gg \|B\|$ in this representation (large $N$ means high kinetic energy in second quantization because states with up to $N$ electrons are included, but first quantization has exactly $\eta$ electrons with bounded kinetic energy per particle).

---

## Limits / caveats

**Large $N$ required for accuracy.** The algorithm is most efficient for large $N$, but large $N$ is necessary to suppress basis-set discretization error. Plane waves converge as $O(1/N)$ for smooth potentials, but need roughly $100\times$ more terms than Gaussian orbitals to reach chemical accuracy for molecules. So the regime where this algorithm is efficient ($N \gg \eta$) is exactly the regime required for accurate molecular calculations. Whether the scaling in $N$ is still favorable after accounting for the larger absolute $N$ values vs. Gaussian orbitals is system-dependent.

**Periodic boundary conditions.** The Galerkin plane-wave Hamiltonian (Eqs. 3–5) uses periodic boundary conditions in a cubic cell. Extension to non-orthogonal unit cells is possible but adds implementation complexity.

**Antisymmetry overhead.** In second quantization, antisymmetry is free (it's baked into the operator algebra). In first quantization, it must be maintained explicitly. Initial-state preparation costs $O(\eta \log\eta \log N)$ and requires that the input state is properly antisymmetrized. This is not a leading-order cost, but it adds circuit complexity.

**No Born-Oppenheimer approximation required — but nuclei add cost.** The algorithm handles $L$ nuclei via the $U$ term (external potential). Computing nuclear phases $e^{-ik_\nu \cdot R_\ell}$ for $L$ nuclei adds $O(L \log(1/\delta_R))$ cost per oracle call. For molecules, $L \leq \eta$ so this doesn't worsen the leading $O(\eta)$ scaling, but precision in nuclear positions ($\delta_R$) adds polylog overhead.

**Constant factors not estimated.** Unlike [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. 2018]], this paper gives asymptotic scaling only — no T-gate counts, no surface-code resource estimates. The absolute costs for specific molecules are unknown. **Update:** The follow-up paper [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] provides the full constant-factor compilation, with $\sim 1000\times$ circuit-level improvements and concrete resource estimates showing first-quantized qubitization outperforming second-quantized Gaussian methods.

**The sublinear scaling has a catch.** To suppress discretization error, $N$ must be large. The $O(\eta \log N)$ qubit count is excellent, but the $N^{1/3}$ gate-complexity dependence means that using $N = 100\eta$ costs $(100\eta)^{1/3} = 4.6 \eta^{1/3}$ times more than $N = \eta$. For $\eta = 50$, this is about a $17\times$ overhead in gate count just from the basis size.

---

## Reusable ideas

1. [[First-Quantized Plane-Wave Chemistry Encoding]] — Use $\eta$ registers of $\log N$ qubits to encode a first-quantized many-body wavefunction in momentum space; maintain antisymmetry in the wavefunction rather than operators; qubit count $O(\eta \log N)$ vs $N$ for second-quantized encodings.

2. [[Interaction Picture Simulation (Kinetic Frame)]] — In first-quantized momentum space, choose $A = T$ and $B = U+V$ so that $\|T\| \gg \lambda_B = O(\eta^{5/3} N^{1/3})$; simulate in the rotating frame of $T$ using the interaction picture (Low & Wiebe 2018); cost scales with $\lambda_B t$ not $\|T\|t$; gives sublinear-in-$N$ gate complexity.

3. [[LCU Sign-Parity Cancellation for Grid Boundaries]] — When LCU terms involve momentum shifts that can push grid indices out of bounds, introduce a parity bit $x \in \{0,1\}$ with sign $(-1)^x$ and activate the sign flip on boundary violations; the two $x$ terms cancel ($+1 - 1 = 0$) wherever the shift is invalid, preserving unitarity without extra projection or checking circuits.

---

## References within this paper

| Reference | What it is | Vault note? |
|---|---|---|
| Low & Wiebe 2018, Quantum | Interaction picture simulation technique — this paper applies their framework | No (Low & Wiebe) |
| Berry, Childs, Cleve, Kothari, Somma 2015 | [[Truncated Taylor Series Simulation]] — the LCU method underpinning the interaction picture approach | [[Truncated Taylor Series Simulation]] |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018c (CI)]] | First-quantized CI matrix with $\widetilde{O}(\eta^2 N^3)$ — the prior best first-quantized approach | [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018a (plane-wave dual)]] | Dual-basis second-quantized simulation; defines the $\lambda$ scaling for second-quantized plane waves | [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush et al. 2018b (T complexity)]] | Qubitization-based simulation with compiled T counts; $O(N^3/\varepsilon)$ | [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes\|Kivlichan et al. 2017]] | First-quantized real-space grid simulation; prior work on first-quantized chemistry costs | [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] |
| Berry et al. 2018 | Efficient antisymmetrization via sorting/comparison circuits — cited for initial-state prep cost $O(\eta\log\eta\log N)$ | No |
| Low & Chuang 2019 | Qubitization framework — cited for the alternative complexity $\widetilde{O}(\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})$ | No |
| Martin 2004 (textbook) | Standard reference for Galerkin plane-wave formulation of DFT | No |
| Harl & Kresse 2008; Shepherd et al. 2012 | Plane-wave basis-set convergence: $O(1/N)$ | No |
| Helgaker et al. 1998; Halkier et al. 1998 | Gaussian basis-set convergence: $O(1/N)$ (same rate as plane waves, but smaller prefactor) | No |
| Toloui & Love 2013; Babbush et al. 2018c | CI matrix / first-quantized approaches using fixed particle number | [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] |
| Bravyi, Gosset, König 2017; Jiang et al. 2018 | Fermion-to-qubit mappings — noted as alternatives to the first-quantized approach taken here | [[Bravyi-Kitaev Transformation]] |

---

## Cross-links

### Paper notes
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — introduces the second-quantized plane-wave dual basis that motivated the plane-wave setting here; this paper's choice to work in first quantization in momentum space is "conceptually dual" to Low & Wiebe's choice to use the dual basis in second quantization
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — the other first-quantized approach by the same group; CI matrix with $\widetilde{O}(\eta^2 N^3)$ complexity; this paper supersedes CI for large $N$
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — provides compiled T-gate counts for the qubitization approach; this paper's qubitization alternative sketch ($\widetilde{O}(\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})$) could be compiled similarly
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — real-space first-quantized simulation; different basis (position grid vs momentum grid), different convergence properties; this paper notes that real-space grids lack variational error bounds (Galerkin property)
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]] — second-quantized approach optimizing Trotter step depth; entirely different regime (NISQ, shallow circuits, linear connectivity); complementary rather than competing
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al 2020]]
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes|McClean, Babbush, Lin et al 2019]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al 2021]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al 2020]]
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman, Mejuto-Zaera, Babbush et al 2018]]
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven 2019]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush 2019]]
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al 2022]]
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al 2023]]

### Trick cards
- [[First-Quantized Plane-Wave Chemistry Encoding]] — new trick from this paper; $\eta$ registers of $\log N$ qubits; antisymmetry in wavefunction
- [[Interaction Picture Simulation (Kinetic Frame)]] — new trick from this paper; $A=T$, $B=U+V$, $\lambda = O(\eta^{5/3} N^{1/3})$
- [[LCU Sign-Parity Cancellation for Grid Boundaries]] — new trick from this paper; $x \in \{0,1\}$ parity trick to handle out-of-bounds momentum shifts
- [[Truncated Taylor Series Simulation]] — the LCU framework the interaction picture method builds on
- [[Qubitization (Quantum Walk for Spectral Encoding)]] — the alternative approach briefly analyzed; gives $\widetilde{O}(\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})$
- [[Plane-Wave Dual Basis]] — second-quantized counterpart to this paper's first-quantized momentum-space representation
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — the follow-up that compiles these algorithms to explicit circuits with full constant-factor analysis; introduces kinetic energy as bit-product LCU, failed PREP fallback, incremental kinetic energy register, and qubitized Dyson series
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes]] — compares these first-quantized algorithms against classical *mean-field* dynamics (RT-TDHF/RT-TDDFT) rather than exact classical methods; shows polynomial quantum speedup in basis size $N$ even over approximate classical approaches; tightens Trotter bounds and introduces first-quantized classical shadows for $k$-RDM estimation
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — Extends this framework to realistic materials with GTH pseudopotentials and non-cubic cells; demonstrates that the $\tilde{O}(\eta)$ block encoding scaling survives the much more complicated pseudopotential form; first concrete resource estimates for first-quantized simulation of transition metal catalysis
- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]] — Product-formula alternative to the interaction-picture approach here; achieves better complexity for $N < \eta^6$ by using quantum FMM to reduce the Coulomb cost to $\widetilde{O}(\eta)$
