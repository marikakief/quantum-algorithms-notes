# Hamiltonian Simulation — Comparison Tables

## Quantum Chemistry Simulation Methods

### Method comparison: H₂ ground state experiments

| Paper | Year | System | Qubits | Algorithm | Fermion mapping | Chemical accuracy? | Scalable preprocessing? |
|---|---|---|---|---|---|---|---|
| Aspuru-Guzik et al. | 2005 | Classical simulation | — | QPE (proposed) | JW | Yes (in principle) | Yes |
| Lanyon et al. | 2010 | Photonic | 2 | PEA | Config. basis | Yes | No |
| Du et al. | 2010 | NMR | 2 | PEA + adiabatic | Config. basis | Yes | No |
| Wang et al. | 2015 | NV center | 1–2 | PEA | Config. basis | Yes (HeH⁺) | No |
| Peruzzo et al. | 2014 | Photonic | 2 | VQE (first demo) | Config. basis | Yes (HeH⁺) | No |
| Shen et al. | 2015 | Ion trap | 2 | VQE + UCC | Config. basis | Yes (H₂) | No |
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes\|O'Malley et al.]] | 2016 | Superconducting | 2 (VQE) / 3 (PEA) | VQE + PEA | Bravyi-Kitaev | VQE: Yes / PEA: No | **Yes** |

### NISQ hardware simulation experiments (superconducting, correlated systems)

| Paper | Year | System | Qubits | Two-qubit gates | Algorithm | Error mitigation | Quantitatively meaningful? |
|---|---|---|---|---|---|---|---|
| [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes\|O'Malley et al.]] | 2016 | H₂ | 2–3 | 2–14 | [[Variational Quantum Eigensolver (VQE)\|VQE]] / PEA | None | VQE: Yes (chem. acc.) / PEA: No |
| Kandala et al. | 2017 | H₂, LiH, BeH₂ | 6 | ~50 | VQE | Readout | Partially (LiH, BeH₂ qualitative only) |
| [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes\|Huggins et al.]] | 2022 | Diamond, C chain | 8–16 | ~50–200 | QC-AFQMC | Shadow tomography, noise-resilient ratios | Yes (competitive with CCSD(T)) |
| Arute et al. | 2020 | Free fermion dynamics | ~12 | ~500 | Direct Trotter | Floquet (no microwave gates) | Yes (free fermion model) |
| [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes\|Tazhigulov et al.]] | 2022 | Fe-S clusters, α-RuCl₃ | 5–11 | 22–310 | QITE + recompilation | Floquet, postselection, DD, rescaling | ≤100 gates: Yes / 310 gates: No |

### Quantum chemistry simulation algorithms: asymptotic gate complexity

| Paper | Year | Representation | Qubits | Gate complexity | Precision scaling |
|---|---|---|---|---|---|
| Aspuru-Guzik et al. | 2005 | Second-quantized (JW) | $O(N)$ | $\tilde{O}(N^{11} t / \varepsilon)$ | $O(1/\varepsilon)$ |
| Wecker, Hastings, Troyer | 2015 | Second-quantized (BK), Trotter | $O(N)$ | $O(N^{8+o(1)} t / \varepsilon^{o(1)})$ | $O(1/\varepsilon)$ |
| Babbush et al. 2016 (NJP) — database | 2016 | Second-quantized, Taylor series | $O(N)$ | $\tilde{O}(N^4 \|H\| t)$ | $O(\log 1/\varepsilon)$ |
| Babbush et al. 2016 (NJP) — on-the-fly | 2016 | Second-quantized, Taylor series | $O(N)$ | $\tilde{O}(N^5 t)$ | $O(\log 1/\varepsilon)$ |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018]] | 2018 | First-quantized (CI matrix), Taylor series | $\tilde{O}(\eta)$ | $\tilde{O}(\eta^2 N^3 t)$ | $O(\log 1/\varepsilon)$ |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes\|Kivlichan et al. 2017]] (fixed $h$) | 2017 | First-quantized (real-space grid), Taylor series | $\eta D \lceil\log b\rceil$ | $\tilde{O}(\eta^2 t)$ (pairwise oracle) | $O(\log 1/\varepsilon)$ |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes\|Kivlichan et al. 2017]] (Corollary 5) | 2017 | First-quantized (real-space grid), Taylor series | $\eta D \lceil\log b\rceil$ | $\tilde{O}(\eta^7 t^3/\varepsilon^2)$ | $O(1/\varepsilon^2)$ |
| [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes\|Kivlichan, McClean et al. 2018]] | 2018 | Second-quantized (JW), Trotter | $N$ | $N(N{-}1)/2$ entangling gates per step; total $O(N^2 t/\varepsilon)$ | $O(1/\varepsilon)$ |
| [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes\|Motta, Babbush, Chan et al. 2018 (double factorization)]] | 2018 | Second-quantized (JW), double factorization + Trotter | $N$ | $O(N^2 \log N)$ per step (asymptotic) / $O(N^3)$ (fixed molecule) | $O(1/\varepsilon)$ |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018 (dual basis, Trotter)]] | 2018 | Second-quantized (JW), plane-wave dual, Trotter | $N$ | depth $O(N^{7/2} t^{3/2}/\sqrt{\varepsilon})$ (fixed density) | $O(1/\sqrt{\varepsilon})$ |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018 (dual basis, LCU on-the-fly)]] | 2018 | Second-quantized (JW), plane-wave dual, Taylor series | $N$ | gates $\widetilde{O}(N^{11/3})$; depth $\widetilde{O}(N^{8/3})$ (fixed density) | $O(\log 1/\varepsilon)$ |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018 (qubitization)]] | 2018 | Second-quantized, plane-wave dual, qubitization | $O(\log(\lambda N/\varepsilon))$ | T gates $O(N^3/\varepsilon + N^2 \log(1/\varepsilon)/\varepsilon)$; per-oracle T = $O(N)$ | $O(1/\varepsilon)$ (Heisenberg-limited) |
| [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes\|Babbush, Berry et al. 2019 (interaction picture)]] | 2019 | First-quantized, plane-wave, interaction picture | $O(\eta \log N)$ | $\widetilde{O}(N^{1/3} \eta^{8/3} t)$ | $O(\log 1/\varepsilon)$ |
| [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes\|Babbush, Huggins, Berry et al. 2023 (1st-quant. Trotter)]] | 2023 | First-quantized, real-space grid, high-order Trotter | $O(\eta \log N)$ | $(N^{1/3}\eta^{7/3}t + N^{2/3}\eta^{4/3}t)(Nt/\varepsilon)^{o(1)}$ | $O(1/\varepsilon^{o(1)})$ |
| [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes\|Su, Berry et al. 2021 (qubitization)]] | 2021 | First-quantized, plane-wave, qubitization | $O(\eta \log N)$ | $\widetilde{O}((\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})/\varepsilon)$ | $O(1/\varepsilon)$ (Heisenberg) |
| [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes\|Su, Berry et al. 2021 (interaction picture)]] | 2021 | First-quantized, plane-wave, interaction picture | $O(\eta \log N)$ | $\widetilde{O}(\eta^{8/3}N^{1/3}/\varepsilon)$ | $O(1/\varepsilon)$ (qubitized Dyson) |
| [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|McClean, Babbush et al. 2019 (DG + LCU)]] | 2019 | Second-quantized, DG block-diagonal basis, LCU | $N_d$ | $O(n_\kappa^2 N_b \lambda t)$; empirically $O(N_h^{2.6} t)$ (H-chain) | $O(\log 1/\varepsilon)$ |
| [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|McClean, Babbush et al. 2019 (DG + Trotter)]] | 2019 | Second-quantized, DG block-diagonal basis, Trotter | $N_d$ | Trotter-step depth $O(N_b n_\kappa^3)$ | $O(1/\varepsilon)$ |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry, Gidney et al. 2019 (single factorization)]] | 2019 | Second-quantized, arbitrary basis, single factorization + qubitization | $O(N)$ | $\widetilde{O}(N^{3/2}\lambda/\varepsilon)$ | $O(1/\varepsilon)$ (Heisenberg) |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry, Gidney et al. 2019 (sparse Coulomb)]] | 2019 | Second-quantized, arbitrary basis, sparse Coulomb + qubitization | $O(N)$ | System-dependent; $\widetilde{O}(\lambda L_V^{(c)}/\varepsilon)$ | $O(1/\varepsilon)$ (Heisenberg) |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee, Berry et al. 2021 (THC qubitization)]] | 2021 | Second-quantized, arbitrary basis, THC + qubitization | $\widetilde{O}(N)$ | $\widetilde{O}(N\lambda_\zeta/\varepsilon)$; empirically $\widetilde{O}(N^{2.1}/\varepsilon)$ (H-chain) to $\widetilde{O}(N^{3.1}/\varepsilon)$ (H₄) | $O(1/\varepsilon)$ (Heisenberg) |
| [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (Bloch DF)]] | 2023 | Second-quantized, Bloch orbitals, DF + qubitization | $\widetilde{O}(\sqrt{N_k}N)$ | $\widetilde{O}(\sqrt{N_k} N \sqrt{\Xi}\, \lambda_{\rm DF}/\varepsilon)$ | $O(1/\varepsilon)$ (Heisenberg) |
| [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (Bloch THC)]] | 2023 | Second-quantized, Bloch orbitals, k-THC + qubitization | $\widetilde{O}(N_k N)$ | $\widetilde{O}(N_k N\, \lambda_{\rm THC}/\varepsilon)$ | $O(1/\varepsilon)$ (Heisenberg) |
| [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes\|Berry, Rubin et al. 2024 (1st-quant. GTH pseudopotential)]] | 2024 | First-quantized, plane-wave, GTH pseudopotential + qubitization | $O(\eta \log N)$ | Block encoding $\widetilde{O}(\eta)$; QPE $O(\eta \lambda/\varepsilon)$ with $\lambda$ dominated by $\lambda_{\rm nonloc}$ | $O(1/\varepsilon)$ (Heisenberg) |
| [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (stopping power, QSP)]] | 2023 | First-quantized, plane-wave, non-BO, QSP | $O(\eta \log N)$ | $\widetilde{O}((\eta^2/\Delta^2 + \eta^3/\Delta) \cdot t \cdot N_s)$ | $O(\log 1/\varepsilon)$ |
| [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (stopping power, 8th-order Trotter)]] | 2023 | First-quantized, real-space grid, non-BO, product formula | $O(\eta \log N)$ | $\sim O(\eta^2)$ at fixed $r_s$; bespoke 8th-order formula with $\xi = 3.4 \times 10^{-8}$ | $O(1/\varepsilon^{1/k})$ |
| [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes\|Berry, Wan et al. 2025 (quantum FMM)]] | 2025 | First-quantized, real-space grid, quantum FMM + high-order product formula | $O(\eta \log N \log^3(1/\varepsilon))$ | $(\eta^{4/3}N^{1/3} + \eta^{1/3}N^{2/3})(\eta N/\varepsilon)^{o(1)}$ | $O(1/\varepsilon^{o(1)})$ |

