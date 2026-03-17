# Paper Index

All paper notes in this vault, organised by topic. A paper may appear under multiple headings if it spans areas. Links use Obsidian wikilink format.

---

## Foundational / Historical

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — quantum computation model, Deutsch problem
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — exponential quantum speedup for Simon's problem
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — polynomial-time factoring and discrete log
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — unstructured search in $O(\sqrt{N})$ queries
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation, abelian stabiliser problem

---

## Hamiltonian Simulation

### Product formulas (Trotter / Suzuki)
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — first efficient sparse Hamiltonian simulation
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — tight commutator-based Trotter error bounds
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Faehrmann-Steudtner-Kueng-Kieferová-Eisert (2022)]] — randomised multi-product formulas
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran-Su-Childs-Wiebe (2021)]] — symmetry-protected Trotter error cancellation
- [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes|Recursive Trotterization (2022)]] — L1-norm reduction and low-rank decomposition
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2018)]] — qDRIFT: random channel simulation with L1 norm scaling

### Taylor series / LCU methods
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — linear combination of unitaries, the foundational technique
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry-Childs (2011)]] — black-box simulation via sparse oracles
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — Fourier and Chebyshev LCU for matrix inversion

### Dyson series / interaction picture
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]] — truncated Dyson series for time-dependent Hamiltonians
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]] — interaction-picture simulation with norm reduction
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes|Berry-Babbush-et al. (2020)]] — L1-norm scaling for time-dependent simulation

### Qubitization / QSP / QSVT
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2016–2017)]] — optimal simulation via quantum signal processing
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — qubitization iterate and block-encoding framework
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — quantum singular value transformation, unifying framework
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low-Yoder-Chuang (2016)]] — composite pulse sequences and QSP precursors
- [[Shadow Hamiltonian Simulation — Paper Notes|Shadow Hamiltonian Simulation]] — simulation via shadow tomography techniques
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|Sublinear-T Block-Encodings (2024)]] — resource-efficient block-encodings for chemistry

### Specialised / structured Hamiltonians
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes|PDE Hamiltonian Simulation (2021)]] — QFT/QSFT/QCT for structured PDE Hamiltonians
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes|Coupled Classical Oscillators (2020)]] — exponential speedup via Hamiltonian simulation
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes|Childs-Kothari-Somma (2010)]] — lower bounds for non-sparse simulation

### Fermionic systems
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes|Fermionic Eigenstate Prep (2018)]] — improved eigenstate preparation for fermionic systems
- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes|Ortiz-Gubernatis-Knill-Laflamme (2001)]] — early fermionic simulation framework

---

## Quantum Signal Processing and Block Encodings

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT: polynomial transformations of singular values
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes|Quantum Eigenvalue Processing for Non-Normal Matrices (2024)]] — QSVT extension to non-normal matrices
- [[SOSSA — Sum-of-Squares Spectral Amplification.md|SOSSA — Sum-of-Squares Spectral Amplification]] — SOS-based spectral amplification
- [[Quantum Hermite Transform — Paper Notes|Quantum Hermite Transform]] — Hermite polynomial quantum transform

---

## Amplitude Amplification and Estimation

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — original $O(\sqrt{N})$ search
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification and estimation, general framework
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard-Høyer-Tapp (1997)]] — collision finding via Grover

---

## Quantum Walks

- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — quantum walk algorithmic survey and framework
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — element distinctness via quantum walk
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — quadratic speedup for Markov chain algorithms
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs-Goldstone (2004)]] — continuous-time spatial search
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — equivalence of continuous and discrete quantum walks
- [[Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) — Paper Notes|Knill-Ortiz-Somma (2007)]] — quantum walk simulation of classical annealing

---

## Linear Systems and Differential Equations

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — HHL: exponential speedup for linear systems
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — linear ODEs via quantum simulation
- [[Quantum Linear Matrix Equations — Paper Notes|Quantum Linear Matrix Equations]] — matrix equation generalisation
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — improved linear system solvers

---

## Eigenvalue / Ground State Problems

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation
- [[Quantum Algorithm for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd (1999)]] — quantum phase estimation for eigenvalues
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin-Tong (2020)]] — near-optimal ground state preparation via signal processing
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo-McClean et al. (2014)]] — VQE: variational quantum eigensolver

---

## Quantum Machine Learning

- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes|Lloyd-Mohseni-Rebentrost (2014)]] — quantum PCA

---

## Variational and Near-Term Algorithms

- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|Farhi-Goldstone-Gutmann (2014)]] — QAOA for combinatorial optimisation
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo-McClean et al. (2014)]] — VQE

---

## Adiabatic Quantum Computation

- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi-Goldstone-Gutmann-Sipser (2000)]] — adiabatic quantum computation proposal
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — adiabatic ↔ circuit equivalence
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov-Ta-Shma (2003)]] — adiabatic state generation and SZK

---

## Complexity Theory (QMA, BQP)

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — local Hamiltonian is QMA-complete
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe-Regev (2003)]] — 3-local Hamiltonian QMA-completeness
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|Kempe-Kitaev-Regev (2006)]] — 2-local Hamiltonian QMA-completeness
- [[QMA-Completeness of N-Representability (Liu-Christandl-Verstraete 2007) — Paper Notes|Liu-Christandl-Verstraete (2007)]] — N-representability is QMA-complete
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — quantum algorithms for group-theoretic problems
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes|Buhrman-Cleve-Watrous-de Wolf (2001)]] — exponential quantum communication advantage

---

## Gate Synthesis and Compilation

- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Dawson-Nielsen (2005)]] — Solovay-Kitaev theorem and efficient gate compilation
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low-Yoder-Chuang (2016)]] — composite pulse sequences for robust gate synthesis
