_Comparison tables for near-term quantum chemistry experiments and variational algorithms._

_Split from [[Hamiltonian Simulation — Comparison Tables]]._

---

### Method comparison: H₂ ground state experiments

| Paper | Year | System | Qubits | Algorithm | Fermion mapping | Chemical accuracy? | Scalable preprocessing? |
|---|---|---|---|---|---|---|---|
| [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|Aspuru-Guzik et al.]] | 2005 | Classical simulation | — | QPE (proposed) | JW | Yes (in principle) | Yes |
| [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes|Lanyon et al.]] | 2010 | Photonic | 2 | PEA | Config. basis | Yes | No |
| [[NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) — Paper Notes\|Du et al.]] | 2010 | NMR | 2 | PEA + adiabatic | Config. basis | Yes | No |
| [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes\|Wang et al.]] | 2015 | NV center | 1–2 | PEA | Config. basis | Yes (HeH⁺) | No |
| [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo et al.]] | 2014 | Photonic | 2 | VQE (first demo) | Config. basis | Yes (HeH⁺) | No |
| [[Quantum Implementation of Unitary Coupled Cluster for Simulating Molecular Electronic Structure (Shen, Zhang, Chen, Zhang, Yung, Kim 2015) — Paper Notes\|Shen et al.]] | 2015 | Ion trap | 2 | VQE + UCC | Config. basis | Yes (HeH⁺) | No |
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes\|O'Malley et al.]] | 2016 | Superconducting | 2 (VQE) / 3 (PEA) | VQE + PEA | Bravyi-Kitaev | VQE: Yes / PEA: No | **Yes** |


### NISQ hardware simulation experiments (superconducting, correlated systems)

| Paper | Year | System | Qubits | Two-qubit gates | Algorithm | Error mitigation | Quantitatively meaningful? |
|---|---|---|---|---|---|---|---|
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes\|O'Malley et al.]] | 2016 | H₂ | 2–3 | 2–14 | [[Variational Quantum Eigensolver (VQE)\|VQE]] / PEA | None | VQE: Yes (chem. acc.) / PEA: No |
| [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes\|Kandala et al.]] | 2017 | H₂, LiH, BeH₂ | 6 | ~50 | VQE | Readout | Partially (LiH, BeH₂ qualitative only) |
| [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes\|Huggins et al.]] | 2022 | Diamond, C chain | 8–16 | ~50–200 | QC-AFQMC | Shadow tomography, noise-resilient ratios | Yes (competitive with CCSD(T)) |
| [[Observation of Separated Dynamics of Charge and Spin in the Fermi-Hubbard Model (Arute et al. 2020) — Paper Notes\|Arute et al.]] | 2020 | Free fermion dynamics | ~12 | ~500 | Direct Trotter | Floquet (no microwave gates) | Yes (free fermion model) |
| [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes\|Tazhigulov et al.]] | 2022 | Fe-S clusters, α-RuCl₃ | 5–11 | 22–310 | QITE + recompilation | Floquet, postselection, DD, rescaling | ≤100 gates: Yes / 310 gates: No |


### Fermion-to-qubit mapping comparison

| Mapping | Qubit count | Operator locality | Two-body term weight | Particle number preserved? |
|---|---|---|---|---|
| Jordan-Wigner | $N$ | $O(N)$ | $O(N)$ Pauli factors | Yes (easily) |
| Bravyi-Kitaev | $N$ | $O(\log N)$ | $O(\log N)$ Pauli factors | Less directly |
| Parity basis | $N$ | $O(\log N)$ | $O(\log N)$ Pauli factors | Indirectly |


### VQE vs. PEA resource comparison (for H₂, O'Malley et al.)

| Resource | VQE | PEA |
|---|---|---|
| Qubits | 2 | 3 |
| Single-qubit gates | 11 | 51+ |
| Two-qubit gates | 2 CZ$_\pi$ | 4 CZ$_{\phi\neq\pi}$ + 10 CZ$_\pi$ |
| Trotter steps required | N/A | $\rho \sim 10$–$100$ for chem. acc. |
| Error correction needed | No (H₂) | Yes (for chemical accuracy) |
| Error robustness | High (classical loop) | Low (fixed circuit) |
| Achieved accuracy (Hartree) | $(8\pm5)\times10^{-4}$ | $(1\pm1)\times10^{-2}$ |


### VQE-UCC resource strategies comparison (Romero, Babbush et al. 2018)

| Strategy | Qubit reduction | Parameter reduction | Sampling reduction | Ref |
|---|---|---|---|---|
| Trotterized product form ($\rho=1$) | None | None | None | [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes\|Romero et al. 2018]] |
| MP2 prescreening ($d=10^{-3}$) | None | $\sim 2\times$ | $\sim 3\times$ (optimizer evals) | [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes\|Romero et al. 2018]] |
| Active space (CAS-UCC) | $N \to N_A$ | $O(\eta^2N^2) \to O(\eta_A^2N_A^2)$ | Proportional | [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes\|Romero et al. 2018]] |
| Analytical gradient | None | None | $(2\delta)^2\times$ per gradient call | [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes\|Romero et al. 2018]] |
| BK vs JW mapping | None | None | $O(N) \to O(\log N)$ gates/param | [[Bravyi-Kitaev Transformation]] |


### VQE measurement strategies comparison

| Method | # Partitions | Gate count (per circuit) | Depth | Connectivity | Diagonal? | Empirical shot scaling | Reference |
|---|---|---|---|---|---|---|---|
| Separate Pauli terms | $O(N^4)$ | $N$ | 1 | any | no | $\sim N^{5.7}$ | [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes\|Wecker et al. (2015)]] |
| Compatible Pauli heuristic | $O(N^4)$ | $N$ | 1 | any | no | $\sim N^{4.9}$ | Verteletskyi et al. (2020) |
| [[Variance Reduction via N-Representability Constraints (LP Method)\|RDM constraints]] | $O(N^4)$ | $N$ | 1 | any | no | $\sim N^{5.6}$ | [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes\|Rubin et al. (2018)]] |
| Commuting Pauli clique cover | $O(N^3)$ | $O(N^2)$ | — | full | no | — | Jena et al. (2019) |
| [[Fermionic Swap Network\|Generalized swap networks]] | $O(N^3)$ | $O(N^2)$ | $O(N)$ | linear | no | — | O'Gorman et al. (2019) |
| [[Variational Measurement Reduction via Dual Basis\|Dual basis (plane-wave)]] | 2 | $O(N \log N)$ | $O(N)$ | planar | yes | — | [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. (2018)]] |
| [[Basis Rotation Grouping for VQE Measurement\|Basis Rotation Grouping]] | $O(N)$ | $N^2/4$ | $N/2$ | **linear** | **yes** | $\sim N^{2.75}$ | [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes\|Huggins et al. (2021)]] |


---

**See also:** [[Quantum Chemistry — Fault-Tolerant Resource Estimates]] · [[Quantum Chemistry — Circuit Primitives]] · [[Quantum Chemistry — Technique Crosswalk]] · [[Hamiltonian Simulation — Time-Dependent Methods]]