### Qubitization-based chemistry: FeMoCo resource estimates

| Algorithm | Logical qubits (Reiher) | Toffoli count (Reiher) | Logical qubits (Li) | Toffoli count (Li) |
|---|---|---|---|---|
| [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes\|Reiher et al. 2017 (Trotter)]] | 111 | $5.0 \times 10^{13}$ | — | — |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry et al. 2019 (sparse)]] | 2,190 | $8.8 \times 10^{10}$ | 2,489 | $4.4 \times 10^{10}$ |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry et al. 2019 (single factorization)]] | 3,320 | $9.5 \times 10^{10}$ | 3,628 | $1.2 \times 10^{11}$ |
| von Burg et al. 2020 (double factorization) | 3,725 | $1.0 \times 10^{10}$ | 6,404 | $6.4 \times 10^{10}$ |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee, Berry et al. 2021 (THC)]] | **2,142** | $\mathbf{5.3 \times 10^9}$ | **2,196** | $\mathbf{3.2 \times 10^{10}}$ |
| [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes\|Berry, Tong et al. 2024 (THC + MPS + QPE)]] | — | — | 2,194 | $7.3 \times 10^{10}$ (95% CI) |
| [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes\|Berry, Tong et al. 2024 (DF + MPS + QPE)]] | — | — | 6,402 | $1.1 \times 10^{11}$ (95% CI) |
| Oumarou et al. 2024 (symmetry compression DF) | 1,994 | $2.6 \times 10^9$ | — | — |
| Caesura et al. 2025 (symmetry compression THC+BLISS) | — | — | 1,512 | $4.3 \times 10^9$ |
| [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes\|Low, King, Berry et al. 2025 (DFTHC+BLISS+SA)]] | **1,137** | $\mathbf{3.41 \times 10^8}$ | **1,459** | $\mathbf{9.99 \times 10^8}$ |

### Qubitization-based chemistry: DFTHC+SA resource estimates (broader benchmarks)

| System | Orbitals | Electrons | Logical qubits | Toffoli count | $\lambda_{\text{eff}}$ [Ha] | Improvement over DF | Improvement over THC |
|---|---|---|---|---|---|---|---|
| Fe₂S₂ | 20 | 30 | 466 | $3.97 \times 10^7$ | 6.47 | 11.7× | 11.7× |
| Fe₄S₄ | 36 | 54 | 873 | $1.72 \times 10^8$ | 14.98 | 23.3× | 13.2× |
| FeMoCo | 54 | 54 | 1,137 | $3.41 \times 10^8$ | 21.37 | 7.0× | 15.5× |
| FeMoCo | 76 | 113 | 1,459 | $9.99 \times 10^8$ | 43.65 | 37.1× | 4.3× |
| CPD1-P450X | 58 | 63 | 1,150 | $4.91 \times 10^8$ | 32.79 | 7.7× | 3.5× |
| CO₂ [XVIII] | 56 | 64 | 924 | $2.05 \times 10^8$ | 17.07 | 121.9× | — |
| CO₂ [XVIII] | 100 | 100 | 1,960 | $1.06 \times 10^9$ | 37.68 | 112.7× | — |
| CO₂ [XVIII] | 150 | 150 | 2,870 | $2.81 \times 10^9$ | 65.87 | 195.2× | — |

Source: [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low, King, Berry et al. (2025)]]. $\sigma_{\text{PEA}} = 1.0$ mHa. $\lambda_{\text{eff}} = \sqrt{2\Lambda E_{\text{gap}}}$.

### Qubitization-based chemistry: CYP P450 Cpd I resource estimates (THC)

| Active space | Orbitals | Electrons | Logical qubits | Toffoli count | Physical qubits (0.1%) | Runtime (0.1%) |
|---|---|---|---|---|---|---|
| G | 43 | 47 | ~1,100 | $\sim 3 \times 10^9$ | — | ~30 hrs |
| X | 58 | 63 | 1,429 | $7.0 \times 10^9$ | 4,624,440 | 73 hrs |
| Full space (extrap.) | ~500 | — | ~9,000 | $1.5 \times 10^{12}$ | — | Infeasible |

Source: [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al. (2022)]]. Physical qubit counts and runtimes assume 4 AutoCCZ factories, 1 µs surface code cycle, 10 µs reaction time.

### Qubitization-based materials: Diamond resource estimates (cc-pVDZ, Bloch orbitals)

