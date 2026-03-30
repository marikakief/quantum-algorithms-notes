
All paper notes in this vault, organised by topic. A paper may appear under multiple headings if it spans areas. Links use Obsidian wikilink format.

---

## Foundational / Historical

- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes|Feynman (1982)]] — vision paper: classical computers can't efficiently simulate quantum physics; proposes quantum computers as simulators
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — quantum computation model, Deutsch problem
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|BBHT (1998)]] — exact analysis of Grover's algorithm, unknown-$t$ search, $\Omega(\sqrt{N/t})$ lower bound
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] — amplitude amplification framework, quantum counting via QFT + Grover, heuristic speedup
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
- [[Quantum Teleportation is a Universal Computational Primitive (Gottesman-Chuang 1999) — Paper Notes|Gottesman-Chuang (1999)]] — gate teleportation: any gate via teleportation through entangled resource states; Clifford hierarchy; origin of magic states for fault tolerance
- [[A One-Way Quantum Computer (Raussendorf-Briegel 2001) — Paper Notes|Raussendorf-Briegel (2001)]] — measurement-based quantum computation: universal QC via single-qubit measurements on cluster states; no unitary gates after initial entanglement
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes|Verstraete-Wolf-Cirac (2009)]] — universal QC via engineered dissipation; local Lindblad operators drive system to steady state encoding computation output; also dissipative preparation of frustration-free ground states (MPS, PEPS, topological codes)
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal (2003)]] — classical simulation via MPS when Schmidt rank $\chi = \text{poly}(n)$; entanglement growth is necessary for quantum advantage; launched tensor network simulation
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes|Vidal (2004)]] — TEBD algorithm: practical MPS simulation of 1D dynamics via Trotter decomposition into local gates; cost $O(n(d\chi)^3)$ per step
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes|Verstraete-Cirac (2004)]] — introduces PEPS; extends MPS/TEBD/DMRG to 2D via projected entangled pairs with area-law entanglement
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|Knill-Laflamme (1998)]] — DQC1: one clean qubit + $n$ maximally mixed qubits can estimate normalised traces; intermediate complexity class between BPP and BQP
- [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes|KLM (2001)]] — universal QC from linear optics + single-photon sources + photon detection; measurement-induced nonlinearity + gate teleportation; no optical nonlinearity required
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes|Jozsa-Linden (2003)]] — bounded multi-partite entanglement implies efficient classical simulation; exponential quantum speedup requires $\omega(\log n)$-partite entanglement; mixed-state case remains open
- [[Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002) — Paper Notes|Osborne-Nielsen (2002)]] — entanglement (von Neumann entropy, concurrence) peaks at quantum critical point in transverse Ising model; establishes entanglement-criticality connection
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes|Verstraete-Cirac-Latorre (2009)]] — exact diagonalising circuits for free-fermion Hamiltonians (XY model, Ising chain, Kitaev honeycomb); $O(n^2)$ gates, $O(n \log n)$ depth; time evolution cost independent of $t$
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes|Bravyi-Hastings-Verstraete (2006)]] — effective light cone for local Hamiltonians; finite speed for correlation generation; $\Omega(L)$ lower bound for creating topological order; entanglement area law in time
- [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes|Ambainis-Buhrman-Høyer-Karpinski-Kurur (2002)]] — $O(n^{7/4})$ quantum matrix product verification via recursive Grover + Freivalds; first quantum speedup for linear algebra verification
- [[Quantum Computations with Cold Trapped Ions (Cirac-Zoller 1995) — Paper Notes|Cirac-Zoller (1995)]] — original proposal for ion-trap quantum computing; internal electronic states as qubits, shared phonon mode as quantum bus for two-qubit gates; blueprint for all ion-trap experiments

## Communication Complexity

- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes|Buhrman-Cleve-Wigderson (1998)]] — first quantum-classical communication separations: near-quadratic for DISJ (bounded-error), exponential for EQ' (exact vs. zero-error); introduces black-box to communication reduction
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes|Buhrman-Cleve-Watrous-de Wolf (2001)]] — exponential quantum communication advantage for equality in SMP via quantum fingerprints; $O(\log n)$ vs. $\Theta(\sqrt{n})$ qubits
- [[Exponential Separation of Quantum and Classical Communication Complexity (Raz 1999) — Paper Notes|Raz (1999)]] — first unconditional exponential separation for a partial function in the standard bounded-error model; $O(\log n)$ qubits vs. $\Omega(n^{1/4})$ classical bits; distributional method for classical lower bounds

## Nonlocal Games / Bell Inequalities / MIP*

- [[Consequences and Limits of Nonlocal Strategies (Cleve-Hoyer-Toner-Watrous 2004) — Paper Notes|Cleve-Høyer-Toner-Watrous (2004)]] — formalises nonlocal games as computational framework; CHSH, Odd Cycle, Magic Square, Kochen-Specker examples; shows entangled provers break graph colouring and 3-SAT proof systems; Tsirelson/Grothendieck bounds on XOR game quantum values; introduces MIP vs. MIP* question

---

## Hamiltonian Simulation

### Product formulas (Trotter / Suzuki)
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — foundational product-formula simulation of local Hamiltonians; proves Feynman's conjecture
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes|Wiebe-Berry-Høyer-Sanders (2010)]] — higher-order Suzuki [[Product Formulas]]s for time-dependent Hamiltonians; explicit bounds without analyticity; smoothness-order saturation
- [[Simulating Quantum Dynamics on a Quantum Computer (Wiebe-Berry-Høyer-Sanders 2011) — Paper Notes|Wiebe-Berry-Høyer-Sanders (2011)]] — end-to-end quantum algorithm for time-dependent sparse simulation; constant- and adaptive-step variants; explicit oracle precision requirements; discontinuity handling
- [[Quantum-Circuit Design for Efficient Simulations of Many-Body Quantum Dynamics (Raeisi-Wiebe-Sanders 2012) — Paper Notes|Raeisi-Wiebe-Sanders (2012)]] — autonomous gate-level circuit synthesis for $k$-local Hamiltonians; Pauli-string primitives via parity-CNOT ladder; commuting-group parallelisation; $O(n^{2+o(1)})$ gates for physically local models
- [[Quantum Simulation of Time-Dependent Hamiltonians and the Convenient Illusion of Hilbert Space (Poulin-Qarry-Somma-Verstraete 2011) — Paper Notes|Poulin-Qarry-Somma-Verstraete (2011)]] — derivative-free Trotter for arbitrary time-dependent Hamiltonians; quantum Shannon counting argument; randomised [[Product Formulas]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — first efficient sparse [[Hamiltonian simulation]]
- [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes|Childs-Wiebe (2013)]] — recursive product formulas for commutator exponentials; nearly-optimal via quantum search lower bound
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes|Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe (2015)]] — Trotter error scales with $Z_{\max}^6$ not $N$; norm bounds loose by 10¹⁶; classical error subtraction
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher, Wiebe, Svore, Wecker, Troyer (2017)]] — The FeMoCo benchmark paper: first complete fault-tolerant resource estimate for a scientifically important molecule (nitrogenase FeMoCo, 108–114 spin-orbitals); 2nd-order Trotter + QPE + surface code compilation; ~$10^{14}$ T gates, 111 logical qubits; subsequently improved by ~$300{,}000\times$ via qubitization and tensor factorization
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — tight commutator-based Trotter error bounds
- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes|Su-Huang-Campbell (2021)]] — nearly tight Trotter bounds for fermionic Hamiltonians via fermionic seminorm; $(n^{5/3}/\eta^{2/3} + n^{4/3}\eta^{2/3})n^{o(1)}$ gates for plane-wave electronic structure
- [[Randomized Product Formulas for Hamiltonian Simulation (Quantum 2019-09-02-182) — Paper Notes|Childs-Su-Tran-Wiebe-Zhu (2019)]] — randomized product-formula ordering; permutation averaging improves error bounds
- [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes|Low-Kliuchnikov-Wiebe (2019)]] — well-conditioned multiproduct formulas via Chebyshev nodes; $O(t\lambda\log^2(t\lambda/\epsilon))$ simulation combining commutativity exploitation with near-optimal time/error scaling
- [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes|Faehrmann-Steudtner-Kueng-Kieferová-Eisert (2022)]] — randomized sampling of multi-product formulas; trades circuit depth for shots
- [[Doubling the Order of Approximation via the Randomized Product Formula (Cho-Berry-Hsieh 2022) — Paper Notes|Cho-Berry-Hsieh (2022)]] — randomized corrections double the error order of symmetric product formulas from $2k+1$ to $4k+2$; requires Pauli-string structure
- [[Faster Digital Quantum Simulation by Symmetry Protection (Tran-Su-Childs-Wiebe 2021) — Paper Notes|Tran-Su-Childs-Wiebe (2021)]] — symmetry-protected Trotter error cancellation
- [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes|Recursive Trotterization (2022)]] — L1-norm reduction and low-rank decomposition
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes|Campbell (2018)]] — qDRIFT: random channel simulation with L1 norm scaling
- [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes|Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry (2025)]] — new 8th-order product formulas ~100–300× more accurate than prior work; eigenvalue error metric; cross-order threshold analysis shows 8th order optimal for $T/\varepsilon \sim 10^7$–$10^{16}$; 10th order irrelevant for quantum computing
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab-Berry-Aspuru-Guzik et al. (2024)]] — corrected product formulas: inject commutator correctors at boundaries to gain factor-$\alpha$ error improvement for perturbed systems; two-order improvement for non-perturbed; ancilla-free
- [[Randomly Compiled Quantum Simulation with Exponentially Reduced Circuit Depths (Watson 2025) — Paper Notes|Watson (2025)]] — qFLO: Richardson extrapolation over qDRIFT step sizes; exponential depth improvement for observable estimation
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes|Martyn-Rall (2025)]] — Stochastic QSP: randomizes at the polynomial/QSP level rather than Hamiltonian term level; halves $\log(1/\epsilon)$ query cost across all QSP-family algorithms
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes|An-Costa-Berry (2025)]] — for near-adiabatic dynamics, product formulas can use $\varepsilon$-independent step sizes; discrete adiabatic theorem as analysis tool; first-order Trotter + boundary cancellation → super-polynomial convergence

### Lattice / geometrically local simulation
- [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes|Bravyi-Hastings-Verstraete (2006)]] — foundational: Lieb-Robinson bounds constrain information, correlation, and entanglement propagation speeds; TQO creation requires $\Omega(L)$ time; area law for entropy generation in time
- [[Quantum Algorithm for Simulating Real Time Evolution of Lattice Hamiltonians (Haah-Hastings-Kothari-Low 2021) — Paper Notes|Haah-Hastings-Kothari-Low (2021)]] — optimal $O(nT\operatorname{polylog}(nT/\epsilon))$ gate count for geometrically local lattice Hamiltonians via Lieb-Robinson decomposition; matching $\tilde{\Omega}(nT)$ gate lower bound; commutator-sensitive Lieb-Robinson velocity

### Taylor series / LCU methods
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|Childs-Wiebe (2012)]] — linear combination of unitaries, the foundational technique
- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes|Berry-Cleve-Gharibian (2012)]] — gate-efficient conversion of continuous-time query algorithms to discrete circuits; compresses CGMSY control qubits to polylog space; $\tilde{O}(T)$ gates and polylog$(\|H\|T)$ ancillae
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes|Berry-Cleve-Somma (2013)]] — first polylog$(1/\varepsilon)$ simulation via compressed fractional-query Trotter; broke the precision barrier
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2014)]] — full STOC version; introduces OAA, proves optimal $\varepsilon$-lower bound
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry-Childs-Cleve-Kothari-Somma (2015)]] — truncated Taylor series + LCU + robust OAA; near-optimal $\log(1/\varepsilon)$ precision scaling
- [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes|Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik (2015)]] — first application of truncated Taylor series to chemistry; on-the-fly integral computation reduces cost to $\widetilde{O}(N^5 t)$
- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes|Berry-Childs-Kothari (2015)]] — Bessel-LCU of walk steps; optimal in both sparsity and precision
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry-Childs (2011)]] — black-box simulation via sparse oracles
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — Fourier and Chebyshev LCU for matrix inversion
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (2023)]] — improved LCHS with generalised Hardy-space kernels; polylog precision + optimal state-prep for linear ODEs
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|An-Childs-Lin (2024)]] — Lap-LCHS: extends LCHS to general eigenvalue transforms via Laplace representation; LCU of [[Hamiltonian simulation]] unitaries

