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
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes|Poulin-Qarry-Somma-Verstraete (2011)]] — derivative-free Trotter for arbitrary time-dependent Hamiltonians; quantum Shannon counting argument; randomised product formula
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — first efficient sparse Hamiltonian simulation
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — tight commutator-based Trotter error bounds
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — randomized product-formula ordering; permutation averaging improves error bounds
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann-Steudtner-Kueng-Kieferová-Eisert (2022)]] — randomized sampling of multi-product formulas; trades circuit depth for shots
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran-Su-Childs-Wiebe (2021)]] — symmetry-protected Trotter error cancellation
- [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes|Recursive Trotterization (2022)]] — L1-norm reduction and low-rank decomposition
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2018)]] — qDRIFT: random channel simulation with L1 norm scaling
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes|Watson (2025)]] — qFLO: Richardson extrapolation over qDRIFT step sizes; exponential depth improvement for observable estimation
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes|Martyn-Rall (2025)]] — Stochastic QSP: randomizes at the polynomial/QSP level rather than Hamiltonian term level; halves $\log(1/\epsilon)$ query cost across all QSP-family algorithms

### Taylor series / LCU methods
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — linear combination of unitaries, the foundational technique
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes|Berry-Cleve-Somma (2013)]] — first polylog$(1/\varepsilon)$ simulation via compressed fractional-query Trotter; broke the precision barrier
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2014)]] — full STOC version; introduces OAA, proves optimal $\varepsilon$-lower bound
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2015)]] — truncated Taylor series + LCU + robust OAA; near-optimal $\log(1/\varepsilon)$ precision scaling
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes|Berry-Childs-Kothari (2015)]] — Bessel-LCU of walk steps; optimal in both sparsity and precision
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry-Childs (2011)]] — black-box simulation via sparse oracles
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — Fourier and Chebyshev LCU for matrix inversion
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (2023)]] — improved LCHS with generalised Hardy-space kernels; polylog precision + optimal state-prep for linear ODEs
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|An-Childs-Lin (2024)]] — Lap-LCHS: extends LCHS to general eigenvalue transforms via Laplace representation; LCU of Hamiltonian simulation unitaries

### Time-dependent / unbounded
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes|An-Fang-Liu (2021)]] — vector-norm Trotter bounds for unbounded time-dependent Hamiltonians; cost independent of $\|H\|$ for smooth initial states

### Dyson series / interaction picture
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]] — truncated Dyson series for time-dependent Hamiltonians
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]] — interaction-picture simulation with norm reduction
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes|Berry-Babbush-et al. (2020)]] — L1-norm scaling for time-dependent simulation
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes|An-Fang-Lin (2022)]] — qHOP: first-order Magnus + LCU quadrature; commutator scaling without time-ordering; superconvergence for Schrödinger ($\log N$ dependence)