| LCU | k-mesh | Spin-orbitals | Toffolis | Logical qubits | Physical qubits (M) | Runtime (days) |
|---|---|---|---|---|---|---|
| DF | [1,1,1] | 52 | $9.6 \times 10^8$ | 2,396 | 1.55 | 0.18 |
| DF | [2,2,2] | 416 | $6.7 \times 10^{10}$ | 18,693 | 18.5 | 12.7 |
| DF | [3,3,3] | 1,404 | $1.1 \times 10^{12}$ | 68,470 | 82.4 | 237 |
| THC | [1,1,1] | 52 | $1.7 \times 10^{10}$ | 18,095 | 14.2 | 3.1 |
| THC | [2,2,2] | 416 | $4.9 \times 10^{11}$ | 36,393 | 35.6 | 105 |
| Sparse | [2,2,2] | 416 | $2.7 \times 10^{12}$ | 75,287 | 90.6 | 577 |
| SF | [2,2,2] | 416 | $3.3 \times 10^{12}$ | 20,567 | 24.9 | 711 |

Source: [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]]. Symmetry-adapted block encodings. Physical qubits and runtimes assume 4 Toffoli factories, 0.01% physical error rate, 1 µs cycle time.

### Qubitization-based materials: LiNiO₂ (LNO) resource estimates (DF, Bloch orbitals)

| Structure | k-mesh | Spin-orbitals | $\lambda$ | Toffolis | Logical qubits | Runtime (days) |
|---|---|---|---|---|---|---|
| R$\bar{3}$m | [2,2,2] | 116 | 10,730 | $5.0 \times 10^{12}$ | 149,939 | 1,080 |
| R$\bar{3}$m | [3,3,3] | 116 | 44,795 | $7.3 \times 10^{13}$ | 598,286 | 17,900 |
| C2/m | [2,2,1] | 116 | 4,874 | $1.2 \times 10^{12}$ | 75,178 | 256 |
| C2/m | [4,4,2] | 116 | 51,416 | $9.8 \times 10^{13}$ | 598,736 | 24,100 |
| P21/c | [1,2,1] | 232 | 3,958 | $1.3 \times 10^{12}$ | 75,383 | 276 |
| P2/c | [1,1,1] | 464 | 2,754 | $9.7 \times 10^{11}$ | 75,834 | 211 |

Source: [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]]. DF with symmetry-adapted block encodings. Same hardware assumptions. LNO is a realistic battery cathode problem — even the smallest k-meshes require $O(10^{12})$ Toffolis.

### First-quantized vs second-quantized: LNO comparison (large-core GTH-LDA)

| Method | Structure | Grid / basis | $\lambda$ | Toffolis | Logical qubits |
|---|---|---|---|---|---|
| 1st quant. PW (Berry et al. 2024) | C2/m | [5,5,5] | 3,310,000 | $6.0 \times 10^{13}$ | ~1,400 |
| 1st quant. PW (Berry et al. 2024) | C2/m | [6,6,6] | 3,490,000 | $7.4 \times 10^{13}$ | ~1,600 |
| 2nd quant. DF (Rubin et al. 2023) | C2/m | DZVP | 4,874 | $1.2 \times 10^{12}$ | ~75,000 |
| 1st quant. PW (Berry et al. 2024) | P21/c | [5,5,5] | 3,340,000 | $6.0 \times 10^{13}$ | ~1,400 |
| 2nd quant. DF (Rubin et al. 2023) | P21/c | DZVP | 3,958 | $1.3 \times 10^{12}$ | ~75,000 |
| 1st quant. PW (Berry et al. 2024) | P2/c | [5,5,5] | 3,240,000 | $5.9 \times 10^{13}$ | ~1,400 |
| 2nd quant. DF (Rubin et al. 2023) | P2/c | DZVP | 2,754 | $9.7 \times 10^{11}$ | ~75,000 |

Source: [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry, Rubin, Babbush et al. (2024)]] and [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]]. First quantization uses large-core GTH-LDA (92 electrons, 4 formula units). Second quantization uses GTH-HF-REV with DZVP basis. $\lambda_{\text{nonloc}}$ dominates in first quantization. First quantization costs ~10–50× more Toffolis but ~50× fewer logical qubits — the spacetime volume comparison favours first quantization.

### Stopping power / quantum dynamics resource estimates (first-quantized, non-BO)

| System | $\eta$ | QSP Toffolis | Trotter Toffolis | QSP Qubits | Trotter Qubits |
|---|---|---|---|---|---|
| $\alpha$ + H (50% cell) | 28 | $5.6 \times 10^{14}$ | $1.1 \times 10^{13}$ | 1,749 | 2,666 |
| $\alpha$ + H (75% cell) | 92 | $2.0 \times 10^{16}$ | $3.1 \times 10^{14}$ | 3,309 | 3,902 |
| $\alpha$ + H (full) | 218 | $2.0 \times 10^{17}$ | $1.4 \times 10^{15}$ | 5,650 | 6,170 |
| p + D | 1,729 | $2.1 \times 10^{20}$ | $2.1 \times 10^{17}$ | 33,038 | 33,368 |
| p + C | 391 | $2.2 \times 10^{18}$ | $1.1 \times 10^{16}$ | 8,841 | 9,284 |

Source: [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]]. Parameters: 10 time points ($t = 1$ to $t = 10$ a.u.), infidelity $\varepsilon = 0.01$, $N_s = 50$ samples, $\sigma_k = 6$ a.u. Trotter uses bespoke 8th-order formula with prefactor $\xi = 3.4 \times 10^{-8}$. Product formula outperforms QSP by $\sim 100\times$ for all systems.

### SYK model simulation algorithms

| Paper | Year | Method | Gate complexity | Precision scaling |
|---|---|---|---|---|
| García-Álvarez et al. | 2017 | Lie-Trotter | $O(N^{10} t^2/\varepsilon)$ | $O(1/\varepsilon)$ |
| [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes\|Babbush, Berry, Neven 2019]] | 2019 | Asymmetric qubitization + QSP | $O(N^{7/2}t + N^{5/2}t\,\mathrm{polylog}(N/\varepsilon))$ | $O(\mathrm{polylog}(1/\varepsilon))$ |

### Fault-tolerant force estimation algorithms: asymptotic Toffoli complexity

Source: [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al. (2022)]]

All scalings target 2-norm error $\varepsilon$ in the $3N_a$-dimensional gradient vector. $N$ = number of orbitals/plane waves, $\eta$ = electrons, $\gamma$ = spectral gap. Assumes $\gamma \in \Theta(1)$ and state preparation is not the dominant cost.

| Algorithm | First-quant. plane waves | 2nd-quant. plane waves | H-chains (sparse) | H-chains (DF) | Water (sparse) | Water (DF) |
|---|---|---|---|---|---|---|
| Finite difference | $\tilde{O}(N^{2/3}N_a^{17/6}/\varepsilon)$ | $\tilde{O}(N^3 N_a^{3/2}/\varepsilon)$ | $\tilde{O}(N_a^{3.83}/\varepsilon)$ | $\tilde{O}(N_a^{4.44}/\varepsilon)$ | $\tilde{O}(N_a^{3.87}/\varepsilon)$ | $\tilde{O}(N_a^{4.20}/\varepsilon)$ |
| Overlap estimation (OEA) | $\tilde{O}(N^{4/3}N_a^{23/6}/\varepsilon)$ | $\tilde{O}(N^3 N_a^{5/2}/\varepsilon)$ | $\tilde{O}(N_a^{4.11}/\varepsilon)$ | $\tilde{O}(N_a^{4.50}/\varepsilon)$ | $\tilde{O}(N_a^{4.30}/\varepsilon)$ | $\tilde{O}(N_a^{4.74}/\varepsilon)$ |
| Gradient-based (Huggins et al.) | $\tilde{O}(N^{4/3}N_a^{10/3}/\varepsilon)$ | $\tilde{O}(N^3 N_a^{2}/\varepsilon)$ | $\tilde{O}(N_a^{3.61}/\varepsilon)$ | $\tilde{O}(N_a^{4.00}/\varepsilon)$ | $\tilde{O}(N_a^{3.80}/\varepsilon)$ | $\tilde{O}(N_a^{4.24}/\varepsilon)$ |

