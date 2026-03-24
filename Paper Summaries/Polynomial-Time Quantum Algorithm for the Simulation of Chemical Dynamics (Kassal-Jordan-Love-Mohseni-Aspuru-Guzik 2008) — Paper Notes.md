> **Source:** Ivan Kassal, Stephen P. Jordan, Peter J. Love, Masoud Mohseni, Alán Aspuru-Guzik, *Polynomial-time quantum algorithm for the simulation of chemical dynamics*, Proc. Natl. Acad. Sci. **105**, 18681 (2008); arXiv:0801.2986
> **Links:** [arXiv](https://arxiv.org/abs/0801.2986) · [PNAS](https://doi.org/10.1073/pnas.0808245105)
> **Tags:** #quantum-simulation #chemistry #split-operator #real-space #phase-kickback #Born-Oppenheimer

---

## The problem

Simulating chemical reaction dynamics — time-evolving the full quantum state of nuclei and electrons — on a classical computer costs exponentially in the number of particles. The Born-Oppenheimer approximation helps classically but breaks down for non-adiabatic processes (strong-field ionisation, conical intersections, fragmentation). Can a quantum computer do this in polynomial time?

## What the paper does

Yes. A complete quantum algorithm for chemical dynamics that:

1. Propagates the full electron-nuclear wavefunction on a real-space grid using the split-operator method
2. Evaluates the Coulomb potential in $O(B^2 m^2)$ time ($B$ particles, $m$ bits of precision)
3. Shows that **skipping the Born-Oppenheimer approximation is actually faster** on a quantum computer for reactions with more than ~4 atoms
4. Gives efficient recipes for state preparation (Gaussian wavepackets, harmonic oscillator eigenstates, antisymmetrised electronic states)
5. Shows how to extract chemically useful observables: reaction probabilities, thermal rate constants, state-to-state transition probabilities

The headline: **~100 qubits could outperform classical computers** for chemical dynamics. A 300-qubit machine could simulate the H + H₂ reaction with full electron-nuclear dynamics.

---

## The algorithm

### Real-space grid simulation (Zalka-Wiesner improved)

Store the $d$-dimensional wavefunction on a grid of $2^{nd}$ points using $nd$ qubits ($n$ qubits per degree of freedom, $d = 3B - 6$ degrees of freedom for $B$ particles). The spatial resolution is exponential in qubit count.

Time-evolve via split-operator (first-order Trotter):

$$\hat{U}(\delta t) \approx \text{QFT} \cdot e^{-iT(p)\delta t} \cdot \text{QFT}^\dagger \cdot e^{-iV(x)\delta t}$$

Both $e^{-iV(x)\delta t}$ and $e^{-iT(p)\delta t}$ are diagonal in their respective bases (position and momentum). Switch between bases with the QFT.

### Applying diagonal unitaries via phase kickback

This is the key implementation detail. To apply $e^{-iV(x)\delta t}$:

1. Prepare an ancilla register in state $|1\rangle_m$ ($m$ qubits for precision)
2. Inverse-QFT the ancilla → Fourier eigenstate $\frac{1}{\sqrt{M}} \sum_y e^{2\pi i y/M} |y\rangle$
3. Compute the potential: $|x\rangle |y\rangle \to |x\rangle |y \oplus V(x)\rangle$
4. The Fourier eigenstate is an eigenstate of addition mod $M$ with eigenvalue $e^{-2\pi i V(x)/M}$

So the potential value kicks back as a phase — exactly [[Fourier-Eigenstate Kickback for Arithmetic Oracles|Fourier-eigenstate kickback]]. The ancilla decouples after each application. Choose $\delta t = 2\pi/M$.

Same trick for kinetic energy in momentum space: compute $T(p) = p^2/2m$, add to the Fourier-eigenstate ancilla.

**Improvement over Zalka:** No need to uncompute the potential — the ancilla state is restored automatically by the eigenstate property. This halves the gate count.

### Coulomb potential evaluation

For $B$ charged particles, the Coulomb potential has $O(B^2)$ pairwise terms $q_i q_j / r_{ij}$. Each term requires:
- Computing $r_{ij}$ from coordinates: subtraction + Newton's method for $1/r$
- $O(m^3)$ gates per pair (using space-efficient arithmetic)

Total: $(\frac{75}{4}m^3 + \frac{51}{2}m^2)$ gates per particle pair.

Qubit count: $n(3B - 6) + 4m$ total ($n$ qubits per spatial degree of freedom, $4m$ ancilla qubits for the potential circuit).

---

## The Born-Oppenheimer surprise

On a classical computer, the Born-Oppenheimer approximation is essential — computing the full electron-nuclear dynamics is impractical. On a quantum computer, the situation inverts:

- **BO approach:** Pre-compute potential energy surfaces, then propagate nuclei. But storing/interpolating the PES requires $K^{3B-6}$ points (degree-$K$ polynomial along each of $3B-6$ dimensions) — exponential in $B$.
- **Full dynamics:** Evaluate the Coulomb potential directly in $O(B^2 m^2)$ — polynomial in $B$.

For $K \geq 15$ (needed for 0.1% accuracy), the crossover is at ~4 atoms. Beyond that, the diabatic treatment is both **more accurate** and **faster**.

The intuition: classically, memory is the bottleneck (exponential wavefunction), so pre-computing the PES is cheap relative to storing $\psi$. Quantumly, the wavefunction fits in $O(nB)$ qubits, but evaluating a complicated fitted PES is expensive. Better to just compute Coulomb on the fly.

---

## State preparation

Three approaches, all polynomial:

1. **Gaussian wavepackets and harmonic oscillator states** — efficiently preparable since they're products of efficiently integrable functions (Zalka's result)
2. **Electronic states** — expand in Gaussian-type orbitals, occupy according to electronic structure calculation (using their earlier [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|quantum chemistry algorithm]]), antisymmetrise via Abrams-Lloyd
3. **Thermal states** — prepare superposition $\sum_{\zeta,E} \Gamma(E,T) |\zeta, E\rangle |\phi_0(\zeta,E)\rangle$ where $\Gamma$ is the Boltzmann weight; this is efficient because the Boltzmann factor is exponential in energy

