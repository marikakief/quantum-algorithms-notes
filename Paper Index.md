# Paper Index

All paper notes in this vault, organised by topic. A paper may appear under multiple headings if it spans areas. Links use Obsidian wikilink format.

---

## Foundational / Historical

- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — quantum computation model, Deutsch problem
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes|Deutsch-Jozsa (1992)]] — first exponential quantum speedup (vs. deterministic); Hadamard-sandwich oracle algorithm
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani (1993)]] — BQP definition; first quantum-vs-randomised separation; hidden linear function in 1 query
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes|Jordan (2005)]] — $d$-dimensional gradient in 1 query via Fourier-eigenstate phase kickback (Bernstein-Vazirani generalisation)
- [[Optimizing Quantum Optimization Algorithms via Faster Quantum Gradient Computation (Gilyén-Arunachalam-Wiebe 2019) — Paper Notes|Gilyén-Arunachalam-Wiebe (2019)]] — improved gradient estimation with higher-order stencils; probability ↔ phase oracle conversion
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — proves Feynman's conjecture: quantum computers efficiently simulate local quantum systems
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — exponential quantum speedup for Simon's problem
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — polynomial-time factoring and discrete log
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — unstructured search in $O(\sqrt{N})$ queries
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation, abelian stabiliser problem
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes|Cleve-Ekert-Macchiavello-Mosca (1998)]] — unified phase estimation framework; standard QPE circuit; QFT construction; Shor = phase estimation

---

## Hamiltonian Simulation

### Product formulas (Trotter / Suzuki)
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — foundational product-formula simulation of local Hamiltonians; proves Feynman's conjecture
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — first efficient sparse Hamiltonian simulation
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — tight commutator-based Trotter error bounds
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Faehrmann-Steudtner-Kueng-Kieferová-Eisert (2022)]] — randomised multi-product formulas
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran-Su-Childs-Wiebe (2021)]] — symmetry-protected Trotter error cancellation
- [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes|Recursive Trotterization (2022)]] — L1-norm reduction and low-rank decomposition
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2018)]] — qDRIFT: random channel simulation with L1 norm scaling

### Taylor series / LCU methods
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — linear combination of unitaries, the foundational technique
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes|Berry-Cleve-Somma (2013)]] — first polylog$(1/\varepsilon)$ simulation via compressed fractional-query Trotter; broke the precision barrier
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2014)]] — full STOC version; introduces OAA, proves optimal $\varepsilon$-lower bound
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2015)]] — truncated Taylor series + LCU + robust OAA; near-optimal $\log(1/\varepsilon)$ precision scaling
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes|Berry-Childs-Kothari (2015)]] — Bessel-LCU of walk steps; optimal in both sparsity and precision
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

### Quantum field theory
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes|Jordan-Lee-Preskill (2012)]] — first algorithm for scattering in $\phi^4$ QFT; polynomial in particles, energy, precision; EFT discretisation analysis

### Fermionic systems
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams-Lloyd (1997)]] — first fermionic simulation algorithms; antisymmetrization, Hubbard model
- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes|Ortiz-Gubernatis-Knill-Laflamme (2001)]] — early fermionic simulation framework
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes|Fermionic Eigenstate Prep (2018)]] — improved eigenstate preparation for fermionic systems

---

## Quantum Signal Processing and Block Encodings

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT: polynomial transformations of singular values
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes|Quantum Eigenvalue Processing for Non-Normal Matrices (2024)]] — QSVT extension to non-normal matrices
- [[SOSSA — Sum-of-Squares Spectral Amplification.md|SOSSA — Sum-of-Squares Spectral Amplification]] — SOS-based spectral amplification
- [[Quantum Hermite Transform — Paper Notes|Quantum Hermite Transform]] — Hermite polynomial quantum transform

---