### Block-encoding rescaling factors: Hamiltonian vs. force operator

| System | Method | $\lambda_H$ scaling | $\lambda_F$ scaling |
|---|---|---|---|
| H-chains (STO-6G) | Sparse | $O(N_H^{1.33})$ | $O(N_H^{0.28})$ |
| H-chains (STO-6G) | Double factorisation | $O(N_H^{1.94})$ | $O(N_H^{0.06})$ |
| Water clusters (STO-3G) | Sparse | $O(N_{\text{H}_2\text{O}}^{1.37})$ | $O(N_{\text{H}_2\text{O}}^{0.43})$ |
| Water clusters (STO-3G) | Double factorisation | $O(N_{\text{H}_2\text{O}}^{1.70})$ | $O(N_{\text{H}_2\text{O}}^{0.54})$ |
| First-quant. plane waves | Qubitisation | $O(\eta N^{2/3}/\Omega^{2/3})$ | $O(\eta N^{2/3}/\Omega^{2/3})$ |

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

### Best product formulas by order (eigenvalue error)

Best-performing product formulas for each order, as benchmarked in [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales et al. (2025)]]. Performance metric is $M\zeta^{1/k}$ where $M$ = stages, $\zeta$ = eigenvalue error constant, $k$ = order. Lower is better.

| Order | Formula | Stages ($M$) | Processed? | $\zeta$ (eigenvalue) | $M\zeta^{1/k}$ | Source |
|---|---|---|---|---|---|---|
| 4th | BM4M6 | 6 | No | $2.4 \times 10^{-5}$ | 0.42 | Blanes-Moan (2002) |
| 4th | PPBCM4m6 | 6 | Yes | $2.4 \times 10^{-5}$ | 0.42 | Blanes-Casas-Murua (2006) |
| 6th | PPBCM6m9 | 9 | Yes | $4.3 \times 10^{-7}$ | 0.78 | Blanes-Casas-Murua (2006) |
| 8th | **YP8m8** | **17** | **Yes** | $\mathbf{8.1 \times 10^{-10}}$ | **1.24** | **Morales et al. (2025)** |
| 8th | **Y8m10b** | **21** | **No** | $\mathbf{5.4 \times 10^{-10}}$ | **1.46** | **Morales et al. (2025)** |
| 8th | SS8s19 (prior best) | 19 | No | $5.3 \times 10^{-8}$ | 2.34 | Sofroniou-Spaletta (2005) |
| 10th | SS10s35 | 35 | No | $3.1 \times 10^{-11}$ | 3.11 | Sofroniou-Spaletta (2005) |

Cross-order thresholds (random matrices, [[Eigenvalue Error as the Correct Product Formula Metric|eigenvalue error]]):

| Transition | Asymptotic $T/\varepsilon$ | Non-asymptotic $T/\varepsilon$ | Practical range |
|---|---|---|---|
| 4th → 6th | ~1,700 | — | NISQ |
| 6th → 8th | ~68,000 | ~$3.2 \times 10^7$ | Early fault-tolerant |
| 8th → 10th | ~$9.3 \times 10^{15}$ | Similar | Never (for quantum computing) |

### Trotter step circuit depth comparison (linear connectivity)

| Method | Trotter-step depth | Two-qubit gates | Connectivity | Reference |
|---|---|---|---|---|
| Naive JW Pauli decomposition | $O(N^4)$ | $O(N^5)$ | Linear | Aspuru-Guzik et al. (2005) |
| Prior swap network | $2N$ | $O(N^2)$ | Planar | [Prior work, ref 40] |
| [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes\|Fermionic swap network]] | $N$ | $N(N{-}1)/2$ | **Linear** | Kivlichan et al. (2018) |
| [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes\|Double factorization]] | $O(N^2)$ | $O(N^2 \log N)$ asymptotic | **Linear** | Motta et al. (2018) |
| [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|DG block-diagonal swap network]] | $O(N_b n_\kappa^3)$ | $O(N_b^2 n_\kappa^4)$ | **Linear** | McClean et al. (2019) |
| FFFT (plane-wave basis) | $O(N \log N)$ | $O(N \log N)$ | Planar | — |

### Slater determinant state preparation comparison

| Method | Depth | Givens rotations | Connectivity | Reference |
|---|---|---|---|---|
| Wecker et al. | $O(N^2)$ | $N^2$ | Arbitrary | Wecker et al. (2015) |
| FFFT-based (plane waves) | $O(N)$ | $O(N \log N)$ | Planar | — |
| [[Givens Rotation Slater Determinant Preparation\|Parallel Givens (this work)]] | $\leq N/2$ | $\eta(N{-}\eta)$ (with symmetry) | **Linear** | Kivlichan et al. (2018) |

### Multi-determinant state preparation comparison

| Method | Gate cost | Ancilla | Notes | Reference |
|---|---|---|---|---|
| Compressed register + [[QROM (Quantum Read-Only Memory)\|QROM]] | $O(L)$ (register prep) + isometry | $\lceil \log L \rceil$ | Isometry via QROM or select-unitary | [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes\|Tubman et al. (2018)]] |
| [[Sequential Multi-Determinant State Preparation\|Sequential auxiliary-qubit]] | $O(nL)$ | $1$ | Reduced by Hamming ordering; no index register | [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes\|Tubman et al. (2018)]] |

### Trotter step count estimates for chemical accuracy

| Molecule | Orbitals | Est. Trotter steps (first-order) | Reference |
|---|---|---|---|
| H₂ (STO-6G) | 4 | $\sim 10$ | O'Malley et al. (2016) |
| LiH | 12 | $\sim 100$ | Wecker et al. (2015) |
| BeH₂ | 14 | $\sim 200$ | Wecker et al. (2015) |

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
| Separate Pauli terms | $O(N^4)$ | $N$ | 1 | any | no | $\sim N^{5.7}$ | Wecker et al. (2015) |
| Compatible Pauli heuristic | $O(N^4)$ | $N$ | 1 | any | no | $\sim N^{4.9}$ | Verteletskyi et al. (2020) |
| [[Variance Reduction via N-Representability Constraints (LP Method)\|RDM constraints]] | $O(N^4)$ | $N$ | 1 | any | no | $\sim N^{5.6}$ | [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes\|Rubin et al. (2018)]] |
| Commuting Pauli clique cover | $O(N^3)$ | $O(N^2)$ | — | full | no | — | Jena et al. (2019) |
| [[Fermionic Swap Network\|Generalized swap networks]] | $O(N^3)$ | $O(N^2)$ | $O(N)$ | linear | no | — | O'Gorman et al. (2019) |
| [[Variational Measurement Reduction via Dual Basis\|Dual basis (plane-wave)]] | 2 | $O(N \log N)$ | $O(N)$ | planar | yes | — | [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. (2018)]] |
| [[Basis Rotation Grouping for VQE Measurement\|Basis Rotation Grouping]] | $O(N)$ | $N^2/4$ | $N/2$ | **linear** | **yes** | $\sim N^{2.75}$ | [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes\|Huggins et al. (2021)]] |

## Technique crosswalk

