---
aliases:
  - QPE
  - phase estimation
  - quantum phase estimation
---

# Quantum Phase Estimation

**Concept hub** — the workhorse subroutine that converts eigenvalue information into classical bits.

---

## What it is

Given a unitary $U$ with eigenvector $|\psi\rangle$ and eigenvalue $e^{2\pi i\theta}$, quantum phase estimation (QPE) extracts an $n$-bit approximation of $\theta$ with high probability.

The textbook version (Kitaev, 1995; Cleve-Ekert-Macchiavello-Mosca, 1998) uses:
- An $n$-qubit ancilla register prepared in $|+\rangle^{\otimes n}$
- Controlled powers $U, U^2, U^4, \ldots, U^{2^{n-1}}$
- An inverse quantum Fourier transform (QFT$^\dagger$)

**Total cost:** $2^n - 1$ applications of controlled-$U$ for $n$ bits of precision, giving Heisenberg-limited scaling $O(1/\varepsilon)$.

For Hamiltonian eigenvalue problems, $U = e^{-iHt}$ and $\theta = Et/2\pi$, so QPE converts Hamiltonian simulation into energy estimation.

---

## Variants in this vault

### Textbook / multi-ancilla QPE
The standard construction requiring $n$ ancilla qubits and coherent controlled unitaries across all of them. Used in most fault-tolerant resource estimates.

- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — original formulation
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd (1999)]] — first application to quantum chemistry eigenvalues
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes|Cleve-Ekert-Macchiavello-Mosca (1998)]] — textbook formulation via QFT

### Iterative / single-ancilla QPE
One ancilla qubit, one controlled-$U^{2^k}$ at a time, classical feedforward between rounds. Same total query count but much lower qubit overhead.

- [[Iterative Phase Estimation (Kitaev)]] — the basic single-ancilla protocol
- [[Efficient Bayesian Phase Estimation (Wiebe-Granade 2016) — Paper Notes|Wiebe-Granade (2016)]] — RFPE: Bayesian posterior update with rejection sampling; noise-tolerant
- [[Bayesian Restart Protocol for Phase Estimation]] — tail failure recovery for Bayesian QPE
- [[Sign-Conditioned Phase Estimation (Ito-Wiebe Trick)]] — halves the number of $U$ applications by using $U$ or $U^\dagger$ conditioned on the control

### Robust / SPAM-tolerant QPE
Protocols that maintain Heisenberg scaling even under state preparation and measurement errors.

- [[Robust Calibration of a Universal Single-Qubit Gate Set via Robust Phase Estimation (Kimmel-Low-Yoder 2015) — Paper Notes|Kimmel-Low-Yoder (2015)]] — non-adaptive RPE; additive error threshold $1/\sqrt{8}$
- [[SPAM-Tolerant Phase Estimation via Additive Error Bounds]] — the core trick: SPAM errors are bounded additive perturbations
- [[Geometric Error Probability Bound for Non-Adaptive Phase Estimation]] — tight $1/(\sqrt{2\pi M}\cdot 2^M)$ bound

### Eigenstate filtering (QPE alternatives)
Replace QPE with polynomial eigenvalue filters — no ancilla register, no QFT.

- [[Optimal Polynomial Based Quantum Eigenstate Filtering (Dong-An-Lin 2020) — Paper Notes|Dong-An-Lin (2020)]] — optimal Chebyshev minimax filter
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes|Lin-Tong (2020)]] — ground state preparation without QPE
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes|Dong-Lin-Tong (2022)]] — QET-U: eigenvalue transformation via Hamiltonian evolution
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes|Lin-Tong (2022)]] — energy estimation with Heisenberg scaling via binary search + eigenstate filtering

### Walk-based / qubitised phase estimation
QPE on quantum walks rather than on $e^{-iHt}$ directly — avoids Hamiltonian simulation entirely.

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang (2019)]] — qubitised walk has eigenvalues $e^{\pm i\arccos(E/\alpha)}$
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush-Gidney et al. (2018)]] — first chemistry resource estimate using qubitised QPE
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes|Lee-Berry-Babbush et al. (2021)]] — THC + qubitised QPE