### Time-dependent / unbounded
- [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes|An-Fang-Liu (2021)]] — vector-norm Trotter bounds for unbounded time-dependent Hamiltonians; cost independent of $\|H\|$ for smooth initial states

### Dyson series / interaction picture
- [[Time-Dependent Hamiltonian Simulation via Dyson Series (Kieferová-Scherer-Berry 2018) — Paper Notes|Kieferová-Scherer-Berry (2018)]] — truncated Dyson series for time-dependent Hamiltonians
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes|Low-Wiebe (2018)]] — interaction-picture simulation with norm reduction
- [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes|Berry-Childs-Su-Wang-Wiebe (2020)]] — L1-norm scaling via time reparameterization and continuous qDRIFT
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes|Berry-Costa (2022)]] — Dyson block-encoding for time-dependent ODEs (not just Hamiltonians); optimal QLSA solver; derivative-free oracle count
- [[qHOP — Time-Dependent Simulation of Highly Oscillatory Dynamics (An-Fang-Lin 2022) — Paper Notes|An-Fang-Lin (2022)]] — qHOP: first-order Magnus + LCU quadrature; commutator scaling without time-ordering; superconvergence for Schrödinger ($\log N$ dependence)

### Qubitization / QSP / QSVT
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|Low-Chuang (2016–2017)]] — optimal simulation via quantum signal processing
- [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes|Low-Chuang (2017)]] — uniform spectral amplification; flexible QSP and amplitude multiplication; $O(t\sqrt{d\|H\|_{\max}\|H\|_1})$ sparse simulation
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — qubitization iterate and block-encoding framework
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén-Su-Low-Wiebe (2019)]] — quantum singular value transformation, unifying framework
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low-Yoder-Chuang (2016)]] — composite pulse sequences and QSP precursors
- [[Shadow Hamiltonian Simulation — Paper Notes|Shadow Hamiltonian Simulation]] — simulation via shadow tomography techniques
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes|Sublinear-T Block-Encodings (2024)]] — resource-efficient block-encodings for chemistry
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes|Martyn-Liu-Chin-Chuang (2023)]] — fully-coherent QSP/QET simulation; one-shot algorithm bypasses parity obstruction via affine spectrum compression, achieving $\Theta(\alpha|t| + \log(1/\epsilon) + \log(1/\delta))$ with no amplitude amplification
- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes|Martyn-Rall (2025)]] — Stochastic QSP; halves query complexity of all QSP-based algorithms via randomized compiling + Hastings-Campbell mixing lemma; output is a channel (not a unitary)
- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes|Motlagh-Wiebe (2023)]] — GQSP: SU(2) signal processing operators lift all parity and realness constraints on QSP polynomials; any $|P| \leq 1$ polynomial achievable; simple recursive phase extraction; optimal fractional query $O(1/\delta + \log(1/\epsilon))$
- [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes|Berry-Motlagh-Pantaleoni-Wiebe (2024)]] — factor-of-2 query reduction for QSP simulation via directional walk control ($U$ vs $U^\dagger$) and GQSP; constant-factor improvement on the dominant cost
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|Dong-Meng-Whaley-Lin (2021)]] — QSPPACK: optimisation-based QSP phase-factor computation in standard double precision for $d > 10{,}000$; perturbative initial guess + L-BFGS + scaling-and-squaring
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes|Wang-Dong-Lin (2022)]] — energy landscape analysis of QSP phase-factor optimisation; proves local strong convexity near fixed initial guess for $\|f\|_\infty = O(d^{-1})$; first provably convergent double-precision algorithm

### Specialised / structured Hamiltonians
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes|PDE Hamiltonian Simulation (2021)]] — QFT/QSFT/QCT for structured PDE Hamiltonians
- [[Exponential Quantum Speedup for Coupled Classical Oscillators (Quantum 2020-04-20-254 follow-on) — Paper Notes|Coupled Classical Oscillators (2020)]] — exponential speedup via [[Hamiltonian simulation]]
- [[Limitations on Simulation of Non-Sparse Hamiltonians (Childs-Kothari-Somma 2010) — Paper Notes|Childs-Kothari-Somma (2010)]] — lower bounds for non-sparse simulation

### Quantum field theory
- [[Quantum Algorithms for Quantum Field Theories (Jordan-Lee-Preskill 2012) — Paper Notes|Jordan-Lee-Preskill (2012)]] — first algorithm for scattering in $\phi^4$ QFT; polynomial in particles, energy, precision; EFT discretisation analysis
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes|Jordan-Krovi-Lee-Preskill (2018)]] — vacuum-to-vacuum amplitude in $(1+1)$D $\phi^4$ with classical sources is BQP-complete; qubits as double-well potentials, gates via adiabatic parameter variation; even weakly-coupled QFT is classically intractable

### Fermionic systems
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams-Lloyd (1997)]] — first fermionic simulation algorithms; antisymmetrization, Hubbard model
- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes|Ortiz-Gubernatis-Knill-Laflamme (2001)]] — early fermionic simulation framework
- [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes|Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik (2015)]] — first truncated Taylor series for chemistry; database $\widetilde{O}(N^8 t)$ and on-the-fly $\widetilde{O}(N^5 t)$ algorithms with $\log(1/\varepsilon)$ precision
- [[Fermionic Eigenstate Prep Techniques (Nature 2018) — Paper Notes|Fermionic Eigenstate Prep (2018)]] — improved eigenstate preparation for fermionic systems
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — first scalable hardware quantum chemistry; VQE + Bravyi-Kitaev on 2-qubit H₂; chemical accuracy achieved; PEA comparison
- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes|Su-Huang-Campbell (2021)]] — nearly tight Trotter bounds for electronic Hamiltonians; fermionic seminorm + commutator scaling + sparse path counting; $(n^{5/3}/\eta^{2/3} + n^{4/3}\eta^{2/3})n^{o(1)}$ for plane-wave electronic structure

---

## Quantum Signal Processing and Block Encodings

- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — QSVT: polynomial transformations of singular values
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes|Martyn-Rossi-Tan-Chuang (2021)]] — pedagogical tutorial: QSP → QET → QSVT unification; constructs search, QPE, simulation, matrix inversion as QSVT instances
- [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes|Quantum Eigenvalue Processing for Non-Normal Matrices (2024)]] — QSVT extension to non-normal matrices
- [[SOSSA — Sum-of-Squares Spectral Amplification.md|SOSSA — Sum-of-Squares Spectral Amplification]] — SOS-based spectral amplification
- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes|King, Low, Babbush, Somma, Rubin (2025)]] — SOSSA framework: SOS decomposition + spectral amplification for low-energy simulation; query complexity $O(\sqrt{\Delta\lambda}/\varepsilon)$; adaptive algorithms with no prior on $\Delta$; $\sqrt{N}$ speedup for SYK; query-optimal lower bounds
- [[Quantum Hermite Transform — Paper Notes|Quantum Hermite Transform]] — Hermite polynomial quantum transform

---

## [[Amplitude Amplification and Estimation]]

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — original $O(\sqrt{N})$ search
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|BBHT (1998)]] — closed-form success probability, multiple solutions, unknown-$t$ randomised schedule, tight lower bound
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|Brassard-Høyer-Tapp (1998)]] — quantum counting = phase estimation on Grover operator; heuristic quadratic speedup; amplitude estimation
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr-Høyer (1996)]] — minimum finding in $O(\sqrt{N})$ via Grover with evolving threshold
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification and estimation, general framework
- [[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes|Yoder-Low-Chuang (2014)]] — first fixed-point amplitude amplification with optimal $O(1/\sqrt{\lambda})$ queries; Chebyshev polynomial design seeded the entire QSP programme
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|Brassard-Høyer-Tapp (1997)]] — collision finding via Grover
- [[Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) — Paper Notes|Wocjan-Chiang-Abeyesinghe-Nagaj (2009)]] — quantum FPRAS for partition functions via quantum walks + phase estimation; $\tilde{O}(\ell^2/(\sqrt{\delta}\varepsilon))$ cost; predates Montanaro's general framework
- [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — near-quadratic speedup for Monte Carlo mean estimation; partition function applications via quantum walks
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes|Rebentrost-Gupt-Bromley (2018)]] — quantum amplitude estimation applied to European and Asian option pricing; quadratic speedup over classical MC

---

## Quantum Walks

- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes|Childs-Cleve-Deotto-Farhi-Gutmann-Spielman (2003)]] — first exponential speedup via quantum walks (not QFT); continuous-time walk on glued trees
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — quantum walk algorithmic survey and framework
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|Ambainis-Childs-Reichardt-Špalek-Zhang (2007)]] — optimal $O(\sqrt{N})$ quantum evaluation of AND-OR formulas via coined walk
- [[Quantum Algorithms for Element Distinctness (Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf 2001) — Paper Notes|Buhrman-Dürr-Heiligman-Høyer-Magniez-Santha-de Wolf (2001)]] — first quantum speedup for element distinctness: $O(N^{3/4} \log N)$ via nested amplitude amplification + classical sorting
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — element distinctness via quantum walk
- [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes|Magniez-Santha-Szegedy (2007)]] — triangle finding in $\tilde{O}(n^{13/10})$ via recursive quantum walk; combinatorial algorithm in $\tilde{O}(n^{10/7})$
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy (2004)]] — quadratic speedup for Markov chain algorithms
- [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes|Childs-Cleve-Jordan-Yonge-Mallo (2009)]] — converts FGG continuous-time NAND tree algorithm to $O(N^{1/2+\varepsilon})$ discrete queries via product formula simulation; pins quantum query complexity to $\Theta(N^{1/2+o(1)})$
- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes|Berry-Cleve-Gharibian (2012)]] — makes continuous-time quantum walk algorithms (e.g., AND-OR tree evaluation) gate-efficient; $N^{1/2+o(1)}$ gates for NAND tree
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|Magniez-Nayak-Roland-Santha (2007)]] — unified quantum walk search; combines Ambainis and Szegedy frameworks with separated $S + \frac{1}{\sqrt{\varepsilon}}(\frac{1}{\sqrt{\delta}} U + C)$ cost
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs-Goldstone (2004)]] — continuous-time spatial search
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes|Childs (2010)]] — equivalence of continuous and discrete quantum walks
- [[Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) — Paper Notes|Knill-Ortiz-Somma (2007)]] — quantum walk simulation of classical annealing
- [[Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016) — Paper Notes|Krovi-Magniez-Ozols-Roland (2016)]] — resolves open question: quantum walk search achieves $O(\sqrt{\mathrm{HT}})$ for finding a marked vertex on any reversible Markov chain; interpolated walk + eigenvalue estimation
- [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes|Montanaro (2015)]] — near-quadratic speedup for backtracking algorithms via quantum walk on implicitly-defined trees; $O(\sqrt{Tn})$ detection
- [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes|Campbell-Khurana-Montanaro (2019)]] — concrete resource estimates for quantum backtracking and Grover applied to random $k$-SAT and graph colouring; speedup factors $10^3$–$10^5$ in optimistic regime
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes|Ambainis-Childs-Liu (2011)]] — quantum graph property testing via derandomization + element distinctness; $\tilde{O}(N^{1/3})$ for bipartiteness and expansion

---