| Technique | Used in | Complexity advantage | Key reference |
|---|---|---|---|
| [[Bravyi-Kitaev Transformation]] | O'Malley 2016 | $O(\log N)$ vs $O(N)$ Pauli weight | Seeley, Richard, Love (2012) |
| [[Symmetry Reduction in Qubit Hamiltonians]] | O'Malley 2016 | Reduce qubit count | O'Malley 2016 (implicit) |
| [[Variational Quantum Eigensolver (VQE)]] | O'Malley 2016, Peruzzo 2014 | Error-tolerant, shallow circuits | Peruzzo et al. (2014) |
| [[Unitary Coupled Cluster (UCC) Ansatz]] | O'Malley 2016, Shen 2015 | Classically intractable ansatz | Bartlett et al. (1989) |
| [[Pauli Expectation Value Estimation]] | O'Malley 2016 | Poly. measurement overhead | McClean et al. (2016) |
| [[Trotterized Time Evolution for Chemistry]] | O'Malley 2016 + many others | Scalable Hamiltonian simulation | Trotter (1959) |
| [[Iterative Phase Estimation (Kitaev)]] | O'Malley 2016 | 1 ancilla qubit only | Kitaev (1995) |
| [[CI Matrix Simulation (First-Quantized Encoding)]] | Babbush et al. 2018 | $\tilde{O}(\eta)$ qubits vs $O(N)$; $\tilde{O}(\eta^2 N^3 t)$ gate count | Babbush et al. (2018) |
| [[Truncated Taylor Series Simulation]] | Babbush et al. 2016/2018 | $O(\log 1/\varepsilon)$ precision vs $O(1/\varepsilon)$ Trotter | Berry, Childs et al. (2015) |
| [[On-the-fly Molecular Integral Evaluation]] | Babbush et al. 2018 | Avoids $O(N^4)$ classical precomputation; $\tilde{O}(N)$ per query | Babbush et al. (2018) |
| [[1-Sparse Hamiltonian Decomposition via Graph Coloring]] | Babbush et al. 2018 | $O(\eta^2 N^2)$ colors (vs $O(N^4)$ Pauli terms) | Babbush et al. (2018); Toloui & Love (2013) |
| [[Real-Space Grid Encoding for Many-Body Simulation]] | Kivlichan et al. 2017 | $\eta D \lceil\log b\rceil$ qubits; $\tilde{O}(\eta^2)$ pairwise oracle queries for fixed $h$ | Kivlichan et al. (2017); Wiesner (1996) |
| [[High-Order Finite Difference Kinetic Energy]] | Kivlichan et al. 2017 | LCU 1-norm bounded by $2\pi^2/3$ independent of approximation order | Kivlichan et al. (2017) |
| [[Regularized Coulomb Potential (Delta Cutoff)]] | Kivlichan et al. 2017 | Bounds potential norm at $O(\eta^2/\Delta)$; prerequisite for real-space LCU | Kivlichan et al. (2017) |
| [[Controlled Swap Network for Select Oracle]] | Kivlichan et al. 2017 | Reduces $O(\eta Da)$ adder terms to $O(1)$ queries via $O(\log\eta D)$-depth swap tree | Kivlichan et al. (2017) |
| [[MP2-Based Amplitude Prescreening for UCC]] | Romero, Babbush et al. 2018 | $\sim 2\times$ parameter reduction; $\sim 3\times$ fewer optimizer evaluations | Romero et al. (2018) |
| [[Analytical Gradient Estimation for Variational Quantum Circuits]] | Romero, Babbush et al. 2018 | $(2\delta)^2\times$ fewer gradient measurements vs finite difference | Romero et al. (2018) |
| [[Active Space Restriction for VQE (CAS-UCC)]] | Romero, Babbush et al. 2018 | Reduces qubits $N \to N_A$; parameters $O(\eta^2N^2) \to O(\eta_A^2N_A^2)$ | Romero et al. (2018) |
| [[Double Factorization of Two-Electron Integrals]] | Motta, Babbush, Chan et al. 2018 | Trotter step $O(N^2 \log N)$ gates (asymptotic) in arbitrary MO basis; $O(N^2)$ depth on linear chain | Motta et al. (2018, arXiv:1808.02625) |
| [[Partial Basis Rotation (Reduced Givens Count)]] | Motta, Babbush, Chan et al. 2018 | Reduces Givens rotation count from $\binom{N}{2}$ to $\sim N\rho_\ell$ when basis rotation has rank $\rho_\ell \ll N$ | Motta et al. (2018, arXiv:1808.02625) |
| [[Perturbative Error Correction for Truncated Hamiltonians]] | Motta, Babbush, Chan et al. 2018 | Classical correction reduces truncation error from $O(\varepsilon)$ to $O(\varepsilon^2)$ at zero quantum cost | Motta et al. (2018, arXiv:1808.02625) |
| [[Fermionic Swap Network]] | Kivlichan, McClean et al. 2018 | Trotter-step depth $N$; gates $N(N-1)/2$; linear connectivity only | Kivlichan et al. (2018) |
| [[Givens Rotation Slater Determinant Preparation]] | Kivlichan, McClean et al. 2018 | Slater-det. depth $\leq N/2$; $\eta(N-\eta)$ Givens rotations; linear connectivity | Kivlichan et al. (2018) |
| [[Plane-Wave Dual Basis]] | Babbush et al. 2018 (materials) | Reduces Hamiltonian terms from $O(N^4)$ to $\Theta(N^2)$; enables $O(N)$-depth Trotter on planar lattice | Babbush et al. (2018, PRX) |
| [[Fermionic Fast Fourier Transform (FFFT)]] | Babbush et al. 2018 (materials) | Maps between plane-wave and dual basis in $O(N)$ depth on planar lattice; $O(N \log N)$ gates | Babbush et al. (2018, PRX) |
| [[Variational Measurement Reduction via Dual Basis]] | Babbush et al. 2018 (materials) | Measures all $\Theta(N^2)$ potential terms + $N$ kinetic terms in 2 circuits; shots $O(N^4/\varepsilon^2)$ | Babbush et al. (2018, PRX) |
| [[Qubitization (Quantum Walk for Spectral Encoding)]] | Babbush, Gidney et al. 2018 | Encodes spectrum in walk-operator eigenphases; $O(\lambda/\varepsilon)$ oracle calls (Heisenberg-optimal); avoids time-evolution simulation | Low & Chuang (2017); Babbush et al. (2018, PRX) |
| [[Unary Iteration]] | Babbush, Gidney et al. 2018 | Indexed controlled operations over $L$ targets; T-count $4L-4$, ancilla $O(\log L)$ | Babbush et al. (2018, PRX) |
| [[QROM (Quantum Read-Only Memory)]] | Babbush, Gidney et al. 2018 | Coherent classical table lookup; T-count $4L-4$ for $L$ entries; bakes data as CNOTs | Babbush et al. (2018, PRX) |
| [[Coherent Alias Sampling for PREPARE]] | Babbush, Gidney et al. 2018 | Prepares $\sum_\ell \sqrt{w_\ell/\lambda}\,\|\ell\rangle$ with T-cost $O(L + \log(L/\varepsilon))$; additive precision scaling | Babbush et al. (2018, PRX) |
| [[QROAM (Space-Time Tradeoff for QROM)]] | Berry, Gidney et al. 2019 | Space-time tradeoff for QROM: $O(\sqrt{dM})$ Toffolis using $O(\sqrt{dM})$ clean ancillae or $O(dM/N)$ with $N$ dirty ancillae | Berry et al. (2019, Quantum 3, 208) |
| [[Measurement-Based QROM Uncomputation]] | Berry, Gidney et al. 2019 | X-basis measurement + classical phase fixup uncomputes QROM at cost $O(\sqrt{d})$ independent of output size $M$ | Berry et al. (2019, Quantum 3, 208) |
| [[Single Factorization for Qubitized Chemistry]] | Berry, Gidney et al. 2019 | Eigendecompose Coulomb supermatrix ($L = O(N)$ rank) for $O(N^3)$ LCU coefficients; QROAM-based PREPARE gives $\widetilde{O}(N^{3/2}\lambda)$ total | Berry et al. (2019, Quantum 3, 208) |
| [[Sparse State Preparation for Qubitized Chemistry]] | Berry, Gidney et al. 2019 | Alias sampling over nonzero entries only; symmetry expansion via controlled swaps; cost $\propto$ number of nonzero Coulomb entries | Berry et al. (2019, Quantum 3, 208) |
| [[First-Quantized Plane-Wave Chemistry Encoding]] | Babbush, Berry et al. 2019 | $O(\eta \log N)$ qubits vs. $N$ for second quantization; antisymmetry in wavefunction; enables $\lambda = O(\eta^{5/3} N^{1/3})$ | Babbush et al. (2019, npj) |
| [[Interaction Picture Simulation (Kinetic Frame)]] | Babbush, Berry et al. 2019 | Choose $A=T$, $B=U+V$ in first-quantized momentum space; $\lambda_B = O(\eta^{5/3} N^{1/3})$; gate cost $\widetilde{O}(\eta^{8/3} N^{1/3} t)$ — sublinear in $N$ | Babbush et al. (2019, npj) |
| [[LCU Sign-Parity Cancellation for Grid Boundaries]] | Babbush, Berry et al. 2019 | Parity bit $x \in \{0,1\}$ cancels out-of-bounds momentum shift terms in LCU decomposition; doubles LCU terms but leaves $\lambda$ unchanged up to constants | Babbush et al. (2019, npj) |
| [[Sequential Multi-Determinant State Preparation]] | Tubman, Babbush et al. 2018 | $O(nL)$ gate cost for $L$-term multi-determinant initial state; 1 ancilla qubit; Hamming ordering reduces constant | Tubman et al. (2018, arXiv:1809.05523) |
| [[ASCI Overlap Estimation for QPE Initial State]] | Tubman, Babbush et al. 2018 | Classical ASCI certifies ground-state overlap converges faster than energy; validates single-det. HF reference for most chemical systems | Tubman et al. (2018, arXiv:1809.05523) |
| [[Hamming-Distance Ordering for Multi-Determinant Preparation]] | Tubman, Babbush et al. 2018 | Greedy reordering of $L$ determinants minimises qubit-flip gates per step; $O(L^2 n)$ classical preprocessing | Tubman et al. (2018, arXiv:1809.05523) |
| [[Asymmetric Qubitization]] | Babbush, Berry, Neven 2019 | Two different state oracles $A$, $B$ for block encoding $H/\lambda = \langle B|V|A\rangle$; overhead $\sqrt{\langle w^2\rangle}/\langle|w|\rangle$ over symmetric qubitization; $\sqrt{\pi/2}$ for Gaussian couplings | Babbush et al. (2019, PRA 99, 040301(R)) |
| [[Directional Walk Control for Phase Doubling]] | Berry, Motlagh, Pantaleoni, Wiebe 2024 | Control $U$ vs $U^\dagger$ doubles effective phase per QSP step; halves query count for Hamiltonian simulation; requires [[Generalized Quantum Signal Processing (GQSP)\|GQSP]] for complex polynomial pairs | [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes\|Berry et al. (2024, PRA 110, 012612)]] |
| [[Random Circuit State Preparation (Gaussian Amplitudes)]] | Babbush, Berry, Neven 2019 | Random orthogonal circuit produces Gaussian-distributed amplitudes for PREPARE in $O(\mathrm{polylog}(L/\varepsilon))$ — exponentially cheaper than QROM/alias sampling when applicable | Babbush et al. (2019, PRA 99, 040301(R)) |
| [[Virtual Quantum Subspace Expansion (VQSE)]] | Takeshita, Rubin, Babbush, McClean 2019 | Recovers virtual-orbital correlation without extra qubits/depth; requires active-space 4-RDM ($\sim N_A^8$ measurements); quantum analogue of internally contracted MRCI | Takeshita et al. (2019, PRX 10, 011004) |
| [[Orbital Relaxation via RDM Postprocessing]] | Takeshita, Rubin, Babbush, McClean 2019 | Classical orbital rotation optimization using measured 1- and 2-RDMs; zero additional quantum cost; quantum analogue of CASSCF orbital step | Takeshita et al. (2019, PRX 10, 011004) |
| [[Cumulant Truncation for RDM Measurement Reduction]] | Takeshita, Rubin, Babbush, McClean 2019; Kutzelnigg & Mukherjee 1997 | Approximates 4-RDM from 2-RDM by zeroing high-order cumulants; reduces measurement from $N_A^8$ to $N_A^4$ | Takeshita et al. (2019, PRX 10, 011004) |
| [[Shadow Tomography for QMC Overlap Estimation]] | Huggins, Babbush et al. 2021 | Offline overlap estimation via classical shadows: $O(\log M/\varepsilon^2)$ measurements for $M$ overlap queries; decouples quantum and classical computers | Huggins et al. (2022, Nature 603, 416) |
| [[Virtual Correlation via Matchgate Contraction]] | Huggins, Babbush et al. 2021 | Reduces full-space overlap to active-space overlap via matchgate tensor contraction; zero extra quantum cost; more efficient than VQSE for virtual correlation | Huggins et al. (2022, Nature 603, 416) |
| [[Noise-Resilient Overlap Ratios in QC-QMC]] | Huggins, Babbush et al. 2021 | Overlap ratios cancel depolarizing noise exactly ($\langle\phi|0\rangle=0$ for nonzero particle number); extends to robust shadow tomography under Markovian noise | Huggins et al. (2022, Nature 603, 416) |
| [[Basis Rotation Grouping for VQE Measurement]] | Huggins et al. 2021 | $O(N)$ measurement circuits vs $O(N^4)$; empirical $\sim N^{2.75}$ shot scaling for H-chains | [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes\|Huggins et al. (2021)]] |
| [[Number-Basis Postselection for Error Mitigation]] | Huggins et al. 2021 | Postselect on $\eta$ and $S_z$ eigenvalues (not just parities) at zero circuit cost; readout errors suppressed to $(1-2p)^2$ independent of $N$ | [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes\|Huggins et al. (2021)]] |
| [[Qubit vs Fermionic Variance Bounds]] | Huggins et al. 2021 | Computing $\Lambda = \sum |w_\ell|$ in qubit (JW) representation gives $\sim 3\times$ tighter measurement bounds than fermionic representation | [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes\|Huggins et al. (2021)]] |
| [[DG Blocking for Block-Diagonal Hamiltonians]] | McClean, Babbush et al. 2019 | SVD-based compression of MO active space into block-local DG functions; reduces two-electron integrals from $O(N_a^4)$ to $O(N_d^2)$ (with $n_\kappa$ constant); crossover at 15–20 atoms | [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|McClean et al. (2019)]] |
| [[Block-Diagonal Swap Network for Trotter Steps]] | McClean, Babbush et al. 2019 | Trotter step depth $O(N_b n_\kappa^3)$ for block-diagonal Hamiltonians; interpolates between $O(N)$ diagonal and $O(N^3)$ general regimes | [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|McClean et al. (2019)]] |
| [[Hybrid Active Space Construction]] | McClean, Babbush et al. 2019 | Weighted density matrix combination (UHF + Gaussian) for balanced static/dynamic correlation in DG basis; 1–2 orders of magnitude DMRG speedup | [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|McClean et al. (2019)]] |
| [[Kinetic Energy as Bit-Product LCU]] | Su, Berry et al. 2021 | Decomposes $\|p\|^2$ into single-bit products; eliminates all multiplication from kinetic SELECT; cost 1 Toffoli per LCU term | Su et al. (2021, PRX Quantum 2, 040332) |
| [[Failed PREP Fallback to Alternative Hamiltonian Term]] | Su, Berry et al. 2021 | Uses failure branch of probabilistic PREPARE to apply SEL for a different Hamiltonian term; avoids $3\times$ amplitude amplification cost | Su et al. (2021, PRX Quantum 2, 040332) |
| [[Incremental Kinetic Energy Register]] | Su, Berry et al. 2021 | Maintains running $\sum_j\|k_{p_j}\|^2$ register; update per potential step $O(n_p^2)$ vs. full recomputation $O(\eta n_p^2)$; $\eta$ dependence drops to logarithmic | Su et al. (2021, PRX Quantum 2, 040332) |
| [[Qubitized Dyson Series for Phase Estimation]] | Su, Berry et al. 2021 | Qubitizes $\sin(H\tau)$ instead of amplitude-amplifying each Dyson step; factor $3/2$ savings over OAA; works when goal is eigenvalue estimation, not time evolution | Su et al. (2021, PRX Quantum 2, 040332) |
| [[Time-Reparameterization for Norm-Flattened Dyson Simulation]] | Berry, Childs, Su, Wang, Wiebe 2020 | Reparameterize $s=\int_0^t\|H(\tau)\|_{\max}d\tau$ to flatten norm profile; any Dyson-series algorithm on rescaled $\tilde{H}$ achieves $L^1$ scaling; factor of $t\max\|H\|/\int\|H\|$ improvement over $L^\infty$ bounds | [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Berry et al. (2020, Quantum 4, 254)]] |
| [[Continuous qDRIFT Time-Sampling by Instantaneous Norm]] | Berry, Childs, Su, Wang, Wiebe 2020 | Sample simulation time from $p(\tau)\propto\|H(\tau)\|_{\max}$; term from LCU at that time; avoids worst-case blowup; produces mixed-unitary channel with diamond-norm error $\le 4\|H\|_{\max,1}^2/r$ | [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Berry et al. (2020, Quantum 4, 254)]] |
| [[Integrated-Norm Equal-Segment Partitioning]] | Berry, Childs, Su, Wang, Wiebe 2020 | Split $[0,t]$ into segments with equal $\int\|H(\tau)\|\,d\tau$ (not equal width); each segment has same simulation budget; error bound uniform across segments; discrete version of time reparameterization | [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Berry et al. (2020, Quantum 4, 254)]] |
| [[Commuting Hamiltonian Rescaling for Error Mitigation]] | Tazhigulov et al. 2022 | Rescales noisy results using ratio of ideal-to-hardware data on a classically solvable commuting sub-Hamiltonian run through the same circuit ansatz | [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes\|Tazhigulov et al. (2022)]] |
| [[Floquet Gate Calibration for Circuit Recompilation]] | Tazhigulov et al. 2022; Neill et al. 2021 | Characterizes actual two-qubit gate parameters via repeated-gate experiments; feeds corrected model into circuit compilation | [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes\|Tazhigulov et al. (2022)]] |
| [[Classical Circuit Recompilation for Depth Reduction]] | Tazhigulov et al. 2022; Sun et al. 2021 | Variationally compresses exact unitary into fixed-depth native-gate circuit; state-specific fidelity optimization; not scalable but useful for benchmarking | [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes\|Tazhigulov et al. (2022)]] |
| [[Parallelised Importance Sampling for Vector Estimation]] | O'Brien, Babbush et al. 2022 | Saves up to $3N_a\times$ by optimising 2-norm of error vector jointly across measurement bases | [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes\|O'Brien et al. (2022)]] |
| [[Force Operator Block Encoding via Hamiltonian Differentiation]] | O'Brien, Babbush et al. 2022 | Differentiates THC/DF factorisation for force block-encoding at $\leq \sqrt{2}\times$ circuit overhead; $\lambda_F \ll \lambda_H$ for extended systems | [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes\|O'Brien et al. (2022)]] |
| [[Ground-State Recycling in Serial Amplitude Estimation]] | O'Brien, Babbush et al. 2022 | Recovers ground state after OEA at average cost $\leq 2T_R$; makes state preparation additive, not multiplicative, in force component count | [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes\|O'Brien et al. (2022)]] |
| [[Finite Difference Order Optimisation for Quantum Gradient Estimation]] | O'Brien, Babbush et al. 2022 | Jointly optimises FD step size and order against QPE error; $\tilde{O}(N_a^{3/2}\lambda_H/\varepsilon)$ total query complexity | [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes\|O'Brien et al. (2022)]] |
| [[FFFT Diagonalisation of Force Operators in Plane-Wave Basis]] | O'Brien, Babbush et al. 2022 | Force operators in plane waves are one-body and mutually commuting under FFFT/QFT; same $\lambda$ scaling as Hamiltonian | [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes\|O'Brien et al. (2022)]] |
| [[QROM Interpolation for Negative Exponential]] | Berry, Rubin, Babbush et al. 2024 | Compute $e^{-z}$ via base-2 conversion + QROM polynomial interpolation + bit shift; error $(1/\delta_f)^{1/3}$ scaling for quadratic interpolation | Berry et al. (2024, arXiv:2312.07654); Sanders et al. (2020) |
| [[Shared Exponential Evaluation Across Pseudopotential Terms]] | Berry, Rubin, Babbush et al. 2024 | Share single exponential evaluation between local and nonlocal pseudopotential by setting $q=0$ for local part | Berry et al. (2024, arXiv:2312.07654) |
| [[Gramian-Based Momentum Norm for Non-Cubic Cells]] | Berry, Rubin, Babbush et al. 2024 | Compute $\|k_\nu\|^2$ via reciprocal lattice Gramian with structure-specific optimisations; worst-case $O(n^2 + nb)$; diamond reduces to 3 squares | Berry et al. (2024, arXiv:2312.07654) |
| [[Arithmetic vs QROM for Pseudopotential Evaluation]] | Berry, Rubin, Babbush et al. 2024 | Use coherent arithmetic ($O(\log^2 N)$) instead of QROM ($O(N)$) for pseudopotential functions with analytical form; ~100× improvement | Berry et al. (2024, arXiv:2312.07654) |
| [[Nested Box State Preparation with Overlapping Boxes]] | Berry, Rubin, Babbush et al. 2024 | Overlapping nested boxes boost PREPARE success probability; generalises non-overlapping approach of Su et al. 2021 | Berry et al. (2024, arXiv:2312.07654) |
| [[First-Quantized Classical Shadows for k-RDM]] | Babbush, Huggins, Berry et al. 2023 | Measures $k$-RDM with $\widetilde{O}(k^k \eta^k/\epsilon^2)$ samples — polylogarithmic in basis size $N$; exploits particle-permutation symmetry in first quantization | [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes\|Babbush et al. (2023)]] |
| [[Second-to-First Quantization State Conversion]] | Babbush, Huggins, Berry et al. 2023 | On-the-fly conversion from second-quantized Givens rotation prep to first-quantized registers; $\widetilde{O}(N\eta)$ Toffolis, $O(\eta)$ ancillae | [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes\|Babbush et al. (2023)]] |
| [[Incidence Matrix Factorization for Square-Root Block Encoding]] | Babbush, Berry, Kothari, Somma, Wiebe 2023 | Factor graph Laplacian as $BB^\dagger$ via incidence matrix; block-encode $\sqrt{A}$ without matrix square root; single sparse-state preparation gives $O(\sqrt{d})$ normalization | [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes\|Babbush et al. (2023)]] |
| [[Energy-Balanced Encoding for Classical-to-Quantum Reduction]] | Babbush, Berry, Kothari, Somma, Wiebe 2023 | Encode classical oscillator state with equal kinetic/potential energy weight so norm = conserved energy; avoids condition-number-dependent overhead of naive velocity-position encoding | [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes\|Babbush et al. (2023)]] |
| [[Square-Root Sparsity in Block Encoding]] | Babbush, Berry, Kothari, Somma, Wiebe 2023 | $O(\sqrt{d})$ block-encoding normalization instead of $O(d)$ when Hamiltonian factors as $BB^\dagger$ with sparse $B$ | [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes\|Babbush et al. (2023)]] |

