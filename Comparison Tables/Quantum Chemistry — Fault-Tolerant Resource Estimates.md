_Comparison tables for fault-tolerant quantum chemistry and materials simulation resource estimates._

_Split from [[Hamiltonian Simulation — Comparison Tables]]._

---

### Quantum chemistry simulation algorithms: asymptotic gate complexity

| Paper                                                                                                                                                                                                                  | Year                  | Representation                                                                 | Qubits                                 | Gate complexity                                                                                                                                      | Precision scaling                       |                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------------------------------------------------------------------------------ | -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- | ------------------ |
| [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]]                                                                                                       | Aspuru-Guzik et al.]] | 2005                                                                           | Second-quantized (JW)                  | $O(N)$                                                                                                                                               | $\tilde{O}(N^{11} t / \varepsilon)$     | $O(1/\varepsilon)$ |
| [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes\|Wecker, Hastings, Troyer]]                                                                                  | 2015                  | Second-quantized (BK), Trotter                                                 | $O(N)$                                 | $O(N^{8+o(1)} t / \varepsilon^{o(1)})$                                                                                                               | $O(1/\varepsilon)$                      |                    |
| [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes\|Babbush et al. 2016 (NJP) — database]]                          | 2016                  | Second-quantized, Taylor series                                                | $O(N)$                                 | $\tilde{O}(N^4 \|H\| t)$                                                                                                                             | $O(\log 1/\varepsilon)$                 |                    |
| [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes\|Babbush et al. 2016 (NJP) — on-the-fly]]                        | 2016                  | Second-quantized, Taylor series                                                | $O(N)$                                 | $\tilde{O}(N^5 t)$                                                                                                                                   | $O(\log 1/\varepsilon)$                 |                    |
| [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes\|Babbush et al. 2018]]                                                                         | 2018                  | First-quantized (CI matrix), Taylor series                                     | $\tilde{O}(\eta)$                      | $\tilde{O}(\eta^2 N^3 t)$                                                                                                                            | $O(\log 1/\varepsilon)$                 |                    |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes\|Kivlichan et al. 2017]] (fixed $h$)                                        | 2017                  | First-quantized (real-space grid), Taylor series                               | $\eta D \lceil\log b\rceil$            | $\tilde{O}(\eta^2 t)$ (pairwise oracle)                                                                                                              | $O(\log 1/\varepsilon)$                 |                    |
| [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes\|Kivlichan et al. 2017]] (Corollary 5)                                      | 2017                  | First-quantized (real-space grid), Taylor series                               | $\eta D \lceil\log b\rceil$            | $\tilde{O}(\eta^7 t^3/\varepsilon^2)$                                                                                                                | $O(1/\varepsilon^2)$                    |                    |
| [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes\|Kivlichan, McClean et al. 2018]]                                                        | 2018                  | Second-quantized (JW), Trotter                                                 | $N$                                    | $N(N{-}1)/2$ entangling gates per step; total $O(N^2 t/\varepsilon)$                                                                                 | $O(1/\varepsilon)$                      |                    |
| [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes\|Motta, Babbush, Chan et al. 2018 (double factorization)]]                                   | 2018                  | Second-quantized (JW), double factorization + Trotter                          | $N$                                    | $O(N^2 \log N)$ per step (asymptotic) / $O(N^3)$ (fixed molecule)                                                                                    | $O(1/\varepsilon)$                      |                    |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018 (dual basis, Trotter)]]                                                                            | 2018                  | Second-quantized (JW), plane-wave dual, Trotter                                | $N$                                    | depth $O(N^{7/2} t^{3/2}/\sqrt{\varepsilon})$ (fixed density)                                                                                        | $O(1/\sqrt{\varepsilon})$               |                    |
| [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes\|Babbush et al. 2018 (dual basis, LCU on-the-fly)]]                                                                     | 2018                  | Second-quantized (JW), plane-wave dual, Taylor series                          | $N$                                    | gates $\widetilde{O}(N^{11/3})$; depth $\widetilde{O}(N^{8/3})$ (fixed density)                                                                      | $O(\log 1/\varepsilon)$                 |                    |
| [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes\|Babbush, Gidney et al. 2018 (qubitization)]]                                                    | 2018                  | Second-quantized, plane-wave dual, qubitization                                | $O(\log(\lambda N/\varepsilon))$       | T gates $O(N^3/\varepsilon + N^2 \log(1/\varepsilon)/\varepsilon)$; per-oracle T = $O(N)$                                                            | $O(1/\varepsilon)$ (Heisenberg-limited) |                    |
| [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes\|Babbush, Berry et al. 2019 (interaction picture)]]                                         | 2019                  | First-quantized, plane-wave, interaction picture                               | $O(\eta \log N)$                       | $\widetilde{O}(N^{1/3} \eta^{8/3} t)$                                                                                                                | $O(\log 1/\varepsilon)$                 |                    |
| [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes\|Babbush, Huggins, Berry et al. 2023 (1st-quant. Trotter)]] | 2023                  | First-quantized, real-space grid, high-order Trotter                           | $O(\eta \log N)$                       | $(N^{1/3}\eta^{7/3}t + N^{2/3}\eta^{4/3}t)(Nt/\varepsilon)^{o(1)}$                                                                                   | $O(1/\varepsilon^{o(1)})$               |                    |
| [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes\|Su, Berry et al. 2021 (qubitization)]]                                                  | 2021                  | First-quantized, plane-wave, qubitization                                      | $O(\eta \log N)$                       | $\widetilde{O}((\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})/\varepsilon)$                                                                                 | $O(1/\varepsilon)$ (Heisenberg)         |                    |
| [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes\|Su, Berry et al. 2021 (interaction picture)]]                                           | 2021                  | First-quantized, plane-wave, interaction picture                               | $O(\eta \log N)$                       | $\widetilde{O}(\eta^{8/3}N^{1/3}/\varepsilon)$                                                                                                       | $O(1/\varepsilon)$ (qubitized Dyson)    |                    |
| [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|McClean, Babbush et al. 2019 (DG + LCU)]]                                                | 2019                  | Second-quantized, DG block-diagonal basis, LCU                                 | $N_d$                                  | $O(n_\kappa^2 N_b \lambda t)$; empirically $O(N_h^{2.6} t)$ (H-chain)                                                                                | $O(\log 1/\varepsilon)$                 |                    |
| [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes\|McClean, Babbush et al. 2019 (DG + Trotter)]]                                            | 2019                  | Second-quantized, DG block-diagonal basis, Trotter                             | $N_d$                                  | Trotter-step depth $O(N_b n_\kappa^3)$                                                                                                               | $O(1/\varepsilon)$                      |                    |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry, Gidney et al. 2019 (single factorization)]]     | 2019                  | Second-quantized, arbitrary basis, single factorization + qubitization         | $O(N)$                                 | $\widetilde{O}(N^{3/2}\lambda/\varepsilon)$                                                                                                          | $O(1/\varepsilon)$ (Heisenberg)         |                    |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry, Gidney et al. 2019 (sparse Coulomb)]]           | 2019                  | Second-quantized, arbitrary basis, sparse Coulomb + qubitization               | $O(N)$                                 | System-dependent; $\widetilde{O}(\lambda L_V^{(c)}/\varepsilon)$                                                                                     | $O(1/\varepsilon)$ (Heisenberg)         |                    |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee, Berry et al. 2021 (THC qubitization)]]                                    | 2021                  | Second-quantized, arbitrary basis, THC + qubitization                          | $\widetilde{O}(N)$                     | $\widetilde{O}(N\lambda_\zeta/\varepsilon)$; empirically $\widetilde{O}(N^{2.1}/\varepsilon)$ (H-chain) to $\widetilde{O}(N^{3.1}/\varepsilon)$ (H₄) | $O(1/\varepsilon)$ (Heisenberg)         |                    |
| [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (Bloch DF)]]                                                          | 2023                  | Second-quantized, Bloch orbitals, DF + qubitization                            | $\widetilde{O}(\sqrt{N_k}N)$           | $\widetilde{O}(\sqrt{N_k} N \sqrt{\Xi}\, \lambda_{\rm DF}/\varepsilon)$                                                                              | $O(1/\varepsilon)$ (Heisenberg)         |                    |
| [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (Bloch THC)]]                                                         | 2023                  | Second-quantized, Bloch orbitals, k-THC + qubitization                         | $\widetilde{O}(N_k N)$                 | $\widetilde{O}(N_k N\, \lambda_{\rm THC}/\varepsilon)$                                                                                               | $O(1/\varepsilon)$ (Heisenberg)         |                    |
| [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes\|Berry, Rubin et al. 2024 (1st-quant. GTH pseudopotential)]]       | 2024                  | First-quantized, plane-wave, GTH pseudopotential + qubitization                | $O(\eta \log N)$                       | Block encoding $\widetilde{O}(\eta)$; QPE $O(\eta \lambda/\varepsilon)$ with $\lambda$ dominated by $\lambda_{\rm nonloc}$                           | $O(1/\varepsilon)$ (Heisenberg)         |                    |
| [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (stopping power, QSP)]]                                           | 2023                  | First-quantized, plane-wave, non-BO, QSP                                       | $O(\eta \log N)$                       | $\widetilde{O}((\eta^2/\Delta^2 + \eta^3/\Delta) \cdot t \cdot N_s)$                                                                                 | $O(\log 1/\varepsilon)$                 |                    |
| [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes\|Rubin, Berry et al. 2023 (stopping power, 8th-order Trotter)]]                             | 2023                  | First-quantized, real-space grid, non-BO, [[Product Formulas]]                  | $O(\eta \log N)$                       | $\sim O(\eta^2)$ at fixed $r_s$; bespoke 8th-order formula with $\xi = 3.4 \times 10^{-8}$                                                           | $O(1/\varepsilon^{1/k})$                |                    |
| [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes\|Berry, Wan et al. 2025 (quantum FMM)]]                                         | 2025                  | First-quantized, real-space grid, quantum FMM + high-order [[Product Formulas]] | $O(\eta \log N \log^3(1/\varepsilon))$ | $(\eta^{4/3}N^{1/3} + \eta^{1/3}N^{2/3})(\eta N/\varepsilon)^{o(1)}$                                                                                 | $O(1/\varepsilon^{o(1)})$               |                    |