## Linear Systems and Differential Equations

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow-Hassidim-Lloyd (2009)]] — HHL: exponential speedup for linear systems
- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes|Wiebe-Braun-Lloyd (2012)]] — extends HHL to least-squares fitting via isometry embedding; fit-quality estimation via swap test; sparse classical readout via quantum-aided compressed sensing
- [[Quantum Algorithms and the Finite Element Method (Montanaro-Pallister 2016) — Paper Notes|Montanaro-Pallister (2016)]] — honest end-to-end complexity analysis for applying QLSA/HHL to FEM; discretisation-accuracy tradeoff kills generic exponential claims, leaving polynomial speedups in fixed dimension and better prospects in high-dimensional PDEs
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes|Tong-An-Wiebe-Lin (2021)]] — preconditioned QLSP via fast inversion; Green's function computation; Gibbs state preparation via contour integral and inverse-transform approaches
- [[High-Order Quantum Algorithm for Solving Linear Differential Equations (Berry 2014) — Paper Notes|Berry (2014)]] — first quantum algorithm for general linear ODEs; history-state encoding + HHL, $\tilde{O}(\Delta t^2)$ scaling
- [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes|Childs-Liu (2020)]] — Chebyshev spectral methods for quantum ODE solving; $\mathrm{polylog}(1/\varepsilon)$ for time-dependent linear ODEs; also handles boundary value problems
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang (2017)]] — linear ODEs via quantum simulation
- [[Quantum Linear Matrix Equations — Paper Notes|Quantum Linear Matrix Equations]] — matrix equation generalisation
- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs-Kothari-Somma (2015)]] — improved linear system solvers
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin (2019)]] — near-optimal QLSA via time-optimal adiabatic scheduling; AQC(p) achieves $O(\kappa/\varepsilon)$, AQC(exp) gets polylog precision; QAOA connection
- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|Dong-An-Lin (2020)]] — eigenstate filtering via QSP + Chebyshev minimax; QLSP in $\tilde{O}(d\kappa\log(1/\epsilon))$ via AQC or Zeno, no phase estimation or amplitude amplification
- [[Quantum Algorithm for Time-Dependent Differential Equations Using Dyson Series (Berry-Costa 2022) — Paper Notes|Berry-Costa (2022)]] — time-dependent inhomogeneous linear ODEs via Dyson series block-encoding + optimal QLSA; $\tilde{O}(\lambda_A T \log(1/\varepsilon))$ oracle calls; derivative-independent query count; improves log factors over Childs-Liu spectral method
- [[Time-Marching Quantum Solvers for Linear ODEs (Fang-Lin-Tong 2023) — Paper Notes|Fang-Lin-Tong (2023)]] — time-marching ODE solver via uniform singular value amplification + compression gadget; linear dependence on amplification ratio $Q$ (proven tight); handles non-diagonalisable, non-smooth $A(t)$
- [[Quantum Algorithm for Linear Non-Unitary Dynamics with Near-Optimal Dependence on All Parameters (An-Childs-Lin 2023) — Paper Notes|An-Childs-Lin (2023)]] — improved LCHS: generalised kernel identity + Hardy-space kernel gives polylog precision scaling with optimal state-prep cost; first quantum ODE solver near-optimal on all parameters simultaneously
- [[Quantum Algorithm for General Eigenvalue Transforms via the Laplace Transform (An-Childs-Lin 2024) — Paper Notes|An-Childs-Lin (2024)]] — Lap-LCHS: general eigenvalue transforms $h(A)$ via Laplace representation + LCHS; covers fractional powers, $e^{-TA^{-1}}$, inhomogeneous ODEs; optimal state-preparation cost; requires dissipative $A$
- [[Efficient Quantum Algorithm for Dissipative Nonlinear Differential Equations (Liu-Kolden-Krovi-Loureiro-Trivisa-Childs 2021) — Paper Notes|Liu-Kolden-Krovi et al. (2021)]] — THE Carleman linearisation paper: embeds quadratic nonlinear ODEs into infinite-dimensional linear systems, solves with QLSA; $T^2 q \cdot \mathrm{poly}(\log)/\varepsilon$ complexity for $R < 1$; lower bound $\Omega(2^T)$ for $R \ge \sqrt{2}$
- [[Improved Quantum Algorithms for Linear and Nonlinear Differential Equations (Krovi 2023) — Paper Notes|Krovi (2023)]] — improves Liu et al. (2021): characterises ODE complexity via $C(A) = \sup_t \|e^{At}\|$; handles non-normal/non-diagonalisable matrices; exponentially better $\varepsilon$-dependence for nonlinear ODEs via truncated Taylor series; log-norm condition
- [[Further Improving Quantum Algorithms for Nonlinear DEs via Higher-Order Methods and Rescaling (Costa-Schleich-Morales-Berry 2023) — Paper Notes|Costa-Schleich-Morales-Berry (2023)]] — nonlinear ODEs/PDEs via Carleman linearisation with rescaling, higher-order time/space discretisation; eliminates exponential-in-$N$ amplitude penalty; $\log(1/\varepsilon)$ error scaling, near-linear $T$
- [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes|Leyton-Osborne (2008)]] — first quantum algorithm for nonlinear ODEs; nondeterministic Euler method on amplitudes via tensor products + postselection; poly($\log n$) but exponential in $t/h$; historical precursor to Carleman approach
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes|Arrazola-Kalajdzievski-Weedbrook-Lloyd (2019)]] — continuous-variable version of matrix inversion for inhomogeneous linear PDEs; Fourier decomposition of $A^{-1}$ + CV Hamiltonian simulation; exact gate decompositions for polynomial CV Hamiltonians
- [[Quantum vs. Classical Algorithms for Solving the Heat Equation (Linden-Montanaro-Shao 2022) — Paper Notes|Linden-Montanaro-Shao (2022)]] — systematic comparison of 10 classical/quantum algorithms for the heat equation; finds at most quadratic quantum speedup; QLSA-based approaches never competitive; best quantum method is amplitude estimation on random walks
- [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes|An-Liu-Lin (2023)]] — LCHS: express non-unitary propagator as weighted integral over Hamiltonian simulations via Cauchy kernel; optimal state-preparation cost $O(q)$; hybrid and coherent implementations; near-optimal for open quantum dynamics with complex absorbing potentials

---

## Eigenvalue / Ground State Problems

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — phase estimation
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd (1999)]] — quantum phase estimation for eigenvalues
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|Aspuru-Guzik et al. (2005)]] — first quantum chemistry calculations (H₂O, LiH); recursive PEA, adiabatic state prep
- [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes|Lanyon et al. (2010)]] — first experimental quantum chemistry calculation; photonic IPEA on H$_2$ to 20-bit precision
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes|Kassal et al. (2008)]] — full chemical dynamics beyond Born-Oppenheimer; split-operator real-space simulation in $O(B^2 m^2)$
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes|Temme et al. (2011)]] — quantum Metropolis for Gibbs state preparation; sign-problem-free sampling from eigenbasis
- [[Fast Inversion, Preconditioned QLSP, Green's Functions, and Matrix Functions (Tong-An-Wiebe-Lin 2021) — Paper Notes|Tong-An-Wiebe-Lin (2021)]] — Gibbs state preparation via preconditioned contour integral and inverse-transform approaches
- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|Dong-An-Lin (2020)]] — optimal eigenstate filtering via QSP + Chebyshev minimax polynomials; applied to QLSP via AQC and Zeno routes; $\tilde{O}(d\kappa\log(1/\epsilon))$ without phase estimation or amplitude amplification
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin-Tong (2020)]] — near-optimal ground state preparation via signal processing
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin-Tong (2022)]] — Heisenberg-limited ground-state energy estimation with 1 ancilla qubit; early fault-tolerant regime; spectral CDF approach
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|Dong-Lin-Tong (2022)]] — QET-U: eigenvalue transformation via Hamiltonian evolution; ground-state energy $\tilde{O}(\varepsilon^{-1}\gamma^{-2})$ with 1 ancilla (short depth) or $\tilde{O}(\varepsilon^{-1}\gamma^{-1})$ with 3 ancillas (near-optimal); control-free variant for spin models
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes|Ding-Chen-Lin (2023)]] — quantum MCMC via Lindbladian with single ancilla qubit; no initial overlap needed; $\tilde{O}(T^{1+1/p}\epsilon^{-1/p})$ total [[Hamiltonian simulation]] time; mixing time is the key hardness parameter
- [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes|Anshu (2026)]] — heuristic alternating-minimisation method; escapes eigenstate stalling by switching between altered Hamiltonians that share the ground space; energy-dependent uncertainty principle (Theorems 2.1, 2.3); no convergence guarantee; promising numerics on 8–12 qubit systems
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo-McClean et al. (2014)]] — VQE: variational quantum eigensolver
- [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes|Epperly-Lin-Nakatsukasa (2022)]] — rigorous analysis of QSD with Löwdin thresholding; explains why noisy generalized eigenvalue problems from Krylov subspaces work; exponential Krylov convergence via trigonometric minimax polynomials
- [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes|Ding-Li-Lin-Ni-Ying-Zhang (2024)]] — QMEGS: first algorithm achieving Heisenberg-limited multiple eigenvalue estimation with reduced circuit depth for gapped systems; unifies gapped and gapless regimes via Gaussian-filtered Hadamard tests

---

## Quantum Machine Learning

- [[Quantum Principal Component Analysis (Lloyd-Mohseni-Rebentrost 2014) — Paper Notes|Lloyd-Mohseni-Rebentrost (2014)]] — quantum PCA

## Quantum Learning / Tomography

- [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes|Wiebe-Braun-Lloyd (2012)]] — quantum least-squares fitting; foundational quantum ML paper; exponential speedup in $N$ under strict conditions (sparse $F$, efficient state prep, sparse solution)
- [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes|Aaronson (2006)]] — PAC learning quantum states
- [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes|Cramer-Plenio et al. (2010)]] — MPS tomography: $O(N)$ local measurements + polynomial classical processing; two schemes (sequential disentanglement, MPS-SVT); rigorous fidelity certification via parent Hamiltonian witness
- [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes|Krovi (2019)]] — quantum Schur transform in $O(\mathrm{poly}(n, \log d, \log(1/\varepsilon)))$ via symmetric group permutation modules; exponential improvement over BCH in local dimension; primitive for spectrum estimation and tomography

## Property Testing / Distribution Testing

- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes|Ambainis-Childs-Liu (2011)]] — $\tilde{O}(N^{1/3})$ quantum testing of bipartiteness and expansion for bounded-degree graphs; derandomize classical testers + element distinctness; $\tilde{\Omega}(N^{1/4})$ lower bound for expansion
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes|Bravyi-Harrow-Hassidim (2011)]] — quantum property testing of distributions; $O(N^{1/2})$ for statistical difference, $\Theta(N^{1/3})$ for uniformity and orthogonality; all via amplitude estimation

## Hamiltonian Learning and Certification

- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe-Granade-Ferrie-Cory (2014, PRL)]] — Bayesian Hamiltonian learning via IQLE + PGH + SMC; $O(d \log 1/\delta)$ experiments; certifies unknown quantum simulators; noiseless analysis
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|Wiebe-Granade-Ferrie-Cory (2014, PRA)]] — sequel: robustness of QHL to depolarizing noise (rate degrades by $(1-\mathcal{N})$, not catastrophically); noisy swap gates via superoperator modeling; model mismatch saturates at residual scale; free Bayesian model selection via SMC marginal likelihoods
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes|Aaronson (2018)]] — shadow tomography: $\tilde{O}(\log^4 M \cdot \log D / \varepsilon^4)$ copies to estimate $M$ observables on an unknown $D$-dimensional state; polylog in both $M$ and $D$
- [[Robust Online Hamiltonian Learning (Granade-Ferrie-Wiebe-Cory 2012) — Paper Notes|Granade-Ferrie-Wiebe-Cory (2012)]] — 🚧 STUB — robust online Bayesian Hamiltonian learning; predecessor to QHL
- [[Quantum Bootstrapping via Compressed Quantum Hamiltonian Learning (Wiebe-Granade-Ferrie-Cory 2015) — Paper Notes|Wiebe-Granade-Ferrie-Cory (2015)]] — 🚧 STUB — compressed QHL for bootstrapping quantum simulators
- [[Experimental Quantum Hamiltonian Learning (Wang, Paesani, Santagati et al 2017) — Paper Notes|Wang, Paesani, Santagati et al. (2017)]] — First experimental demonstration of QHL; silicon-photonics simulator learns NV$^-$ Rabi frequency via QLE and IQLE; model deficiency detection via variance saturation; Bayes factor model comparison

---

## Variational and Near-Term Algorithms