| [[Non-BO Projectile Block Encoding Extension]] | Rubin, Berry et al. 2023 | Extends first-quantized block encoding to quantum projectile with larger momentum grid; controlled $\nu$ preparation costs $n_n - n_p$ extra Toffolis | [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin et al. (2023)]] |
| [[QROM Interpolation plus Newton-Raphson for Inverse Square Root]] | Rubin, Berry et al. 2023 | Hybrid QROM cubic interpolation (15 bits) + one Newton-Raphson step for $b/\sqrt{x}$ at $\sim 10^{-9}$ error; $\sim 2137 + 4n^2 + 19n$ Toffolis per pair | [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin et al. (2023)]] |
| [[Gaussian Wave Packet Variance Tuning for Observable Estimation]] | Rubin, Berry et al. 2023 | Tune wave packet $\sigma_k$ to balance physical accuracy against sampling cost; calibrate via classical TDDFT convergence | [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin et al. (2023)]] |
| [[Shot-Noise vs Heisenberg-Scaling Observable Estimation Crossover]] | Rubin, Berry et al. 2023 | Monte Carlo beats KO algorithm at moderate precision ($\varepsilon \gtrsim 0.01$ Ha); first constant-factor analysis of KO algorithm via Cirq-FT | [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin et al. (2023)]] |
| [[Symplectic Corrector Injection for Product Formulas]] | Bagherimehrab, Berry et al. 2024 | Boundary-injected commutator correctors reduce PF$_{2k}$ error by factor $\alpha$ for perturbed systems; additive overhead only; ancilla-free | [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes\|Bagherimehrab et al. (2024)]] |
| [[Bernoulli Polynomial Kernel Expansion for Product Formulas]] | Bagherimehrab, Berry et al. 2024 | Exact closed-form identification of PF2 error terms via Bernoulli polynomials; enables systematic corrector design | [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes\|Bagherimehrab et al. (2024)]] |
| [[Vandermonde Compilation of Nested Commutator Exponentials]] | Bagherimehrab, Berry et al. 2024 | Decomposes commutator exponentials via Vandermonde system; $10k$ exponentials; rational solutions; extends Childs-Wiebe commutator formulas | [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes\|Bagherimehrab et al. (2024)]] |

