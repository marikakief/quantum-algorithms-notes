> **Source:** Andrew M. Childs, Dmitri Maslov, Yunseong Nam, Neil J. Ross, Yuan Su, "Toward the first quantum simulation with quantum speedup," *Proceedings of the National Academy of Sciences* **115**(38), 9456–9461 (2018).
> **Links:** [arXiv:1711.10980](https://arxiv.org/abs/1711.10980) · [PNAS](https://doi.org/10.1073/pnas.1801723115)
> **Tags:** #hamiltonian-simulation #product-formulas #fault-tolerant #T-gate-count #quantum-chemistry #Trotter #resource-estimation #concrete-costs

## The computational problem

How many [[Clifford Hierarchy for Fault-Tolerant Gate Classification|fault-tolerant]] T gates does it actually take to simulate a quantum chemistry Hamiltonian on a near-future quantum computer? The paper targets the ground-state energy of a small but classically hard molecule — specifically the $\text{FeMo}$ cofactor of nitrogenase, the iron-sulfur cluster responsible for biological nitrogen fixation — and asks whether a quantum device could outperform classical computers at this task.

The Hamiltonian is expressed in second quantization over a Gaussian orbital basis. Fermions are encoded via the Jordan-Wigner transform, giving a sum of Pauli operators. The computational task is to perform phase estimation on the ground state to extract the ground-state energy to chemical accuracy ($\approx 1.6 \times 10^{-3}$ Hartree).

## What the paper does

The paper performs an end-to-end T-gate resource estimate for simulating the FeMo cofactor using a [[Product Formulas|product formula]] (Trotter-Suzuki) approach. The contributions are:

1. **Concrete Hamiltonian**: They write out the second-quantized FeMo cofactor Hamiltonian in a minimal active-space model (54 spin-orbitals, drawn from earlier quantum chemistry work).
2. **Product formula order selection**: They carefully choose the order $k$ of the Suzuki-Childs product formula and optimize the number of Trotter steps $r$ to minimize total T-gate count subject to achieving target simulation error.
3. **T-gate efficient rotation synthesis**: Instead of off-the-shelf Solovay-Kitaev, they use modern [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes|Clifford+T gate synthesis]] (the mixed Clifford+T gridsynth approach of Ross-Selinger) to compile each single-qubit rotation to within additive error $\epsilon_{\text{synth}}$, which they balance against Trotter error.
4. **Scalability analysis**: They extend the analysis to increasingly large active spaces, estimating that the crossover point where quantum computers beat classical methods is within reach of near-future hardware.

## The algorithm / construction

The simulation uses the $2k$th-order Suzuki product formula

$$e^{-iHt} \approx S_{2k}(t/r)^r$$

where each Trotter step $S_{2k}$ decomposes into exponentials of individual Pauli terms. Each Pauli rotation $e^{-i\theta P}$ is realized by a circuit with one CNOT ladder, one $R_z(\theta)$ rotation, and the reverse CNOT ladder. The rotation $R_z(\theta)$ is compiled to Clifford+T gates using approximate synthesis.

The total T-gate count for a single phase-estimation step of length $t = O(1/\epsilon)$ is

$$T_{\text{total}} = r \cdot T_{\text{step}} \cdot \log(1/\delta)$$

where $T_{\text{step}}$ scales as the number of Pauli terms in $H$ times the T-gate cost of each rotation synthesis, and $r$ grows with $k$ and the target error. The authors carefully choose $k$ and $r$ to minimize this product.

Key formula for Trotter error: the $2k$th-order formula has error

$$\|S_{2k}(t/r)^r - e^{-iHt}\| \leq \frac{(2\Lambda t)^{2k+1}}{r^{2k}}$$

where $\Lambda = \sum_j \|H_j\|$ is the 1-norm of the Hamiltonian terms.

For FeMo cofactor specifically:
- Number of spin-orbitals: $N = 54$
- Number of Pauli terms: $\Theta(N^4) \approx 10^6$
- Optimal formula order: $k = 6$ (12th-order product formula)
- Estimated T-gate count to simulate a single phase-estimation step: $\sim 10^{14}$

## Key results

- **Main estimate**: Simulating the FeMo cofactor to chemical accuracy requires approximately $10^{14}$ T gates for a single energy estimate (phase estimation). This is achievable by a fault-tolerant quantum computer but far beyond current hardware.
- **Quantum speedup regime**: For active spaces of $\sim 100$ spin-orbitals or more, the classical configuration interaction (CI) cost grows exponentially while the quantum cost (scaling as $O(N^5)$ in their Trotter approach) remains polynomial, defining a crossover regime.
- **T-gate synthesis insight**: Balancing rotation synthesis error against Trotter formula error is non-trivial. Allocating too much precision to each gate wastes T gates; too little introduces uncorrected simulation error. The optimal balance is derived explicitly.
- **High-order formulas win**: Contrary to naive intuition that higher-order formulas cost more per step, the savings in step count $r$ more than compensates, making $k=6$ optimal for this system.

## Comparison with prior work