- [[A Quantum Approximate Optimization Algorithm (Farhi-Goldstone-Gutmann 2014) — Paper Notes|Farhi-Goldstone-Gutmann (2014)]] — QAOA for combinatorial optimisation
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|Peruzzo-McClean et al. (2014)]] — VQE
- [[The Theory of Variational Hybrid Quantum-Classical Algorithms (McClean-Romero-Babbush-Aspuru-Guzik 2015) — Paper Notes|McClean-Romero-Babbush-Aspuru-Guzik (2015)]] — VQE theory: variational adiabatic paths, generalized UCC, variational error suppression, measurement cost reduction, derivative-free optimization
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — first scalable hardware VQE experiment; Bravyi-Kitaev encoding; chemical accuracy on H₂; experimental confirmation of variational error suppression
- [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes|Anshu (2026)]] — alternating variational minimisation over a family of altered Hamiltonians; addresses eigenstate stalling; complements VQE with a Hamiltonian-switching escape heuristic
- [[Hardware-Efficient Variational Quantum Eigensolver for Small Molecules and Quantum Magnets (Kandala, Mezzacapo, Temme, Takita, Brink, Chow, Gambetta 2017) — Paper Notes|Kandala et al. (2017)]] — hardware-efficient VQE on IBM superconducting qubits; H₂, LiH, BeH₂ and Heisenberg magnets; demonstrates entangler blocks adapted to hardware connectivity rather than chemical structure
- [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes|Wecker-Hastings-Troyer (2015)]] — resource analysis for VQE with adiabatic and Hamiltonian variational ansätze; measurement scaling studies; practical bottleneck analysis for NISQ chemistry
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes|Wecker-Hastings-Wiebe-Clark-Nayak-Troyer (2015)]] — complete fault-tolerant pipeline for 2D Hubbard: adiabatic state prep from mean-field/plaquette states, $O(N)$-gate $O(\log N)$-depth Trotter circuits, QPE with $4\times$ fewer rotations, static/dynamic correlation measurements, non-destructive measurement with quadratic speedup
- [[Entanglement-Induced Barren Plateaus (Ortiz Marrero-Kieferová-Wiebe 2021) — Paper Notes|Ortiz Marrero-Kieferová-Wiebe (2021)]] — 🚧 STUB — entanglement across subsystem boundary causes barren plateaus in variational algorithms regardless of circuit structure

---

## Adiabatic Quantum Computation

- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes|Farhi-Goldstone-Gutmann-Sipser (2000)]] — adiabatic quantum computation proposal
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes|van Dam-Mosca-Vazirani (2001)]] — AQC matches Grover via local adiabatic schedule; $O(n^3)$ counting queries determine 3SAT; exponential lower bound for perturbed Hamming weight
- [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|Roland-Cerf (2002)]] — local adiabatic schedule recovers $O(\sqrt{N})$ for search; proves optimality
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes|Aharonov et al. (2004)]] — adiabatic ↔ circuit equivalence
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov-Ta-Shma (2003)]] — adiabatic state generation and SZK
- [[Spectral Gap Amplification (Somma-Boixo 2013) — Paper Notes|Somma-Boixo (2013)]] — quadratic gap amplification for frustration-free Hamiltonians; optimal; impossible for general $H$
- [[Quantum Linear System Solver via Time-Optimal AQC and QAOA (An-Lin 2019) — Paper Notes|An-Lin (2019)]] — time-optimal adiabatic schedules for QLSP; generalises Roland-Cerf local adiabatic method
- [[Improved Error Bounds for the Adiabatic Approximation (Cheung-Høyer-Wiebe 2011) — Paper Notes|Cheung-Høyer-Wiebe (2011)]] — rigorous upper/lower bounds on adiabatic error; asymptotically tight; resolves Marzlin–Sanders counterexample
- [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes|Kieferová-Wiebe (2014)]] — coherent averaging of two adiabatic paths via LCU gadget; cancels $O(1/T)$ diabatic error leaving $O(1/T^2)$; symmetric-spectrum case cancels all transitions simultaneously; polynomial equivalence to circuit model
- [[Large Time-Step Discretisation of Adiabatic Quantum Dynamics (An-Costa-Berry 2025) — Paper Notes|An-Costa-Berry (2025)]] — time step for digitally simulated AQC can be independent of $\varepsilon$ and $T$; discrete adiabatic view of numerical integrators; boundary cancellation gives super-polynomial convergence for first-order Trotter; Trotterised Grover matches $O(\sqrt{N})$; shows Trotter can open gaps in gapless Hamiltonians
- [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes|Altshuler-Krovi-Roland (2010)]] — Anderson localisation on the hypercube gives super-exponentially small gaps $\exp(-\Omega(N \ln N))$ for random EC3 near satisfiability threshold; typical-case failure of standard AQO

---

## Complexity Theory (QMA, BQP)

- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes|Kitaev (1999)]] — local Hamiltonian is QMA-complete
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes|Watrous (2000)]] — group non-membership in QMA; first natural problem separating QMA from MA relative to an oracle; quantum proof is uniform superposition over subgroup
- [[3-Local Hamiltonian is QMA-Complete (Kempe-Regev 2003) — Paper Notes|Kempe-Regev (2003)]] — 3-local Hamiltonian QMA-completeness
- [[2-Local Hamiltonian is QMA-Complete (Kempe-Kitaev-Regev 2006) — Paper Notes|Kempe-Kitaev-Regev (2006)]] — 2-local Hamiltonian QMA-completeness
- [[QMA-Completeness of N-Representability (Liu-Christandl-Verstraete 2007) — Paper Notes|Liu-Christandl-Verstraete (2007)]] — N-representability is QMA-complete
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — quantum algorithms for group-theoretic problems
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes|Marriott-Watrous (2005)]] — QMA amplification without extra witness qubits via Jordan's lemma; QMA ⊆ PP; QIP(1) = QMA; QMAM = QIP
- [[PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) — Paper Notes|Watrous (1999/2003)]] — $\mathrm{PSPACE} \subseteq \mathrm{QIP}(2)$; quantum transcript compression collapses multi-round classical proofs to 2 quantum messages; the precursor to QIP = PSPACE
- [[QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011) — Paper Notes|Jain-Ji-Upadhyay-Watrous (2009/2011)]] — completes the classification: $\mathrm{QIP} = \mathrm{PSPACE}$ via matrix multiplicative weights update method applied to SDPs capturing quantum interactive proofs
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes|Buhrman-Cleve-Watrous-de Wolf (2001)]] — exponential quantum communication advantage
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV (1997)]] — $\Omega(\sqrt{N})$ search lower bound (Grover is optimal); BQP$^{\text{BQP}}$ = BQP via tidy subroutines
- [[Computational Complexity of Interacting Electrons and Fundamental Limitations of DFT (Schuch-Verstraete 2009) — Paper Notes|Schuch-Verstraete (2009)]] — 2D Hubbard model with local fields is QMA-complete; computing the DFT universal functional to polynomial accuracy is QMA-hard; Hartree-Fock is NP-complete
- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes|Cubitt-Montanaro (2016)]] — full complexity dichotomy for all 2-qubit $S$-Hamiltonian problems: P / NP-complete / StoqMA-complete / QMA-complete; Heisenberg and XY models QMA-complete without local terms
- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes|Cubitt-Montanaro-Piddock (2018)]] — rigorous analogue simulation framework; Heisenberg and XY with variable couplings are universal, and the simulation-power classification matches the 2-qubit complexity classification
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes|Aaronson (2010)]] — introduced Forrelation and Fourier Checking; first evidence BQP $\not\subseteq$ PH; unconditional FBQP $\not\subseteq$ FBPP$^{\text{PH}}$ oracle separation; conditional BQP $\not\subseteq$ PH via Generalized Linial-Nisan Conjecture (later proved by Tal 2017 / Raz-Tal 2019)
- [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes|de Beaudrap-Cleve-Watrous (2002)]] — 1 exact quantum query vs $\Omega(2^{n/2})$ bounded-error classical queries for hidden linear structure over $\mathrm{GF}(2^n)$; introduces QFT over finite fields with control/target inversion
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes|Aaronson-Ambainis (2015)]] — optimal 1-vs-$\tilde{\Omega}(\sqrt{N})$ quantum-classical query separation; simulation theorem ($t$ quantum queries → $O(N^{1-1/(2t)})$ classical); $k$-fold Forrelation is BQP-complete
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes|Jordan-Krovi-Lee-Preskill (2018)]] — vacuum-to-vacuum amplitude in $(1+1)$D $\phi^4$ is BQP-complete; first natural QFT problem shown BQP-hard


## Query Complexity / Lower Bound Methods

- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|Beals-Buhrman-Cleve-Mosca-de Wolf (1998)]] — THE polynomial method: $T$-query acceptance probability is degree-$2T$ polynomial; $D(f) = O(Q_2(f)^6)$ for total functions; tight symmetric function characterisation
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes|Ambainis (2000)]] — THE adversary method: run algorithm on superposition of inputs, track entanglement growth; tight $\Omega(\sqrt{N})$ for search, AND-of-ORs, permutation inversion
- [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes|Ambainis (2003)]] — first superlinear separation between polynomial degree and quantum query complexity ($M$ vs $\Omega(M^{1.321})$); introduces weighted adversary method and adversary composition theorem
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|Špalek-Szegedy (2006)]] — unifies all positive-weight adversary variants (spectral, weighted, strong weighted, Kolmogorov) into a single SDP; proves certificate complexity barrier $\sqrt{C_0 C_1}$
- [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes|Nayak (1999)]] — Nayak's bound: $m \geq (1 - H(p))n$ for quantum random access codes; QFA exponential lower bounds via Holevo's theorem
- [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes|Nayak-Wu (1999)]] — $\Omega(1/\varepsilon)$ quantum query complexity for $\varepsilon$-approximate median via polynomial method; cubic quantum-classical separation
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|BBBV (1997)]] — hybrid method: $\Omega(\sqrt{N})$ search lower bound
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes|Aaronson-Ambainis (2015)]] — optimal quantum-classical query separation via polynomial method

## Quantum Information Theory / Coding Bounds

- [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes|Nayak (1999)]] — operational form of Holevo bound for random access; tight bounds on quantum finite automata

---

## Jones Polynomial / Topological Quantum Computation

- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes|Kitaev (1997/2003)]] — introduces the toric code and topological quantum computation via anyonic braiding; abelian anyons from $\mathbb{Z}_2$ lattice gauge theory; non-abelian anyons from quantum double $D(G)$; fault tolerance from topology
- [[Simulation of Topological Field Theories by Quantum Computers (Freedman-Kitaev-Wang 2002) — Paper Notes|Freedman-Kitaev-Wang (2002)]] — quantum computers efficiently simulate UTMFs/TQFTs; TQFTs cannot exceed BQP; pants decomposition embedding technique
- [[Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003) — Paper Notes|Freedman-Kitaev-Larsen-Wang (2003)]] — braiding of anyons in $\mathrm{SU}(2)$ Chern-Simons at level $k \geq 3$, $k \neq 4$ is universal for QC; topological error protection via $e^{-\alpha \ell}$ scaling
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL (2006)]] — explicit quantum algorithm for Jones polynomial at roots of unity via Temperley-Lieb path model; BQP-complete for plat closure
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes|Aharonov-Arad (2006/2011)]] — extends BQP-hardness to polynomial $k$; elementary universality proof via Bridge and Decoupling Lemmas
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes|Aharonov-Arad-Eban-Landau (2007)]] — extends to entire Tutte polynomial (Potts model, etc.) for planar graphs; handles non-unitary TL representations; new BQP-complete problems
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes|Knill-Laflamme (1998)]] — DQC1 model; Jones polynomial estimation is DQC1-complete (Shor-Jordan 2007); normalised trace of braid group representations

- [[Preparing Topological PEPS on a Quantum Computer (Schwarz-Cubitt-Verstraete 2012) — Paper Notes|Schwarz-Cubitt-Verstraete (2012)]] — efficient quantum algorithm for preparing G-injective PEPS including topological states (toric code, RVB, quantum doubles); extends Marriott-Watrous rewinding to degenerate ground spaces

## Fault Tolerance Theory

- [[Theory of Quantum Error-Correcting Codes (Knill-Laflamme 1997) — Paper Notes|Knill-Laflamme (1997)]] — the Knill-Laflamme conditions: necessary and sufficient conditions for a quantum code to correct a given set of errors; general theory of QEC; quantum Hamming bound; pure-state vs entangled-state fidelity
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes|Aharonov-Ben-Or (1996/2008)]] — first rigorous proof that fault-tolerant QC is possible with constant error rate; concatenated CSS and polynomial codes; extends to general (non-independent) noise models; threshold $\sim 10^{-6}$
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes|Knill (2005)]] — the "Knill threshold" paper; $C_4/C_6$ architecture with error-detecting codes + postselection achieves threshold $\sim 3\%$; changed near-term feasibility expectations
- [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes|KLM (2001)]] — linear optical QC via gate teleportation + detected-error threshold; erasure codes exploit heralded failure for dramatically higher thresholds
- [[Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003) — Paper Notes|Freedman-Kitaev-Larsen-Wang (2003)]] — alternative: topological error protection via anyonic braiding; error rates scale as $e^{-\alpha \ell}$ rather than requiring combinatorial fault tolerance

---

## Hidden Subgroup Problem

- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes|Hallgren (2002)]] — polynomial-time quantum algorithm for Pell's equation and the principal ideal problem via period finding over $\mathbb{R}$ with irrational periods; extends QFT period finding to real quadratic number fields
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]] — first subexponential $2^{O(\sqrt{\log N})}$ algorithm for dihedral HSP via quantum sieve on coset states; requires superpolynomial space
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]] — polynomial-space variant of Kuperberg's dihedral HSP algorithm; trades space for slightly higher time $2^{O(\sqrt{\log N \log \log N})}$
- [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes|Bacon-Childs-van Dam (2005)]] — PGM-based HSP algorithms for semidirect products $A \rtimes \mathbb{Z}_p$; first efficient entangled multi-copy measurement; metacyclic groups and $\mathbb{Z}_p^r \rtimes \mathbb{Z}_p$ solved
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger-Høyer-Knill (2004)]] — $O(\log^4 |G|)$ queries suffice for HSP over any finite group; separates query complexity (polynomial) from computational complexity (exponential); the information is in the coset states — extracting it is the hard part
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes|Watrous (2001)]] — quantum algorithms for group-theoretic problems
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] — first connection between dihedral HSP and lattice problems; quantum reduction from unique-SVP to DCP to average-case subset sum

## Lattice Problems / Post-Quantum Cryptography

- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes|Regev (2002)]] — first quantum–lattice connection; $\Theta(n^{2.5})$-unique-SVP via dihedral coset problem + average-case subset sum; started the post-quantum cryptography discussion

## Ordered Search / Noisy Search

- [[Quantum Search in an Ordered List via Adaptive Learning (Ben-Or-Hassidim 2007) — Paper Notes|Ben-Or-Hassidim (2007)]] — optimal Bayesian noisy binary search; quantum ordered search in $< 0.32\log_2 n$ queries via FGGS subroutine

## Quantum Supremacy / Hardness of Sampling

- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes|Aaronson-Gottesman (2004)]] — stabilizer circuits are *easy* (⊕L-complete, in P); contrast with the results below
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] — IQP sampling hardness; post-IQP = PP; Hadamard gadget
- [[Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016) — Paper Notes|Bremner-Montanaro-Shepherd (2016)]] — average-case hardness of approximate IQP simulation; proves anticoncentration; Ising partition function and polynomial gap conjectures
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes|Aaronson-Arkhipov (2011)]] — BosonSampling; exact hardness via permanent #P-hardness; approximate hardness via PGC + PACC; Gaussian permanent hiding

---

## Quantum Shannon Theory / Information Theory

- [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes|Hayden-Leung-Shor-Winter (2004)]] — $O(n)$ random bits suffice to approximately randomize an $n$-qubit state; approximate private quantum channels; LOCC data hiding; locked correlations
- [[Quantum State Merging and Negative Information (Horodecki-Oppenheim-Winter 2005) — Paper Notes|Horodecki-Oppenheim-Winter (2005)]] — state merging protocol: optimal entanglement cost = conditional entropy $S(A|B)$; negative partial quantum information; solves distributed compression, multiple access channel, entanglement of assistance
- [[The Mother of All Protocols (Abeyesinghe-Devetak-Hayden-Winter 2006) — Paper Notes|Abeyesinghe-Devetak-Hayden-Winter (2006)]] — unifies all quantum Shannon protocols via the decoupling theorem; FQSW protocol simultaneously achieves state transfer and entanglement distillation; collapses mother/father families into one

---

## State Preparation

- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover-Rudolph (2002)]] — recursive bisection state preparation for log-concave distributions; $O(\log N)$ rotations
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] — arithmetic-free state preparation via inequality test; 286–375× Toffoli reduction over Grover's arcsine approach
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes|Low-Kliuchnikov-Schaeffer (2024)]] — SelectSwap network trades dirty qubits for T gates; achieves $\widetilde{O}(\sqrt{N})$ T-count for $N$-dimensional state preparation — quadratic improvement; optimal up to log factors; underlies QROAM

---

## Classical Simulation of Quantum Circuits

- [[Entanglement in a Simple Quantum Phase Transition (Osborne-Nielsen 2002) — Paper Notes|Osborne-Nielsen (2002)]] — entanglement peaks at quantum critical point in transverse Ising model; entanglement delocalises at criticality; connects to the Vidal/Jozsa-Linden entanglement-simulability picture
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes|Jozsa-Linden (2003)]] — $p$-blocked pure-state quantum computations (bounded multi-partite entanglement) are efficiently classically simulable; exponential speedup requires $\omega(\log n)$-partite entanglement
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes|Aaronson-Gottesman (2004)]] — CHP algorithm; destabilizer tableau; $O(n^2)$ simulation; stabilizer circuits are ⊕L-complete; canonical form $O(n^2/\log n)$ gates
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes|Vidal (2003)]] — if the Schmidt rank $\chi$ across any bipartition stays poly$(n)$, the computation is classically simulable in poly time; MPS/tensor network simulation origin; entanglement growth is necessary for quantum advantage
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes|Hastings (2007)]] — first rigorous area law: gapped 1D local Hamiltonians have ground-state entanglement entropy bounded by $\exp(O(v/\Delta E))$, independent of system size; proves why MPS/DMRG works for gapped 1D systems
- [[Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) — Paper Notes|Brandão-Horodecki (2013)]] — exponential decay of correlations alone (no Hamiltonian needed) implies area law in 1D; MPS approximation of poly bond dimension; completes the picture with Hastings
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes|Vidal (2004)]] — TEBD: Trotter-based MPS simulation of 1D Hamiltonian dynamics; the practical algorithm for classical simulation of 1D quantum many-body systems
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes|Verstraete-Cirac (2004)]] — PEPS: 2D generalisation of MPS; variational ansatz with area-law entanglement scaling; algorithms for ground states, dynamics, and finite temperature in 2D and higher dimensions
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes|Bremner-Jozsa-Shepherd (2011)]] — IQP circuits are classically hard to sample (also in Quantum Supremacy section)
- [[Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016) — Paper Notes|Bremner-Montanaro-Shepherd (2016)]] — strengthens to additive-error hardness under average-case conjectures

### Classical algorithms for quantum partition functions

- [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes|Mann-Helmuth (2021)]] — FPTAS for quantum partition functions at high temperature ($|\beta| \leq 1/(e^4 \Delta)$) via quantum cluster expansion; optimal convergence radius under RP ≠ NP

### Free-fermion solvability

- [[A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) — Paper Notes|Chapman-Elman-Mann (2023)]] — spin Hamiltonian is free-fermion solvable if frustration graph is claw-free with simplicial clique; unifies Jordan-Wigner and Fendley approaches; extends to arbitrary spatial dimension
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes|Verstraete-Cirac-Latorre (2009)]] — explicit quantum circuits that exactly diagonalise free-fermion Hamiltonians via Jordan-Wigner + fermionic FFT + Bogoliubov; $O(n^2)$ gates

### Classical algorithms for Ising / graph polynomials

- [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes|Mann-Bremner (2019)]] — deterministic FPTAS for complex-valued Ising partition function on bounded-degree graphs when $|\omega_e|, |\upsilon_v| = O(1/\Delta)$; via Barvinok interpolation + Patel-Regts; extends to IQP circuit amplitudes

---

## Gate Synthesis and Compilation

- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Dawson-Nielsen (2005)]] — Solovay-Kitaev theorem and efficient gate compilation
- [[Floating Point Representations in Quantum Circuit Synthesis (Wiebe-Kliuchnikov 2013) — Paper Notes|Wiebe-Kliuchnikov (2013)]] — gearbox circuit for angle squaring; floating-point small-rotation synthesis; beats ancilla-free T-count lower bound
- [[Resonant Equiangular Composite Gates (Low-Yoder-Chuang 2016) — Paper Notes|Low-Yoder-Chuang (2016)]] — composite pulse sequences for robust gate synthesis
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes|Aaronson-Gottesman (2004)]] — Clifford canonical form: any Clifford unitary has an $O(n^2/\log n)$-gate 11-round circuit
- [[Trading T Gates for Dirty Qubits in State Preparation and Unitary Synthesis (Low-Kliuchnikov-Schaeffer 2024) — Paper Notes|Low-Kliuchnikov-Schaeffer (2024)]] — SelectSwap data-lookup oracle; optimal T-count/dirty-qubit tradeoff for unitary synthesis; $\widetilde{O}(\sqrt{N})$ T-count via $O(\sqrt{N})$ dirty qubits

---

## Babbush Group — Quantum Chemistry & Resource Estimation

_Papers added from Ryan Babbush's body of work, cross-referenced with each other and with the broader vault._

- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes|Reiher, Wiebe, Svore, Wecker, Troyer (2017)]] — The FeMoCo benchmark paper (Microsoft Research). Not Babbush group, but this is the baseline that every Babbush chemistry paper improves upon; ~$10^{14}$ T gates for nitrogenase FeMoCo via 2nd-order Trotter + QPE.

## Quantum Advantage / Super-Quadratic Speedups / Planted Inference

- [[Quartic Quantum Speedups for Planted Inference (Schmidhuber, O'Donnell, Kothari, Babbush 2024) — Paper Notes|Schmidhuber, O'Donnell, Kothari, Babbush (2024)]] — Nearly quartic speedup for Planted Noisy $k$XOR and Tensor PCA via the Kikuchi method + Guided Sparse Hamiltonian framework; $n^{\ell/4} \cdot \text{poly}(n)$ quantum vs. $\widetilde{O}(n^\ell)$ classical; $\widetilde{O}(\log n)$ qubits; generalizes to any Kikuchi-solvable planted inference problem; cryptographic implications for Sparse LPN-based constructions.

---

## Quantum Advantage / Combinatorial Optimization

- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes|Jordan, Shutty, Wootters, Babbush et al. (2024)]] — Decoded Quantum Interferometry (DQI): uses QFT to reduce optimization to syndrome decoding; superpolynomial speedup for Optimal Polynomial Intersection (OPI) via Reed-Solomon decoding; beats general-purpose classical heuristics on crafted max-XORSAT instances; semicircle law converts any decoding radius into an approximation guarantee; $\sim 10^8$ Toffolis / $9 \times 10^3$ qubits for OPI at $p = 521$.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — Resource estimation for six fault-tolerant quantum optimization heuristics (QAOA, quantum simulated annealing, amplitude amplification, etc.) on four problem families; finds quadratic speedups insufficient for advantage on modest surface-code processors.

---

## Quantum Advantage / BQP-Completeness

- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes|Babbush, Berry, Kothari, Somma, Wiebe (2023)]] — Maps $2^n$ coupled classical oscillators to [[Hamiltonian simulation]] with $\text{poly}(n)$ cost; energy-balanced encoding avoids condition-number bottleneck; $2^{\Omega(n)}$ classical lower bound via glued trees; BQP-complete when oracles are circuits; $O(\sqrt{d})$ sparsity dependence from incidence-matrix factorization.

---

## Quantum Advantage / Topological Data Analysis

- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes|Berry, Su, Babbush et al. (2024)]] — Optimized fault-tolerant quantum algorithm for Betti number estimation with ~3–4 orders of magnitude improvement over prior work; ~6.8B Toffolis for a classically intractable instance ($K(16,16)$, $n=256$); super-quadratic speedup only for multiplicative error when Betti number is large; partial dequantization via path-integral Monte Carlo shows exponential dimension alone is insufficient for quantum advantage.

---

## Quantum Advantage / Complexity of Quantum Chemistry

- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al. (2023)]] — First-quantized quantum algorithms for dynamics beat classical mean-field (RT-TDHF/RT-TDDFT) by up to a quintic factor in basis size $N$; introduces first-quantized classical shadows for $k$-RDM with polylogarithmic $N$ dependence; strongest advantage at finite temperature where classical cost scales as $M^2 \simeq N^2$.
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. (2022)]] — Examines the exponential quantum advantage hypothesis for ground-state quantum chemistry; finds no evidence for generic EQA — ansatz overlaps decay exponentially for Fe-S clusters, ASP cost correlates with overlap, and classical heuristics (coupled cluster, DMRG, PEPS, DMET) scale polynomially in tested domains; polynomial speedups remain possible.
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al. (2022)]] — Joint classical-quantum resource estimation for CYP P450 Cpd I; DMRG+NEVPT2 and CCSD(T) benchmarks establish multiconfiguational character (triradical doublet); THC qubitization gives 4.6M physical qubits / 73 hours for 58-orbital active space; runtime crossover with DMRG at ≥31 orbitals; full-system extrapolation (500 orbitals, 1.5T Toffolis) remains infeasible.