---

## Time-Dependent Hamiltonian Simulation

### Abstract (input-model-agnostic) complexity: sparse and LCU models

| Algorithm | Year | SM gate complexity | LCU gate complexity | Norm parameter | Key technique |
|---|---|---|---|---|---|
| Monte Carlo (Poulin-Qarry-Somma-Verstraete) | 2011 | $\widetilde{O}((d^2 \|H\|_{\max,\infty} t)^2 n/\varepsilon)$ | $O((\|\alpha\|_{1,\infty} t)^2 g_e/\varepsilon)$ | $L^\infty$ in time | Hamiltonian averaging |
| Fractional-query (Berry-Childs-Cleve-Kothari-Somma 2015) | 2015 | $\widetilde{O}(d^2 \|H\|_{\max,1} n)$ queries | — | $L^1$ (queries only) | Fractional-query simulation |
| [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes\|Dyson series (Kieferová-Scherer-Berry 2018)]] | 2018 | $\widetilde{O}(d \|H\|_{\max,\infty} t\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,\infty} t\, L^2 g_c)$ | $L^\infty$ in time | Truncated Dyson + LCU |
| [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes\|Dyson/interaction picture (Low-Wiebe 2018)]] | 2018 | $\widetilde{O}(d \|H\|_{\max,\infty} t\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,\infty} t\, L^2 g_c)$ | $L^\infty$ (formal) | Interaction picture + Dyson |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Continuous qDRIFT (Berry-Childs-Su-Wang-Wiebe 2020)]] | 2020 | $\widetilde{O}((d^2 \|H\|_{\max,1})^2 n/\varepsilon)$ | $O(\|\alpha\|_{1,1}^2 g_e/\varepsilon)$ | **$L^1$ in time** | Norm-weighted time sampling |
| [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes\|Rescaled Dyson (Berry-Childs-Su-Wang-Wiebe 2020)]] | 2020 | $\widetilde{O}(d\,\|H\|_{\max,1}\, n)$ | $\widetilde{O}(\|\alpha\|_{\infty,1} L^2 g_c)$ | **$L^1$ in time** | Time reparameterization |
| [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes\|Vector-norm scaling (An-Fang-Liu 2021)]] | 2021 | Depends on $\|\psi(t)\|$ profile | — | Vector norm | Trotter + state-norm bounds |