### Qubitization / QSP / QSVT
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2016–2017)]] — optimal simulation via quantum signal processing
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — qubitization iterate and block-encoding framework
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — quantum singular value transformation, unifying framework
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low-Yoder-Chuang (2016)]] — composite pulse sequences and QSP precursors
- [[Shadow Hamiltonian Simulation — Paper Notes|Shadow Hamiltonian Simulation]] — simulation via shadow tomography techniques
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|Sublinear-T Block-Encodings (2024)]] — resource-efficient block-encodings for chemistry
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes|Martyn-Liu-Chin-Chuang (2023)]] — fully-coherent QSP/QET simulation; one-shot algorithm bypasses parity obstruction via affine spectrum compression, achieving $\Theta(\alpha|t| + \log(1/\epsilon) + \log(1/\delta))$ with no amplitude amplification
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes|Martyn-Rall (2025)]] — Stochastic QSP; halves query complexity of all QSP-based algorithms via randomized compiling + Hastings-Campbell mixing lemma; output is a channel (not a unitary)
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|Dong-Meng-Whaley-Lin (2021)]] — QSPPACK: optimisation-based QSP phase-factor computation in standard double precision for $d > 10{,}000$; perturbative initial guess + L-BFGS + scaling-and-squaring
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang-Dong-Lin (2022)]] — energy landscape analysis of QSP phase-factor optimisation; proves local strong convexity near fixed initial guess for $\|f\|_\infty = O(d^{-1})$; first provably convergent double-precision algorithm

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
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn-Rossi-Tan-Chuang (2021)]] — pedagogical tutorial: QSP → QET → QSVT unification; constructs search, QPE, simulation, matrix inversion as QSVT instances
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
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes|Tong-An-Wiebe-Lin (2021)]] — preconditioned QLSP via fast inversion; Green's function computation; Gibbs state preparation via contour integral and inverse-transform approaches
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first quantum algorithm for general linear ODEs; history-state encoding + HHL, $\tilde{O}(\Delta t^2)$ scaling
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — linear ODEs via quantum simulation
- [[Quantum Linear Matrix Equations — Paper Notes|Quantum Linear Matrix Equations]] — matrix equation generalisation
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — improved linear system solvers
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin (2019)]] — near-optimal QLSA via time-optimal adiabatic scheduling; AQC(p) achieves $O(\kappa/\varepsilon)$, AQC(exp) gets polylog precision; QAOA connection
- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|Dong-An-Lin (2020)]] — eigenstate filtering via QSP + Chebyshev minimax; QLSP in $\tilde{O}(d\kappa\log(1/\epsilon))$ via AQC or Zeno, no phase estimation or amplitude amplification
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] — time-marching ODE solver via uniform singular value amplification + compression gadget; linear dependence on amplification ratio $Q$ (proven tight); handles non-diagonalisable, non-smooth $A(t)$
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (2023)]] — improved LCHS: generalised kernel identity + Hardy-space kernel gives polylog precision scaling with optimal state-prep cost; first quantum ODE solver near-optimal on all parameters simultaneously
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|An-Childs-Lin (2024)]] — Lap-LCHS: general eigenvalue transforms $h(A)$ via Laplace representation + LCHS; covers fractional powers, $e^{-TA^{-1}}$, inhomogeneous ODEs; optimal state-preparation cost; requires dissipative $A$

---

## Eigenvalue / Ground State Problems

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd (1999)]] — quantum phase estimation for eigenvalues
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|Aspuru-Guzik et al. (2005)]] — first quantum chemistry calculations (H₂O, LiH); recursive PEA, adiabatic state prep
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes|Kassal et al. (2008)]] — full chemical dynamics beyond Born-Oppenheimer; split-operator real-space simulation in $O(B^2 m^2)$
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Temme et al. (2011)]] — quantum Metropolis for Gibbs state preparation; sign-problem-free sampling from eigenbasis
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes|Tong-An-Wiebe-Lin (2021)]] — Gibbs state preparation via preconditioned contour integral and inverse-transform approaches
- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|Dong-An-Lin (2020)]] — optimal eigenstate filtering via QSP + Chebyshev minimax polynomials; applied to QLSP via AQC and Zeno routes; $\tilde{O}(d\kappa\log(1/\epsilon))$ without phase estimation or amplitude amplification
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin-Tong (2020)]] — near-optimal ground state preparation via signal processing
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin-Tong (2022)]] — Heisenberg-limited ground-state energy estimation with 1 ancilla qubit; early fault-tolerant regime; spectral CDF approach
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|Dong-Lin-Tong (2022)]] — QET-U: eigenvalue transformation via Hamiltonian evolution; ground-state energy $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ with 1 ancilla (short depth) or $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ with 3 ancillas (near-optimal); control-free variant for spin models
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes|Ding-Chen-Lin (2023)]] — quantum MCMC via Lindbladian with single ancilla qubit; no initial overlap needed; $\tilde{O}(T^{1+1/p}\epsilon^{-1/p})$ total Hamiltonian simulation time; mixing time is the key hardness parameter
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo-McClean et al. (2014)]] — VQE: variational quantum eigensolver
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes|Epperly-Lin-Nakatsukasa (2022)]] — rigorous analysis of QSD with Löwdin thresholding; explains why noisy generalized eigenvalue problems from Krylov subspaces work; exponential Krylov convergence via trigonometric minimax polynomials
- [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes|Ding-Li-Lin-Ni-Ying-Zhang (2024)]] — QMEGS: first algorithm achieving Heisenberg-limited multiple eigenvalue estimation with reduced circuit depth for gapped systems; unifies gapped and gapless regimes via Gaussian-filtered Hadamard tests

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
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin (2019)]] — time-optimal adiabatic schedules for QLSP; generalises Roland-Cerf local adiabatic method

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