---

## Quantum Simulation of Chemistry

- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al. (2022)]] — First systematic costing of molecular force estimation on quantum computers; three Heisenberg-limited FT algorithms (finite difference, overlap estimation, gradient-based); shows force block-encoding costs at most constant-factor overhead via THC/DF differentiation; $\lambda_F \ll \lambda_H$ for extended systems; MD infeasible but geometry optimisation promising.
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes|McClean, Babbush, Lin et al. (2019)]] — DG blocking procedure interpolates between compact MO bases ($O(N^4)$ integrals) and diagonal primitive bases ($O(N^2)$ integrals) by constructing block-local functions via SVD; empirical scaling $O(N_h^{2.6})$ for fault-tolerant simulation cost on hydrogen chains, crossing over from Gaussian at 15–20 atoms; hybrid active space with DMRG achieves near-CBS accuracy at 1–2 orders of magnitude lower cost.
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. (2018)]] — Double factorization (Cholesky + eigenvalue truncation) of two-electron integrals reduces Trotter step from $O(N^4)$ to $O(N^2 \log N)$ gates asymptotically in arbitrary MO basis; $O(N^2)$ depth on linear chain; ~4,000 layers for 50-qubit molecular simulation; applied to iron-sulfur clusters up to 146 spin-orbitals.
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush (2019)]] — First qubitization of arbitrary-basis chemistry Hamiltonians; single factorization gives $\widetilde{O}(N^{3/2}\lambda)$ Toffoli complexity; sparse Coulomb variant achieves $8.4 \times 10^{10}$ Toffolis for FeMoCo (LLDUC, 152 spin-orbitals) — ~$700\times$ improvement over Reiher et al.; introduces QROAM and measurement-based QROM uncomputation.
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. (2021)]] — THC-based qubitization achieves $\widetilde{O}(N\lambda_\zeta/\varepsilon)$ Toffoli complexity in arbitrary molecular orbital basis; lowest known fault-tolerant resource estimates for FeMoCo ($5.3 \times 10^9$ Toffolis, 2,142 logical qubits); ~4M physical qubits and <4 days on surface code.
- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes|Berry, Tong, Babbush, Rubin et al. (2024)]] — End-to-end resource estimation for FeMoCo including MPS state preparation and QPE with imperfect overlap; $7\times$ improved unitary synthesis for MPS prep; Kaiser/Slepian window-optimised QPE; MPS overlap extrapolation protocol; $7.3 \times 10^{10}$ Toffolis at 95% confidence — only $2.3\times$ above perfect-overlap estimates.
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low, King, Berry, Babbush, Somma, Rubin (2025)]] — Spectrum amplification + DFTHC factorization for ground-state energy estimation; effective LCU norm $\lambda_{\text{eff}} = \sqrt{2\Lambda E_{\text{gap}}}$ replaces $\Lambda$; 4–195× speedup over prior art; FeMoCo-76: $9.99 \times 10^8$ Toffolis, 1,459 qubits; SOS decomposition via SDP; new current best for second-quantized chemistry resource estimates.
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al. (2022)]] — Applies SF/DF/THC qubitization resource estimation to CYP P450 active spaces (5–58 orbitals); THC gives $O(N^{2.5})$ Toffoli scaling; direct DMRG-vs-QPE runtime comparison on same Hamiltonians; introduces L1-regularized THC optimization from CP3.
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush (2021)]] — Greedy sum-of-squares decomposition of two-body fermion operators with $O(n^5)$ iteration cost; ~$4\times$ fewer tensor factors than Takagi for UCC compilation; compressed CCSD as warm start for ADAPT-VQE on strongly correlated systems.
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, Babbush et al. (2021)]] — QC-QMC: quantum computer provides trial wavefunction to unbias classical AFQMC via shadow tomography; 8–16 qubit experiments on Sycamore achieve accuracy competitive with CCSD(T) on systems up to 120 orbitals; noise-resilient overlap ratios; virtual correlation via matchgate contraction.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — First scalable quantum chemistry experiment on hardware; VQE achieves chemical accuracy for H₂ using Bravyi-Kitaev encoding; comparison with PEA shows VQE more error-tolerant on NISQ devices.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman, Mejuto-Zaera, Babbush et al. (2018)]] — Addresses the neglected state-preparation bottleneck in QPE: uses ASCI to certify that HF or small multi-determinant states have $\geq 0.75$ squared overlap for most systems of interest; introduces sequential $O(nL)$-gate multi-determinant state preparation algorithm.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — Practical strategies for UCC-VQE on near-term hardware: Trotterized circuit reduction, MP2-based parameter pruning, active-space restriction, and analytical gradient estimation.
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. (2018)]] — Asymptotically best gate complexity for molecular simulation at time of publication: $\tilde{O}(\eta^2 N^3 t)$ gates, $\tilde{O}(\eta)$ qubits; uses first-quantized CI matrix with on-the-fly integral evaluation and truncated Taylor series LCU.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean, Babbush et al. (2018)]] — Fermionic swap network: Trotter step in depth $N$, $N(N-1)/2$ two-qubit gates, linear connectivity only; Slater determinant prep in depth $\leq N/2$; directly relevant to NISQ hardware.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Qubitization of electronic structure Hamiltonians with T complexity $O(N^3/\varepsilon)$; introduces unary iteration, QROM, alias sampling; includes full surface-code compilation to estimate ~1M qubits for FeMoco-scale problems.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]] — First-quantized plane-wave simulation using interaction picture; gate complexity $\widetilde{O}(N^{1/3}\eta^{8/3} t)$ — sublinear in basis size; $O(\eta \log N)$ qubits; best scaling for molecular simulation when $N \gg \eta$.
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — First constant-factor compilation of first-quantized plane-wave chemistry; qubitization Toffoli cost $\widetilde{O}(\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})/\varepsilon$; ~$1000\times$ circuit-level improvement over naive; first-quantized qubitization often beats second-quantized Gaussian methods by orders of magnitude in surface-code spacetime volume.
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry, Rubin, Babbush et al. (2024)]] — Extends first-quantized plane-wave methods to realistic materials with GTH pseudopotentials; full nonlocal pseudopotential block encoding; heterogeneous catalysis and battery cathode resource estimates.
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — Non-BO dynamics for ICF stopping power; extends [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su et al. (2021)]] block encoding; hybrid QROM+Newton-Raphson inverse square root; bespoke 8th-order [[Product Formulas]].

- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes|Berry, Wan, Baczewski, Eklund, Tikku, Babbush (2025)]] — Quantum FMM for first-quantized real-space chemistry; $\widetilde{O}(\eta)$ Coulomb evaluation replaces $O(\eta^2)$; best known complexity for $N < \eta^6$.
- [[Quantum Computing Enhanced Computational Catalysis (von Burg, Low, Häner, Steiger, Reiher, Roetteler, Troyer 2020) — Paper Notes|von Burg et al. (2020)]] — double factorization qubitization for Rh catalysis and FeMoCo; $3.5\times 10^{10}$ Toffolis for FeMoCo (DF, 152 spin-orbitals); introduces regularised DF with reduced 1-norm
- [[Accelerating Quantum Computations of Chemistry Through Regularized Compressed Double Factorization (Oumarou, Scheurer, Parrish, Hohenstein, Gogolin 2024) — Paper Notes|Oumarou et al. (2024)]] — regularized compressed DF with $L_1$ penalty reduces 1-norm and Toffoli counts; follow-up to von Burg et al.; Quantum **8**, 1371
- [[Faster Quantum Chemistry Simulations via Improved Tensor Factorization and Active Volume Compilation (Caesura, Cortes, Pol, Sim, Steudtner, Anselmetti, Degroote, Moll, Santagati, Streif, Tautermann 2025) — Paper Notes|Caesura et al. (2025)]] — improved THC factorisation (BLISS) + active volume compilation for photonic architecture; Phys. Rev. Research

## Quantum Simulation of Materials / Periodic Systems

- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes|Berry, Rubin, Babbush et al. (2024)]] — First complete block encoding of GTH pseudopotential in first-quantized plane waves; arithmetic-based evaluation ($O(\log^2 N)$) replaces QROM ($O(N)$); handles non-cubic cells via Gramian; resource estimates for CO/Pt catalysis and LNO cathode; ~50× fewer qubits than second-quantized approaches at ~12× higher Toffoli count.
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — Symmetry-adapted qubitization (sparse/SF/DF/THC) for periodic systems in Bloch orbital (Gaussian) basis; $\sqrt{N_k}$ per-step speedup for sparse/SF/DF via QROAM data reduction; no asymptotic speedup for THC; k-THC factorization; resource estimates for diamond and LiNiO₂ cathode; DF dominates at all tested sizes.
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes|Tazhigulov, Sun, Babbush, Chan et al. (2022)]] — NISQ benchmark: QITE with classical recompilation on Weber/Sycamore for Fe-S cluster and α-RuCl₃ spin models (up to 11 qubits, 310 gates); deployable gate budget ~1/5 of random circuit supremacy for physically meaningful results; 10-site simulation fails without hardware-tuned parameters.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — Plane-wave dual basis reduces Hamiltonian terms to $\Theta(N^2)$; FFFT enables $O(N)$-depth Trotter steps on planar lattice; depth $\widetilde{O}(N^{8/3})$ via LCU; measurement cost $O(N^4/\varepsilon^2)$; proposes jellium as NISQ supremacy target.
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al. (2020)]] — Optimized Trotter for Hubbard/jellium/periodic materials; extensive error framing gives $O(1)$ T cost for Hubbard; resource estimates for lithium, diamond, silicon, graphite.
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes|Wecker-Hastings-Wiebe-Clark-Nayak-Troyer (2015)]] — End-to-end fault-tolerant pipeline for 2D Hubbard: adiabatic state prep (mean-field + plaquettes), $O(\log N)$-depth Trotter, improved QPE, correlation function measurements, non-destructive measurement; $20 \times 20$ lattice feasible with $\sim 2000$ qubits.

---

## Active-Space Corrections / Basis Set Expansion

- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, Babbush et al. (2021)]] — Virtual correlation via matchgate contraction: reduces full-space overlap to active-space overlap with zero extra quantum cost; more efficient than VQSE (no 3/4-RDM measurement overhead).
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes|Takeshita, Rubin, Babbush, McClean (2019)]] — [[Virtual Quantum Subspace Expansion (VQSE)]] and orbital relaxation: recover dynamic correlation from virtual orbitals using only extra measurements and classical postprocessing; 4-qubit VQSE matches 20-qubit accuracy for H₂ in cc-pVDZ.

## Hybrid Quantum-Classical Algorithms (QMC / VQE / QAOA)

- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes|Tazhigulov, Sun, Babbush, Chan et al. (2022)]] — QITE + classical recompilation for finite-temperature spin model simulation on Sycamore; extensive error mitigation (Floquet calibration, postselection, dynamical decoupling, commuting-Hamiltonian rescaling); honest benchmark showing the gap between synthetic quantum advantage and physically meaningful simulation.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins, McClean, Rubin, Babbush et al. (2021)]] — Basis Rotation Grouping for VQE measurement; $O(N)$ circuits; up to $1000\times$ fewer shots than standard bounds; noise-resilient diagonal measurements with symmetry postselection.
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush (2021)]] — Compressed UCC as ADAPT-VQE warm start; demonstrates ADAPT failure from poor initial states (triplet O₂) and recovery via 6-factor compressed CCSD.
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, Babbush et al. (2021)]] — QC-QMC paradigm: quantum device provides trial wavefunction constraint for classical AFQMC; fundamentally different from VQE (quantum device never represents ground state); noise-resilient via overlap ratio cancellation.
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Wan, Huggins, Lee, Babbush (2022)]] — Matchgate classical shadows with poly-time post-processing; removes the exponential classical bottleneck from QC-AFQMC overlap estimation; also handles local fermionic operators and Gaussian fidelities.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — Hardware demonstration of VQE with UCC ansatz on superconducting qubits; establishes VQE's robustness to systematic errors.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — Practical strategies for UCC-VQE: Trotterized product-form circuits, MP2-based parameter pruning, active-space restriction (CAS-UCC), and analytical gradient estimation that cuts gradient sampling cost by orders of magnitude.
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean (2018)]] — Reduces VQE measurement cost $> 10\times$ via LP over fermionic $n$-representability constraints; also demonstrates noise-robust physicality restoration of measured 2-RDMs via DQG projection.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — Low-depth variational ansatz for jellium on planar hardware; measurement cost $O(N^4/\varepsilon^2)$ via dual-basis commuting-group strategy (2 circuits instead of $O(N^4)$).
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes|Takeshita, Rubin, Babbush, McClean (2019)]] — VQSE and orbital relaxation as VQE postprocessing: improves active-space VQE results without additional qubits or circuit depth.