**Notation:** $\|H\|_{\max,\infty} = \max_\tau \|H(\tau)\|_{\max}$; $\|H\|_{\max,1} = \int_0^t \|H(\tau)\|_{\max}\,d\tau$; $\|\alpha\|_{1,\infty} = \max_\tau \sum_l |\alpha_l(\tau)|$; $\|\alpha\|_{\infty,1} = \int_0^t \|\alpha(\tau)\|_\infty\,d\tau$; $\|\alpha\|_{1,1} = \int_0^t \sum_l |\alpha_l(\tau)|\,d\tau$. $g_c$ = cost of controlled-$H_l$; $g_e$ = cost of exact simulation of single term. $L$ = number of LCU terms.

The key insight of Berry-Childs-Su-Wang-Wiebe (2020): prior methods paid for $t\cdot\max_\tau\|H(\tau)\|$, scaling with peak norm over the full interval. Via [[Time-Reparameterization for Norm-Flattened Dyson Simulation|time reparameterization]], complexity reduces to the integrated norm $\int_0^t \|H(\tau)\|\,d\tau$. The improvement can be exponential when norm is concentrated on a short interval (e.g., reactive scattering).

## Classical Dynamics Simulation via Hamiltonian Simulation

### Quantum algorithms for classical differential equations

| Paper | Year | Problem | Encoding | Query complexity | Speedup | BQP-complete? |
|---|---|---|---|---|---|---|
| Costa, Jordan, Ostrander | 2019 | Wave equation (uniform, local) | Position-based ($\vec{y}$, $B^+\dot{\vec{y}}$) | $O(\text{poly}(N))$ (state prep dominated) | Polynomial | No |
| Jin, Liu, Yu | 2022 | General PDEs via Schrödingerisation | Various | Problem-dependent | Polynomial (generic) | No |
| An, Liu, Wang, Zhao | 2022 | General ODEs (theory of QDE solvers) | Standard ODE | Problem-dependent | Limited by fast-forwarding | No |
| [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes\|Berry, Costa]] | 2022 | Inhomogeneous linear ODEs ($\dot{x} = A(t)x + b(t)$) | History-state + Dyson block-encoding | $\tilde{O}(\lambda_A T \log(\lambda_{Ax}T/\varepsilon)\log(1/\varepsilon))$ calls to $U_A$ | Exponential (in $N$) | No |
| [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes\|Babbush, Berry, Kothari, Somma, Wiebe]] | 2023 | $2^n$ coupled harmonic oscillators | [[Energy-Balanced Encoding for Classical-to-Quantum Reduction\|Energy-balanced]] ($\sqrt{M}\dot{\vec{x}}$, $B^\dagger \vec{y}$) | $O(t\sqrt{\aleph d} + \log(1/\varepsilon))$ | **Exponential** | **Yes** |

The key difference between the last row and all others: the [[Energy-Balanced Encoding for Classical-to-Quantum Reduction|energy-balanced encoding]] makes the quantum state norm equal to the conserved total energy, eliminating condition-number bottlenecks from low-frequency modes. All other approaches encode $\vec{y}$ or $A\vec{y}$ directly, introducing $O(1/\omega_{\min})$ overhead.