### Qubitization-based chemistry: FeMoCo resource estimates

| Algorithm | Logical qubits (Reiher) | Toffoli count (Reiher) | Logical qubits (Li) | Toffoli count (Li) |
|---|---|---|---|---|
| [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes\|Reiher et al. 2017 (Trotter)]] | 111 | $5.0 \times 10^{13}$ | — | — |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry et al. 2019 (sparse)]] | 2,190 | $8.8 \times 10^{10}$ | 2,489 | $4.4 \times 10^{10}$ |
| [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes\|Berry et al. 2019 (single factorization)]] | 3,320 | $9.5 \times 10^{10}$ | 3,628 | $1.2 \times 10^{11}$ |
| [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes\|von Burg et al. 2020 (double factorization)]] | 3,725 | $1.0 \times 10^{10}$ | 6,404 | $6.4 \times 10^{10}$ |
| [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes\|Lee, Berry et al. 2021 (THC)]] | **2,142** | $\mathbf{5.3 \times 10^9}$ | **2,196** | $\mathbf{3.2 \times 10^{10}}$ |
| [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes\|Berry, Tong et al. 2024 (THC + MPS + QPE)]] | — | — | 2,194 | $7.3 \times 10^{10}$ (95% CI) |
| [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes\|Berry, Tong et al. 2024 (DF + MPS + QPE)]] | — | — | 6,402 | $1.1 \times 10^{11}$ (95% CI) |
| [[Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024) — Paper Notes\|Oumarou et al. 2024 (RC-DF)]] | 1,994 | $2.6 \times 10^9$ | — | — |
| [[Faster Quantum Chemistry Simulations via Improved Tensor Factorization and Active Volume Compilation (Caesura, Cortes, Pol, Sim, Steudtner, Anselmetti, Degroote, Moll, Santagati, Streif, Tautermann 2025) — Paper Notes\|Caesura et al. 2025 (BLISS-THC + AV)]] | — | — | 1,512 | $4.3 \times 10^9$ |
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
| 1st quant. PW ([[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry et al. 2024]]) | C2/m | [5,5,5] | 3,310,000 | $6.0 \times 10^{13}$ | ~1,400 |
| 1st quant. PW ([[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry et al. 2024]]) | C2/m | [6,6,6] | 3,490,000 | $7.4 \times 10^{13}$ | ~1,600 |
| 2nd quant. DF ([[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin et al. 2023]]) | C2/m | DZVP | 4,874 | $1.2 \times 10^{12}$ | ~75,000 |
| 1st quant. PW ([[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry et al. 2024]]) | P21/c | [5,5,5] | 3,340,000 | $6.0 \times 10^{13}$ | ~1,400 |
| 2nd quant. DF ([[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin et al. 2023]]) | P21/c | DZVP | 3,958 | $1.3 \times 10^{12}$ | ~75,000 |
| 1st quant. PW ([[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry et al. 2024]]) | P2/c | [5,5,5] | 3,240,000 | $5.9 \times 10^{13}$ | ~1,400 |
| 2nd quant. DF ([[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin et al. 2023]]) | P2/c | DZVP | 2,754 | $9.7 \times 10^{11}$ | ~75,000 |

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
| [[Digital Quantum Simulation of Minimal AdS-CFT (García-Álvarez, Egusquiza, Lamata, del Campo, Sonner, Solano 2017) — Paper Notes\|García-Álvarez et al.]] | 2017 | Lie-Trotter | $O(N^{10} t^2/\varepsilon)$ | $O(1/\varepsilon)$ |
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


---

**See also:** [[Quantum Chemistry — NISQ Experiments & VQE]] · [[Quantum Chemistry — Circuit Primitives]] · [[Quantum Chemistry — Technique Crosswalk]] · [[Hamiltonian Simulation — Time-Dependent Methods]] · [[FeMoCo Resource Estimation Timeline]]