---

## Fermion-to-Qubit Encodings

- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — First experimental use of the Bravyi-Kitaev transformation on hardware; demonstrates 4→2 qubit reduction via symmetry.

---

## Phase Estimation

- [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes|Kimmel-Low-Yoder (2015)]] — SPAM-tolerant non-adaptive phase estimation with Heisenberg scaling; estimates systematic gate errors ($\alpha$, $\epsilon$, $\theta$) in a universal single-qubit gate set; additive error threshold $1/\sqrt{8}$; composite sequence for off-resonance error amplification; improved Higgins scaling constant from $54\pi$ to $10.7\pi$
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes|Wiebe-Granade (2016)]] — RFPE: Gaussian + rejection-sampling approximation to Bayesian posterior; PGH experiment design; $O(\log(1/\varepsilon))$ experiments; Heisenberg-limited; noise-tolerant; restart protocol for tail failure recovery.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — First complete end-to-end execution of Trotterized PEA for quantum chemistry on hardware (3 qubits, 1 Trotter step).
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Phase estimation on the qubitized walk operator; Heisenberg-limited $O(\lambda/\varepsilon)$ query complexity; uses optimized entanglement-based PEA resource state $\chi_m$.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman, Mejuto-Zaera, Babbush et al. (2018)]] — Addresses initial-state preparation for QPE: empirically certifies ground-state overlap for broad system classes; provides sequential $O(nL)$-gate circuit for preparing $L$-determinant superpositions.

---

## Hamiltonian Simulation ([[Product Formulas]]s / Trotter)

- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes|McClean, Babbush, Lin et al. (2019)]] — Block-diagonal swap network for DG Hamiltonian: Trotter step depth $O(N_b n_\kappa^3)$, interpolating between $O(N)$-depth diagonal and $O(N^3)$-depth general regimes; also supports low-rank factorization within blocks.
- [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes|Motta, Babbush, Chan et al. (2018)]] — Double factorization reduces Trotter step gates from $O(N^4)$ to $O(N^2 \log N)$ for molecular Hamiltonians; each factor is a Givens rotation layer + swap network Ising layer on a linear chain.
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes|Rubin, Lee, Babbush (2021)]] — Sum-of-squares Trotter compilation: each factor is a Givens rotation layer + Ising swap network layer; unitary compression minimizes the number of factors.
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes|O'Malley, Babbush et al. (2016)]] — Demonstrates first-order Trotter simulation of H₂; PEA limited by Trotter step count; Trotter term ordering optimized classically per bond length.
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes|Kivlichan, McClean, Babbush et al. (2018)]] — Fermionic swap network achieves Trotter step depth $N$ with $N(N-1)/2$ gates on a linear chain; Slater determinant preparation in depth $\leq N/2$; conjectured gate-count optimal.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — Plane-wave dual basis Trotter step at depth $O(N)$ on planar lattice; total depth $O(N^{7/2}/\sqrt{\varepsilon})$ at fixed density; uses FFFT to alternate between diagonal representations.
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes|Kivlichan, Gidney, Babbush et al. (2020)]] — Optimized second-order Trotter for Hubbard and jellium with Hamming weight phasing; $O(1)$ T complexity for Hubbard with extensive error; compiled surface-code estimates show classically intractable instances in $<1$ hour on $\sim 300$K physical qubits.
- [[Faster Algorithmic Quantum and Classical Simulations by Corrected Product Formulas (Bagherimehrab-Berry-Schleich-Aldossary-Angulo-Aspuru-Guzik 2024) — Paper Notes|Bagherimehrab-Berry-Aspuru-Guzik et al. (2024)]] — Corrected [[Product Formulas]]s: boundary-injected commutator correctors improve PF$_{2k}$ error from $O(\alpha t^{2k+1})$ to $O(\alpha^2 t^{2k+1})$ for perturbed systems; $O(t^{2k+3})$ for non-perturbed; ancilla-free; validated on Heisenberg, Ising, Hubbard and IBM hardware.

## Hamiltonian Simulation (LCU / Taylor series)

- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — Compiled first-quantized plane-wave interaction picture with qubitized Dyson series; Toffoli cost $\widetilde{O}(\eta^{8/3}N^{1/3}/\varepsilon)$; incremental kinetic energy register reduces per-step cost from $O(\eta n_p^2)$ to $O(n_p^2)$.
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. (2018)]] — Applies truncated Taylor series (LCU) to first-quantized CI matrix; achieves $O(\log 1/\varepsilon)$ precision scaling vs Trotter's $O(1/\varepsilon)$.
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes|Kivlichan, Wiebe, Babbush, Aspuru-Guzik (2017)]] — Applies truncated Taylor series to real-space (position grid) many-body simulation; $\tilde{O}(\eta^2)$ pairwise oracle queries for fixed grid spacing; warns that worst-case discretization error scales exponentially in $\eta D$.
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes|Babbush, Wiebe, McClean et al. (2018)]] — Plane-wave dual basis gives $\Theta(N^2)$ Hamiltonian terms; LCU (on-the-fly) achieves depth $\widetilde{O}(N^{8/3})$, best asymptotic circuit depth for electronic structure at publication.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]] — Applies interaction-picture LCU (Low & Wiebe framework) to first-quantized plane-wave chemistry; $\lambda = O(\eta^{5/3} N^{1/3})$; total cost $\widetilde{O}(\eta^{8/3} N^{1/3} t)$; sublinear in $N$.

## Hamiltonian Simulation (Qubitization / Block Encoding)

- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes|Babbush, Berry, Kothari, Somma, Wiebe (2023)]] — Block encoding of classical oscillator Hamiltonian via incidence-matrix factorization; $O(\sqrt{d})$ sparsity scaling; uses QSP/qubitization for optimal simulation; also demonstrates phase-estimation fallback for generalized coordinates (Theorem 4).
- [[Fault-Tolerant Quantum Simulation of Materials Using Bloch Orbitals (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — Extends molecular qubitization (sparse/SF/DF/THC) to periodic Bloch orbital Hamiltonians; symmetry-adapted QROAM reduces per-step cost by $\sqrt{N_k}$ for sparse/SF/DF; controlled momentum-register swaps set $O(N_k N)$ cost floor; compiled resource estimates for diamond and LNO battery cathode.
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes|Berry, Gidney, Motta, McClean, Babbush (2019)]] — First qubitization for arbitrary molecular orbital bases via single Coulomb factorization ($\widetilde{O}(N^{3/2}\lambda)$) and sparse Coulomb thresholding; introduces QROAM (space-time tradeoff for QROM) and measurement-based QROM uncomputation; $8.4 \times 10^{10}$ Toffolis for FeMoCo.
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee, Berry, Babbush et al. (2021)]] — THC qubitization: $\widetilde{O}(N\lambda_\zeta/\varepsilon)$ Toffolis, $\widetilde{O}(N)$ space in arbitrary basis; diagonalizes Coulomb operator via non-orthogonal THC basis with per-term Givens rotations from QROM; lowest known FeMoCo resource estimates; full surface code compilation.
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — First-quantized qubitization with kinetic energy as bit-product LCU and failed PREP fallback; Toffoli cost $\widetilde{O}(\eta^{4/3}N^{2/3} + \eta^{8/3}N^{1/3})/\varepsilon$; outperforms second-quantized qubitization for $N \gtrsim 10^3$.
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush, Gidney et al. (2018)]] — Applies qubitization to electronic structure and Hubbard Hamiltonians in the plane-wave dual basis; T complexity $O(N^3/\varepsilon)$; introduces unary iteration, QROM, and coherent alias sampling; compiled surface-code resource estimates show ~1M qubits for classically intractable instances.
- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven (2019)]] — Introduces asymmetric qubitization (two different PREPARE oracles); applies to SYK model with gate complexity $O(N^{7/2}t + N^{5/2}t\,\mathrm{polylog}(N/\varepsilon))$; exponential improvement in $1/\varepsilon$ and polynomial improvement in $N$ over prior Trotter approach; T-count $< 10^7 Jt$ at $N=100$.
- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — Qubitized walks for optimization cost functions: SK walk at $6N + O(\log^2 N)$ Toffolis, LABS walk at $4N + O(\log N)$ Toffolis; stroboscopic adiabatic walk via inflated-$\lambda$ qubitization.
- [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes|Low, King, Berry et al. (2025)]] — Spectrum amplification via rectangular block-encoding of SOS Hamiltonian square root; DFTHC factorization interpolating DF/THC; effective LCU norm $\sqrt{2\Lambda E_{\text{gap}}}$; current best FeMoCo resource estimates ($9.99 \times 10^8$ Toffolis).
- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes|King, Low, Babbush, Somma, Rubin (2025)]] — General SOSSA theory: SOS decomposition via SDP + spectral amplification for arbitrary Hamiltonians; adaptive energy/phase estimation without prior on $\Delta$; query-optimal lower bounds via PARITY$\circ$OR; SYK degree-2 Majorana SOS demonstration.

## Quantum Simulation of Holographic / Strongly Correlated Models

- [[Quantum Simulation of the Sachdev-Ye-Kitaev Model by Asymmetric Qubitization (Babbush, Berry, Neven 2019) — Paper Notes|Babbush, Berry, Neven (2019)]] — SYK model simulation via asymmetric qubitization; $O(N^{7/2}t)$ gate complexity; motivated by AdS/CFT and holographic duality; T-count makes SYK among the most viable early surface-code applications.
- [[Digital Quantum Simulation of Minimal AdS-CFT (García-Álvarez, Egusquiza, Lamata, del Campo, Sonner, Solano 2017) — Paper Notes|García-Álvarez et al. (2017)]] — digital Trotter simulation of SYK$_4$ at $N=4,6,8$ Majorana modes; gate complexity scaling; early holographic simulation proposal
- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes|King, Low, Babbush, Somma, Rubin (2025)]] — SOSSA applied to SYK: degree-2 Majorana SOS + double factorization achieves $\sqrt{\Delta_{\text{SOS}}\lambda_{\text{SOS}}} \sim N^{3/2}$ vs $\lambda_{\text{LCU}} \sim N^2$ — a $\sqrt{N}$ asymptotic speedup.

## Quantum Dynamics / Stopping Power / Warm Dense Matter

- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — First constant-factor resource estimate for a quantum dynamics simulation of practical relevance; computes ICF stopping power via non-BO first-quantized plane-wave simulation; $10^{13}$–$10^{17}$ Toffoli gates (8th-order Trotter) for 28–1729 electrons; $\sim 10^3$ logical qubits; introduces hybrid QROM+Newton-Raphson inverse square root and bespoke 8th-order [[Product Formulas]].
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al. (2023)]] — Asymptotic advantage of quantum dynamics over classical mean-field; motivates the stopping power application.

## First-Quantized Methods / Sparse Simulation

- [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes|Berry, Wan, Baczewski, Eklund, Tikku, Babbush (2025)]] — Quantum FMM reduces Coulomb potential evaluation from $O(\eta^2)$ to $\widetilde{O}(\eta)$ per Trotter step; overall gate complexity $t(\eta^{4/3}N^{1/3} + \eta^{1/3}N^{2/3})(\eta N t/\epsilon)^{o(1)}$; lowest known complexity for $N < \eta^6$; resolves a long-standing open problem in first-quantized product-formula simulation.
- [[Quantum Computation of Stopping Power for Inertial Fusion Target Design (Rubin, Berry, Babbush et al 2023) — Paper Notes|Rubin, Berry, Babbush et al. (2023)]] — First-quantized non-BO dynamics with quantum projectile; extends Su et al. block encoding; $10^{13}$–$10^{17}$ Toffoli gates for ICF-relevant systems.
- [[Quantum Simulation of Exact Electron Dynamics Can Be More Efficient Than Classical Mean-Field Methods (Babbush, Huggins, Berry et al 2023) — Paper Notes|Babbush, Huggins, Berry et al. (2023)]] — Tightens first-quantized Trotter bounds for dynamics to $(N^{1/3}\eta^{7/3}t + N^{2/3}\eta^{4/3}t)(Nt/\epsilon)^{o(1)}$; introduces first-quantized classical shadows and efficient second-to-first quantization state conversion.
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes|Su, Berry, Wiebe, Rubin, Babbush (2021)]] — Definitive constant-factor analysis of first-quantized plane-wave chemistry via qubitization and interaction picture; $O(\eta\log N)$ qubits; concrete resource estimates show $10^{10}$–$10^{12}$ Toffolis for materials and molecules; also introduces first-quantized real-space algorithms (Appendix K) matching the same asymptotic scaling.
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. (2018)]] — Demonstrates that CI (first-quantized) representation needs only $\tilde{O}(\eta)$ qubits; 1-sparse decomposition via graph coloring enables efficient oracle construction.
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes|Kivlichan, Wiebe, Babbush, Aspuru-Guzik (2017)]] — First-quantized real-space simulation using position grid; high-order finite difference kinetic energy with bounded LCU norm; rigorous discretization error analysis with exponential worst-case warning.
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes|Babbush, Berry, McClean, Neven (2019)]] — First-quantized plane-wave simulation; $\eta$ registers of $\log N$ qubits; antisymmetry in wavefunction; interaction picture with $A=T$ gives $\widetilde{O}(\eta^{8/3} N^{1/3} t)$ — best gate complexity for molecular chemistry at publication.