### Spectrum amplification
Amplify the spectral gap before QPE to reduce query complexity.

- [[Hamiltonian Simulation by Uniform Spectral Amplification (Low-Chuang 2017) — Paper Notes|Low-Chuang (2017)]] — uniform spectral amplification via QSP
- [[Quantum Simulation with Sum-of-Squares Spectral Amplification (King, Low, Babbush, Somma, Rubin 2025) — Paper Notes|King-Low-Babbush-Somma-Rubin (2025)]] — SOSSA: quadratic improvement for clustered spectra
- [[Adaptive Spectral-Amplified Energy Estimation]] — adaptive binary search removing need for prior $\Delta$ knowledge

---

## Trick cards

- [[Iterative Phase Estimation (Kitaev)]]
- [[Bayesian Restart Protocol for Phase Estimation]]
- [[Sign-Conditioned Phase Estimation (Ito-Wiebe Trick)]]
- [[SPAM-Tolerant Phase Estimation via Additive Error Bounds]]
- [[Geometric Error Probability Bound for Non-Adaptive Phase Estimation]]
- [[Gapped Phase Estimation]]
- [[Recursive Phase Estimation for Qubit-Efficient Readout]]
- [[Semiclassical Phase Estimation via QSVT]]
- [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]]
- [[Qubitized Dyson Series for Phase Estimation]]
- [[Majority Vote Bit Refinement for Phase Estimation]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Eigenvalue Multiplication via Phase Estimation]]
- [[Coherent Metropolis Accept-Reject via Phase Estimation]]
- [[Hamiltonian-to-Projection via Phase Estimation]]
- [[Adaptive Spectral-Amplified Energy Estimation]]
- [[Chebyshev-State Phase Estimation for Real-Spectrum Non-Normal Inputs]]
- [[PGH Adaptation for Single-Parameter Phase Estimation]]
- [[Randomized Error Budget for Phase Estimation]]
- [[IPEA with Constant-Depth Controlled Powers]]
- [[Composite Sequence for Off-Resonance Error Amplification]]

---

## Key complexity facts

| Variant | Ancilla qubits | Queries to $U$ | Notes |
|---------|---------------|----------------|-------|
| Textbook QPE | $n = \lceil\log_2(1/\varepsilon)\rceil + O(1)$ | $O(1/\varepsilon)$ | Coherent; requires QFT |
| Iterative (Kitaev) | 1 | $O(1/\varepsilon)$ | Classical feedforward |
| Bayesian (RFPE) | 1 | $O(\log(1/\varepsilon))$ experiments | Heisenberg-limited with restarts |
| Eigenstate filtering | 0 | $O(\Delta^{-1}\log(1/\varepsilon))$ | Needs spectral gap $\Delta$; no ancilla |
| Qubitised walk QPE | $n$ | $O(\alpha/\varepsilon)$ | $\alpha$ = 1-norm of block encoding |

---

## Relationship to other concepts

QPE sits at the interface of several major frameworks:
- **[[Hamiltonian simulation]]** provides the controlled-$U = e^{-iHt}$ needed for QPE on Hamiltonians
- **[[Product Formula (Trotter-Suzuki)|Product formulas]]** and **[[Truncated Taylor Series Simulation|Taylor series]]** are the main methods for implementing $e^{-iHt}$ inside QPE
- **[[Qubitization (Quantum Walk for Spectral Encoding)|Qubitization]]** bypasses Hamiltonian simulation by encoding eigenvalues directly in a quantum walk
- **[[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]]** generalises QPE: any bounded polynomial of eigenvalues, not just bit extraction
- **[[Fixed-Point Quantum Search with an Optimal Number of Queries (Yoder-Low-Chuang 2014) — Paper Notes|Fixed-point amplification]]** and **[[Chebyshev Polynomial Design for Fixed-Point Amplitude Amplification|Chebyshev polynomial design]]** amplify the overlap with the target eigenstate before or after QPE