| Approach | Gate complexity (scaling) | Concrete estimate |
|---|---|---|
| 1st-order Trotter (Lloyd 1996) | $O(\Lambda^2 t^2/\epsilon)$ | $\gg 10^{20}$ gates |
| 2nd-order Trotter | $O(\Lambda^{5/2} t^{3/2}/\epsilon^{1/2})$ | $\sim 10^{17}$ |
| Taylor series / LCU ([[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|Berry et al. 2015]]) | $O(\Lambda t \log(\Lambda t/\epsilon)/\log\log(\Lambda t/\epsilon))$ | competitive |
| **This work** (high-order Trotter) | $O(\Lambda^{1+1/2k} t^{1+1/2k}/\epsilon^{1/2k})$ | $\sim 10^{14}$ |

Compared to the [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes|truncated Taylor series]] approach and [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|quantum signal processing]], Trotter with high order $k$ is competitive in practice despite worse asymptotic scaling, because the constant factors are much smaller — there is no ancilla overhead, no LCU SELECT/PREPARE oracle, and the circuit structure maps efficiently to fault-tolerant hardware.

The paper also supersedes the earlier estimate by [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes|Kivlichan et al. 2017]] for real-space Hamiltonians, showing that molecular Hamiltonians (second-quantized, sparse Pauli structure) can be treated with lower overhead than grid Hamiltonians.

## Limits / caveats

- The $10^{14}$ T-gate estimate assumes a fault-tolerant computer operating at the logical level; accounting for error correction overhead pushes physical qubit and time requirements much higher (not estimated here).
- The initial state preparation problem — getting into the ground-state-dominated subspace — is handled by assuming an oracle that prepares a good initial state; the cost of this step is not included.
- The FeMo cofactor model uses an active space approximation; whether this captures the true chemistry is a separate question.
- Asymptotically, qubitization-based methods ([[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low-Chuang 2019]], [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes|Babbush et al. 2018]]) achieve $O(\Lambda t)$ with no dependence on $\epsilon$ in the exponent, which will dominate for very high precision requirements.
- The Trotter error bound used is loose (worst-case over time orderings); tighter [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|commutator-based bounds]] (Childs-Su 2019 and later) would reduce the required $r$ further.

## Reusable ideas

- **Error budget splitting between synthesis and formula error**: When compiling product-formula circuits to Clifford+T, split the total allowed error $\epsilon$ between Trotter error $\epsilon_T$ and per-gate synthesis error $\epsilon_S$ and optimize over the split. See trick card: [[Error Budget Splitting for Trotter-Plus-Synthesis Circuits]].
- **High-order product formulas have better practical constants**: For a fixed total simulation time and error, increasing formula order $k$ reduces the number of steps $r$ superpolynomially. The crossover from $k=2$ to $k=6$ gives roughly $10^3$ savings in steps. See: [[Suzuki Order as a Tunable Knob for Time Scaling]].
- **Pauli grouping for T-gate reduction**: The Jordan-Wigner Hamiltonian has Pauli terms that share CNOT ladders; grouping commuting terms allows ladder reuse, cutting T-gate overhead. See trick card: [[Pauli Ladder Reuse for T-Gate Reduction in Trotter Steps]].

## References within this paper

- M. Suzuki, "General theory of fractal path integrals..." (1991) — source of the $2k$th-order product formula
- N. J. Ross, P. Selinger, "Optimal ancilla-free Clifford+T approximation of z-rotations" (2016) — gridsynth rotation synthesis
- R. Babbush et al., "Exponentially more precise quantum simulation of fermions in second quantization" (2015) — [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes]]
- D. Berry et al., "Simulating Hamiltonian dynamics with a truncated Taylor series" (2015) — [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
- G. H. Low, I. L. Chuang, "Optimal Hamiltonian simulation by quantum signal processing" (2017) — [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]

## Cross-links

**Concept hubs:**
- [[Product Formulas]] — theoretical basis for all Trotter estimates here
- [[Hamiltonian simulation]] — parent concept
- [[Quantum Phase Estimation]] — outer wrapper algorithm

**Closely related paper notes:**
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]] — later work that tightens the Trotter error bound used here via commutator norms
- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]] — companion work on lattice Hamiltonians
- [[Chemical Basis of Trotter-Suzuki Errors in Quantum Chemistry Simulation (Babbush-McClean-Wecker-Aspuru-Guzik-Wiebe 2015) — Paper Notes]] — earlier empirical study of Trotter errors in quantum chemistry
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — competing approach with better asymptotic scaling
- [[Encoding Electronic Spectra in Quantum Circuits with Linear T Complexity (Babbush, Gidney et al 2018) — Paper Notes]] — qubitization-based approach that followed and beat these estimates
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — follow-on that improves the Trotter estimate further
- [[Exponentially More Precise Quantum Simulation of Fermions in Second Quantization (Babbush-Berry-Kivlichan-Wei-Love-Aspuru-Guzik 2015) — Paper Notes]] — underlying Hamiltonian representation
- [[Higher Order Decompositions of Ordered Operator Exponentials (Wiebe-Berry-Høyer-Sanders 2010) — Paper Notes]] — theory of high-order product formulas
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — asymptotically superior alternative

**Trick cards:**
- [[Error Budget Splitting for Trotter-Plus-Synthesis Circuits]]
- [[Pauli Ladder Reuse for T-Gate Reduction in Trotter Steps]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