## Real-Space / Continuous-Variable Simulation

- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes|Kivlichan, Wiebe, Babbush, Aspuru-Guzik (2017)]] — Foundational analysis of real-space grid simulation costs; Theorems 3 and 4 bound both the discrete simulation error and the continuous-to-discrete discretization error separately.

## Quantum Linear Systems (QLSP)

- [[Optimal Scaling Quantum Linear Systems Solver via Discrete Adiabatic Theorem (Costa, An, Sanders, Su, Babbush, Berry 2021) — Paper Notes|Costa, An, Sanders, Su, Babbush, Berry (2021)]] — First optimal QLSP solver: $O(\kappa \log(1/\varepsilon))$ query complexity matching the lower bound; discrete adiabatic theorem with explicit gap dependence avoids Dyson series overhead; Dolph-Chebyshev eigenstate filtering with closed-form angles and two ancilla qubits.
- [[The Discrete Adiabatic QLSP Has Lower Constant Factors than the Randomized Solver (Costa, An, Babbush, Berry 2023) — Paper Notes|Costa, An, Babbush, Berry (2023)]] — Numerical validation of the optimal QLSP solver: actual constant factor $\alpha \approx 1.56$ is ~1,500× smaller than the analytical bound; discrete walk ~20× cheaper than randomized adiabatic method; settles the comparison with Jennings et al. (2023).

---

## Quantum Machine Learning

- [[Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) — Paper Notes|Kieferová-Wiebe (2017)]] — quantum Boltzmann machine training via relative entropy minimisation and Golden-Thompson lower bound; enables tomography + generative modelling; fermionic Hamiltonian model
- [[Power of Data in Quantum Machine Learning (Huang, Babbush, McClean et al 2021) — Paper Notes|Huang, Babbush, McClean et al. (2021)]] — Rigorous framework for assessing quantum prediction advantage in supervised learning; introduces geometric difference $g_{CQ}$ as function-independent pre-screening test; shows data can elevate classical ML to match quantum models even on quantum-generated labels; proposes projected quantum kernels with provable speedup (discrete log) and empirical advantage up to 30 qubits on engineered datasets.

---

## Fault-Tolerant Quantum Optimization / Resource Estimation

- [[Expressing and Analyzing Quantum Algorithms with Qualtran (Harrigan, Khattar, Babbush, Rubin 2024) — Paper Notes|Harrigan, Khattar, Babbush, Rubin (2024)]] — Qualtran: open-source Python framework for expressing and analyzing fault-tolerant quantum algorithms via hierarchical bloq composition; standard library of primitives (rotations, unary iteration, QROM/QROAM, state preparation, block encoding, QSP, phase estimation); case studies in [[Hamiltonian simulation]], chemistry (sparse/SF/DF/THC), and cryptography (RSA/ECC); surface-code physical cost models (Gidney-Fowler + Beverland); symbolic cost propagation with error budget optimization.
---

## Benchmark Systems

- [[FeMoCo Resource Estimation Timeline]] — Synthesis note tracking all FeMoCo resource estimates from 2017–2025; covers Trotter → qubitization → SF → DF → THC → DFTHC+spectrum amplification progression; 50,000× improvement in eight years
- [[NMR Implementation of a Molecular Hydrogen Quantum Simulation with Adiabatic State Preparation (Du, Xu, Peng, Wang, Wu, Lu 2010) — Paper Notes|Du et al. (2010)]] — NMR H₂ simulation with adiabatic state preparation and phase estimation; PRL **104**, 030502
- [[Quantum Implementation of Unitary Coupled Cluster for Simulating Molecular Electronic Structure (Shen, Zhang, Chen, Zhang, Yung, Kim 2015) — Paper Notes|Shen et al. (2015)]] — trapped-ion UCC implementation for HeH⁺; PRA **95**, 020501(R)
- [[Quantum Simulation of Helium Hydride in a Solid-State Spin Register (Wang, Dolde, Biamonte, Babbush, Bergholm et al. 2015) — Paper Notes|Wang et al. (2015)]] — NV-center HeH⁺ simulation via phase estimation; ACS Nano **9**(8), 7769
- [[Observation of Separated Dynamics of Charge and Spin in the Fermi-Hubbard Model (Arute et al. 2020) — Paper Notes|Arute et al. (2020)]] — Google Sycamore observation of spin-charge separation in 1D Fermi-Hubbard; NISQ dynamics experiment

---

- [[Compilation of Fault-Tolerant Quantum Heuristics for Combinatorial Optimization (Sanders, Berry, Babbush et al 2020) — Paper Notes|Sanders, Berry, Babbush et al. (2020)]] — Compiles fault-tolerant circuits for six optimization heuristics (amplitude amplification, QAOA, adiabatic, three QSA variants) on four cost functions (L-term, QUBO, SK, LABS); constant-factor Toffoli counts and surface-code resource estimates; concludes quadratic speedups insufficient for advantage on modest processors; introduces QROM function interpolation, improved LHPST rotation for dense Hamiltonians, stroboscopic adiabatic walk, explicit spectral gap amplification oracle.

## Gradient Estimation / Classical Optimization for Quantum Circuits

- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al. (2022)]] — Nuclear force (energy derivative) estimation via Hellmann-Feynman theorem and finite differences; NISQ shot-optimised tomography with parallelised importance sampling; FT algorithms using OEA and gradient-based estimation (building on Huggins et al. 2022); gradient-based method achieves $\tilde{O}(T_H \lambda_H \gamma^{-1} N_a \lambda_F / \varepsilon)$.
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes|Romero, Babbush et al. (2018)]] — Derives analytical gradient for product-of-Pauli-exponentials ansatz via Hadamard test; reduces gradient measurement cost by $(2\delta)^2 \gg 1$ relative to numerical finite differences.

## Error Mitigation / Quantum Error Correction (NISQ)

- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes|Tazhigulov, Sun, Babbush, Chan et al. (2022)]] — Comprehensive error mitigation case study: Floquet calibration, Z₂ postselection, dynamical decoupling on ancilla, and commuting-Hamiltonian rescaling; demonstrates both power and limits of stacked mitigation (works up to ~100 two-qubit gates, fails at ~310).
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes|McClean, Jiang, Rubin, Babbush, Neven (2019)]] — Post-processed error mitigation via stabilizer projections and QSE; pseudo-threshold $p \approx 0.50$ on `[[5,1,3]]` code under depolarizing noise; generalizes symmetry verification to arbitrary codes, recovery operations, and approximate symmetries of unencoded Hamiltonians.

## Measurement Efficiency / Error Mitigation

- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes|King, Gosset, Kothari, Babbush (2024)]] — First triply efficient (sample-efficient, time-efficient, two-copy) shadow tomography for $k$-body fermionic operators and all Paulis; two-copy measurements necessary and sufficient; framework via Bell sampling + fractional graph coloring + chi-boundedness; $O((\log n)/\epsilon^4)$ for 1-body fermionic operators.
- [[Matchgate Shadows for Fermionic Quantum Simulation (Wan, Huggins, Lee, Babbush 2022) — Paper Notes|Wan, Huggins, Lee, Babbush (2022)]] — Classical shadows from random matchgate circuits; Clifford matchgates form a matchgate 3-design; efficient $O(n^3)$–$O(n^4)$ post-processing for local fermionic operators ($O(n^{k/2})$ variance), Gaussian fidelities ($O(\sqrt{n}\log n)$ variance), and Slater determinant overlaps (sublinear variance); removes exponential classical bottleneck from QC-AFQMC.
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes|O'Brien, Babbush et al. (2022)]] — Optimised NISQ tomography for force vectors: parallelised importance sampling, fermionic shadow tomography, BRG; force estimation roughly same cost as energy estimation in NISQ; fermionic shadows give $O(N_a^{2.84}/\varepsilon^2)$ for H-chains.
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes|Huggins, Wan, McClean, Babbush et al. (2022)]] — Gradient-based algorithm for estimating $M$ expectation values with $\tilde{O}(\sqrt{M}/\varepsilon)$ state preparation queries; worst-case optimal in the high-precision regime; applies quantum gradient estimation (Gilyen et al.) to a Hadamard-test probability oracle encoding expectation values as a gradient.
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes|Huggins, McClean, Rubin, Babbush et al. (2021)]] — Basis Rotation Grouping: double factorization of two-electron integrals yields $O(N)$ measurement circuits instead of $O(N^4)$; empirical shot scaling $\sim N^{2.75}$ for H-chains; number-basis postselection on $\eta$ and $S_z$ for error mitigation; readout error resilience from $O(1)$-local operators.
- [[Unbiasing Fermionic Quantum Monte Carlo with a Quantum Computer (Huggins, Babbush et al 2021) — Paper Notes|Huggins, Babbush et al. (2021)]] — Shadow tomography for overlap estimation: $O(\log M / \varepsilon^2)$ measurements for $M$ queries; noise-resilient overlap ratios cancel depolarizing noise without calibration; partitioned shadow tomography reduces circuit depth.
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes|Rubin, Babbush, McClean (2018)]] — LP-based variance reduction using fermionic $n$-representability constraints; optimal shot allocation proof; 2-RDM physicality restoration via iterative DQG projection and SDP reconstruction.
- [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes|Takeshita, Rubin, Babbush, McClean (2019)]] — VQSE requires active-space RDM measurement up to 4-RDM; cumulant truncation trades accuracy for reduced measurement burden ($N_A^8 \to N_A^4$).
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes|McClean, Jiang, Rubin, Babbush, Neven (2019)]] — Symmetry-based error mitigation via stabilizer projection and QSE; up to $\sim 3\times$ improvement on unencoded H₂; zero additional circuit cost.

## Initial State Preparation / Overlap Analysis

- [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes|Berry, Tong, Babbush, Rubin et al. (2024)]] — MPS state preparation with $7\times$ improved unitary synthesis; two QPE filtering methods (sampling + binary search) with window function optimisation; empirical MPS overlap extrapolation for FeMoCo ($\sim 0.9$ squared overlap at $\chi = 4000$); number operator symmetry shifting halves $\lambda$; $7.3 \times 10^{10}$ Toffolis for FeMoCo at 95% confidence.
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes|Lee, Babbush, Chan et al. (2022)]] — Fe-S cluster overlaps: HF weight decays exponentially with metal centres ($\sim 10^{-7}$ for FeMoCo); ASP time varies over 8 orders of magnitude and correlates with $\mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|)$.
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes|Tubman, Mejuto-Zaera, Babbush et al. (2018)]] — Systematic study of ground-state overlap across G1 molecules, Fe-porphyrin, FeMoco, HEG, Hubbard models, and DMFT impurity Hamiltonians; shows HF overlap $\geq 0.75$ for most chemical systems; introduces ASCI-based overlap certification and sequential multi-determinant state preparation ($O(nL)$ gates, 1 ancilla).
- [[Reliably Assessing the Electronic Structure of Cytochrome P450 (Goings, Babbush, Rubin et al 2022) — Paper Notes|Goings, Babbush, Rubin et al. (2022)]] — Computational basis state overlap with DMRG ground state decays from ~1.0 (5 qubits) to ~0.6 (84 qubits) across CYP active spaces; slow enough that simple initial states suffice for QPE.