## Amplitude Amplification and Estimation

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — original $O(\sqrt{N})$ search
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr-Høyer (1996)]] — minimum finding in $O(\sqrt{N})$ via Grover with evolving threshold
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification and estimation, general framework
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard-Høyer-Tapp (1997)]] — collision finding via Grover
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — near-quadratic speedup for Monte Carlo mean estimation; partition function applications via quantum walks
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes|Rebentrost-Gupt-Bromley (2018)]] — quantum amplitude estimation applied to European and Asian option pricing; quadratic speedup over classical MC

---

## Quantum Walks

- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs-Cleve-Deotto-Farhi-Gutmann-Spielman (2003)]] — first exponential speedup via quantum walks (not QFT); continuous-time walk on glued trees
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — quantum walk algorithmic survey and framework
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|Ambainis-Childs-Reichardt-Špalek-Zhang (2007)]] — optimal $O(\sqrt{N})$ quantum evaluation of AND-OR formulas via coined walk
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — element distinctness via quantum walk
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — quadratic speedup for Markov chain algorithms
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|Magniez-Nayak-Roland-Santha (2007)]] — unified quantum walk search; combines Ambainis and Szegedy frameworks with separated $S + \frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}} U + C)$ cost
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs-Goldstone (2004)]] — continuous-time spatial search
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — equivalence of continuous and discrete quantum walks
- [[Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) — Paper Notes|Knill-Ortiz-Somma (2007)]] — quantum walk simulation of classical annealing

---

## Linear Systems and Differential Equations

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — HHL: exponential speedup for linear systems
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first quantum algorithm for general linear ODEs; history-state encoding + HHL, $\tilde{O}(\Delta t^2)$ scaling
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — linear ODEs via quantum simulation
- [[Quantum Linear Matrix Equations — Paper Notes|Quantum Linear Matrix Equations]] — matrix equation generalisation
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — improved linear system solvers

---

## Eigenvalue / Ground State Problems

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd (1999)]] — quantum phase estimation for eigenvalues
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|Aspuru-Guzik et al. (2005)]] — first quantum chemistry calculations (H₂O, LiH); recursive PEA, adiabatic state prep
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes|Kassal et al. (2008)]] — full chemical dynamics beyond Born-Oppenheimer; split-operator real-space simulation in $O(B^2 m^2)$
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Temme et al. (2011)]] — quantum Metropolis for Gibbs state preparation; sign-problem-free sampling from eigenbasis
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
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] — local adiabatic schedule recovers $O(\sqrt{N})$ for search; proves optimality
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — adiabatic ↔ circuit equivalence
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov-Ta-Shma (2003)]] — adiabatic state generation and SZK
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo (2013)]] — quadratic gap amplification for frustration-free Hamiltonians; optimal; impossible for general $H$

---

## Complexity Theory (QMA, BQP)

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — local Hamiltonian is QMA-complete
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe-Regev (2003)]] — 3-local Hamiltonian QMA-completeness
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|Kempe-Kitaev-Regev (2006)]] — 2-local Hamiltonian QMA-completeness
- [[QMA-Completeness of N-Representability (Liu-Christandl-Verstraete 2007) — Paper Notes|Liu-Christandl-Verstraete (2007)]] — N-representability is QMA-complete
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — quantum algorithms for group-theoretic problems
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes|Buhrman-Cleve-Watrous-de Wolf (2001)]] — exponential quantum communication advantage
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV (1997)]] — $\Omega(\sqrt{N})$ search lower bound (Grover is optimal); BQP$^{\text{BQP}}$ = BQP via tidy subroutines


## Quantum Supremacy / Hardness of Sampling

- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] — IQP sampling hardness; post-IQP = PP; Hadamard gadget

---

## State Preparation

- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph (2002)]] — recursive bisection state preparation for log-concave distributions; $O(\log N)$ rotations
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] — arithmetic-free state preparation via inequality test; 286–375× Toffoli reduction over Grover's arcsine approach

---

## Gate Synthesis and Compilation

- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Dawson-Nielsen (2005)]] — Solovay-Kitaev theorem and efficient gate compilation
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low-Yoder-Chuang (2016)]] — composite pulse sequences for robust gate synthesis