---

## Measurement

Three readout schemes for extracting chemistry:

| Observable | Method |
|---|---|
| **Reaction probability** | Divide real space into reactant/product regions; construct point-location circuit $R(x)$; measure ancilla → probability of being in product region |
| **Thermal rate constant** | Propagate the thermal mixed state; measure reaction probability → $C^2 k(T)$ directly |
| **State-to-state transition probabilities** | Project final state onto product eigenstates; probability = $|\langle \text{product} | \psi(t) \rangle|^2$ |

All can be improved to $1/M$ precision (vs. $1/\sqrt{M}$) using amplitude estimation.

---

## Resource estimates

| System | Particles | Qubits | Gates/timestep |
|---|---|---|---|
| H atom (nucleus + electron) | 2 | ~30 | ~20k |
| He atom | 3 | ~50 | ~80k |
| Li atom | 4 | ~70 | ~180k |
| H + H₂ reaction | 6 | ~130 | ~600k |
| O atom | 9 | ~220 | ~2M |

Using $n = 10$ qubits per degree of freedom (grid of $2^{30}$ points) and $m = 10$ bits of potential precision.

---

## Key results

| Result | Statement |
|---|---|
| Complexity | $O(B^2 m^2)$ gates per timestep for $B$ Coulomb particles at $m$-bit precision |
| Qubits | $n(3B-6) + 4m$ total |
| BO crossover | Full dynamics faster than BO for $> 4$ atoms ($K \geq 15$) |
| 100-qubit threshold | Can outperform classical computers for Li-scale electron dynamics |
| Measurement precision | $1/M$ scaling via amplitude estimation |

---

## Limits / caveats

- **Grid-based approach** requires enough grid points to resolve the wavefunction. For high-dimensional problems, $n$ must be large enough per dimension — the $2^{nd}$ grid points are stored implicitly but the time-step cost grows with precision.
- **First-order Trotter** error $O(\delta t^2)$ means many timesteps needed for long simulations. Higher-order Trotter helps but adds gates.
- **State preparation** of arbitrary correlated states is still hard. The efficient methods work for states expressible as products of Gaussians/polynomials. Strongly correlated initial states may need adiabatic preparation.
- **Electronic timestep** is ~1000× shorter than nuclear timestep — the full dynamics simulation needs many more timesteps than a BO simulation.
- The 100-qubit estimate assumes a fault-tolerant machine. Error correction overhead would increase the physical qubit count significantly.

---

## Reusable ideas

1. [[Fourier-Eigenstate Kickback for Arithmetic Oracles]] — already in the vault; this paper is a major application
2. [[Split-Operator Real-Space Simulation on a Quantum Computer]] — alternate between position and momentum bases via QFT, applying diagonal unitaries in each

---

## References within this paper

- Zalka (1998) and Wiesner (1996) — the original real-space quantum simulation algorithm that this paper improves (halving gate count by avoiding uncomputation via Fourier-eigenstate kickback)
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — foundational result that quantum computers can simulate local Hamiltonians in polynomial time
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes|Abrams & Lloyd (1997)]] — antisymmetrisation method for preparing multi-electron wavefunctions, used directly in the state-preparation section
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes|Aspuru-Guzik et al. (2005)]] — predecessor from the same group; quantum algorithm for static electronic structure (FCI eigenvalues), referenced for orbital occupation numbers and on-the-fly BO surface computation
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes|Grover & Rudolph (2002)]] — efficient preparation of quantum states from integrable distributions; used for Gaussian wavepacket and harmonic oscillator state prep
- Lidar & Wang (2005) — earlier application of Zalka-Wiesner to thermal rate constants via flux-flux correlation functions
- Steane (2007) — design for a 300-qubit error-corrected trapped-ion computer; the resource estimates in Figs. 2–3 are benchmarked against this proposal
- Brassard, Høyer, Mosca, Tapp (2002) — amplitude estimation, used to improve measurement precision from $1/\sqrt{M}$ to $1/M$
- Suzuki (1990) — higher-order Trotter decompositions referenced for improving the first-order split-operator error
- BKMP2 surface for H₃ (Boothroyd et al. 1996) — the benchmark PES used to estimate that $K \geq 15$ is needed for 0.1% accuracy in the BO crossover argument

---

## Cross-links

### Paper notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — the foundational result on quantum simulation of local Hamiltonians
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] — predecessor from the same group; static chemistry (eigenvalues), this paper does dynamics
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]] — co-author Jordan; uses the same Fourier-eigenstate kickback mechanism
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]] — antisymmetrisation method used for electronic state prep

### Trick cards
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]] — the mechanism for applying diagonal unitaries
- [[Split-Operator Real-Space Simulation on a Quantum Computer]]
- [[Adiabatic State Preparation for Chemistry]] — alternative state prep method referenced
- [[Product-Formula Time-Slicing for Local Hamiltonians]] — the Trotter decomposition underlying the split-operator method
