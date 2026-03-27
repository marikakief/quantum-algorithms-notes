> **Source:** K. Temme, T.J. Osborne, K.G. Vollbrecht, D. Poulin, F. Verstraete, *Quantum Metropolis Sampling*, Nature **471**, 87–90 (2011); arXiv:0911.3635
> **Links:** [arXiv](https://arxiv.org/abs/0911.3635) · [Nature](https://doi.org/10.1038/nature09770)
> **Tags:** #quantum-algorithm #Metropolis #Gibbs-state #thermal-state #phase-estimation #detailed-balance #Markov-chain

---

## The problem

[[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] showed quantum computers can simulate Hamiltonian dynamics. But for condensed matter, chemistry, and high-energy physics, what you usually want are *static* properties: ground states, thermal expectation values, phase diagrams. These require preparing Gibbs states $\rho_G = e^{-\beta H}/Z$. Classically, Metropolis sampling solves this by a random walk that converges to the Boltzmann distribution. But the quantum version faces three obstacles:

1. You don't know the eigenvectors of $H$ (that's what you're trying to learn)
2. Energy measurement is irreversible — but rejection requires undoing the measurement
3. Need to prove the random walk converges to the quantum Gibbs state

## What the paper does

A complete quantum generalisation of the Metropolis algorithm. Each step can be implemented in polynomial time on a quantum computer. The algorithm:

- Samples from eigenstates of $H$ with Boltzmann weights, evading the fermion sign problem
- Uses phase estimation coherently (not destructively) to learn energies
- Engineers a binary accept/reject measurement that reveals only one bit (accept or reject), minimising disturbance
- Implements rejection via a Marriott-Watrous-style iterative procedure that recovers the original state with exponentially high probability
- Proves convergence to the Gibbs state via a quantum detailed balance condition

---

## The algorithm

### One Metropolis step

**Input:** eigenstate $|\psi_i\rangle$ with known energy $E_i$.

**Step 1 (Propose):** Apply a random local unitary $C$ (drawn with $d\mu(C) = d\mu(C^\dagger)$):
$$C|\psi_i\rangle = \sum_k x_k^i |\psi_k\rangle$$

**Step 2 (Coherent energy measurement):** Run phase estimation coherently:
$$\sum_k x_k^i |\psi_k\rangle|E_i\rangle|0\rangle|0\rangle \to \sum_k x_k^i |\psi_k\rangle|E_i\rangle|E_k\rangle|0\rangle$$

**Step 3 (Metropolis gate):** Apply controlled rotation $W(E_k, E_i)$ on an ancilla qubit:
$$W: |0\rangle \mapsto \sqrt{f_k^i}|1\rangle + \sqrt{1-f_k^i}|0\rangle$$
where $f_k^i = \min(1, e^{-\beta(E_k - E_i)})$ — exactly the classical Metropolis acceptance probability.

The state is now:
$$\sum_k x_k^i \sqrt{f_k^i}|\psi_k\rangle|E_i\rangle|E_k\rangle|1\rangle + \sum_k x_k^i \sqrt{1-f_k^i}|\psi_k\rangle|E_i\rangle|E_k\rangle|0\rangle$$

**Step 4 (Measure ancilla):**
- Outcome $|1\rangle$ → **accept.** Measure energy register to get new eigenstate $|\psi_k\rangle$ with energy $E_k$. Transition probability $\propto |x_k^i|^2 f_k^i$ — matches classical Metropolis.
- Outcome $|0\rangle$ → **reject.** Must recover $|\psi_i\rangle$.

### The rejection problem (the hard part)

Classically, rejection is trivial: keep a copy of the old state. Quantumly, no-cloning forbids this. The solution: the binary measurement splits the Hilbert space into a 2D subspace (accept vs. reject). Within this subspace, iterate two projective measurements:

- $Q_\alpha$: re-run the accept/reject test (apply $U$, measure ancilla, apply $U^\dagger$)
- $P_\alpha$: check if the energy matches $E_i$ (via another phase estimation)

Alternating $Q$ and $P$ measurements, the probability of failing to recover the original state decays exponentially:
$$p_{\text{fail}}(n) \leq \frac{1}{2e(n+1)}$$

So $O(1/\varepsilon)$ iterations suffice for failure probability $< \varepsilon$. This is the [[Marriott-Watrous Iterative Rejection for Quantum Metropolis|Marriott-Watrous iterative rejection]] trick.

---

## Quantum detailed balance

The paper develops a quantum version of detailed balance. A CP map $\mathcal{E}$ satisfies quantum detailed balance w.r.t. $\sigma = \sum_i p_i |\psi_i\rangle\langle\psi_i|$ if:
$$\sqrt{p_n p_m}\langle\psi_i|\mathcal{E}(|\psi_n\rangle\langle\psi_m|)|\psi_j\rangle = \sqrt{p_i p_j}\langle\psi_m|\mathcal{E}(|\psi_j\rangle\langle\psi_i|)|\psi_n\rangle$$

This guarantees $\sigma$ is a fixed point. Combined with ergodicity (ensured by choosing $\{C\}$ as a universal gate set), the Gibbs state is the *unique* fixed point. The symmetry condition $d\mu(C) = d\mu(C^\dagger)$ is the quantum analogue of the classical symmetric proposal distribution.

---

## Key results

| Result | Statement |
|---|---|
| Fixed point | Gibbs state $\rho_G = e^{-\beta H}/Z$ (exact for perfect PE) |
| Rejection cost | $O(1/\varepsilon)$ iterations for failure prob $< \varepsilon$; exponential convergence |
| Per-step complexity | Polynomial in system size (dominated by phase estimation / [[Hamiltonian simulation]]) |
| Sign problem | Absent — samples directly from eigenbasis |
| Approximate PE error | $\|σ^* - \rho_G\|_1 \leq \varepsilon_{sg}/(1-\eta_1)$ where $\eta_1$ is ergodicity coefficient |
| Mixing time | Problem-dependent; for XX chain with transverse field, gap $\sim O(1/N)$ even at criticality |

---

## Why it matters

This fills the missing piece of Feynman's programme: Lloyd gave us dynamics, this gives us equilibrium. The quantum Metropolis algorithm is to quantum simulation what classical Metropolis is to classical statistical mechanics — the general-purpose tool for sampling static properties of interacting systems.

Concrete applications the paper sketches:
- **Hubbard model:** phase diagram as a function of filling, interaction, temperature — the central open problem in condensed matter
- **Quantum chemistry:** molecular binding energies via Born-Oppenheimer, with quantum Metropolis as the "black box" energy solver
- **Lattice gauge theory:** hadron masses via quantum link models (Kogut-Susskind Hamiltonian formulation)

The sign problem bypass is the headline for fermions. Classical quantum Monte Carlo fails for fermionic systems because the mapping produces negative statistical weights. Here, you sample in the eigenbasis directly — no sign problem.

---

## Limits / caveats

- **Mixing time is problem-dependent.** The algorithm converges in polynomial time only if the Markov chain mixes fast, which depends on the Hamiltonian and the choice of updates $\{C\}$. No general proof of rapid mixing exists (this would solve QMA-complete problems).
- **Phase estimation overhead.** Each Metropolis step requires coherent phase estimation, which itself requires [[Hamiltonian simulation]] for various times $t$. The total gate count per step scales polynomially but the constants are large.
- **Finite PE precision** introduces errors. The fixed point deviates from the true Gibbs state by $O(\beta \cdot 2^{-r})$ for $r$-bit precision. Cost grows linearly with $\beta$ (low temperature is harder).
- **State preparation not solved.** The algorithm assumes you can start from *some* state and mix to equilibrium. For gapped systems at low temperature, mixing may be exponentially slow.
- **The 5-qubit example** (2-qubit Heisenberg model) is a proof of concept, not a practical demonstration.

---

## Reusable ideas

1. [[Marriott-Watrous Iterative Rejection for Quantum Metropolis]] — recover a pre-measurement state by alternating two binary projective measurements in a 2D subspace; failure probability decays exponentially
2. [[Coherent Metropolis Accept-Reject via Phase Estimation]] — use phase estimation coherently (not destructively) to compute energy differences, then apply a controlled rotation encoding the Boltzmann acceptance probability

---

## Cross-links

### Paper notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — dynamics side of Feynman's programme; this paper completes it with equilibrium
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation, the core subroutine
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]] — eigenvalue estimation; related approach to ground state preparation
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]] — quantum chemistry application; quantum Metropolis provides the missing thermal/ground state prep
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]] — Jordan-Wigner mapping used for fermionic Hamiltonians

### Trick cards
- [[Marriott-Watrous Iterative Rejection for Quantum Metropolis]]
- [[Coherent Metropolis Accept-Reject via Phase Estimation]]
- [[Phase Kickback from Oracle Queries]] — underlying mechanism in phase estimation
- [[Parity-Counted Fermionic Sign Tracking]] — Jordan-Wigner transformation for fermionic systems
