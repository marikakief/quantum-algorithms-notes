
**Source:** [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]  
**Tags:** #trick #hamiltonian-simulation #randomization #multi-product #importance-sampling

---

## The trick

A multi-[[Product Formulas]] (MPF) approximates $U(T)$ as a linear combination of product-formula unitaries:
$$U(T) \approx \frac{1}{\Xi} \sum_q C_q V_q$$
where $\Xi = \sum_q |C_q|$ and each $V_q = S_{2\chi}(T/\ell_q)^{\ell_q}$ is a standard Suzuki product formula at step count $\ell_q$.

**Implementing this coherently** requires LCU: PREPARE + SELECT oracles, $O(\log K)$ ancilla qubits, oblivious amplitude amplification. Heavy.

**The trick:** Don't implement it coherently. Instead:

1. Set $p_q = |C_q| / \Xi$ (importance sampling probabilities).
2. For each of $N$ shots: sample $q \sim p_q$, run $V_q$, measure $O$ via Hadamard test, record $\hat{o} = \mathrm{sgn}(C_q) \cdot \Xi^2 \cdot \langle O \rangle_{V_q}$.
3. Average $N$ samples to estimate $\mathrm{tr}(O \, U(T)\rho U(T)^\dagger)$.

The estimator is unbiased for the observable expectation. Shot count for $\varepsilon$-precision with failure probability $\delta$:
$$N \geq \frac{2\|O\|^2 \ln(2/\delta) \, \Xi^4}{\varepsilon^2}$$

The single ancilla qubit is used for the Hadamard test (controlled-$V_q$), not for LCU.

---

## When to use it

- You want observable estimation (not full state evolution) from a product-formula circuit.
- You want exponential error suppression (beyond first-order qDRIFT) without ancilla registers.
- $\Xi$ is small — ideally $\Xi \lesssim 2$. Use matching MPF or closed-form MPF to achieve this (see [[Resolution Factor Minimization for Multi-Product Formulas]]).
- Near-term setting where controlled gates are acceptable but large ancilla registers are not.

---

## What it doesn't do

- Cannot reconstruct the evolved quantum state — only estimates observable expectation values.
- Still needs one controlled-$V_q$ gate (Hadamard test). Watson 2025's qFLO eliminates even this.
- The $\Xi^4/\varepsilon^2$ shot count is unavoidable: classical Monte Carlo variance requires $1/\varepsilon^2$, and the $\Xi^4$ factor comes from the signed linear combination and the rescaling.

---

## Generality

The trick extends beyond [[Hamiltonian simulation]]. For any task where you need $\mathrm{tr}(O \sum_q C_q U_q \rho U_q^\dagger)$ and each $U_q$ is easy to implement:
- form probabilities $p_q = |C_q|/\Xi$,
- sample and rescale,
- average.

Cost is $O(\Xi^4/\varepsilon^2)$ shots. The linear combination of unitaries is evaluated classically for observables, not quantumly for states. This is the incoherent analogue of LCU.

---

## Related tricks

- [[Resolution Factor Minimization for Multi-Product Formulas]] — how to keep $\Xi$ small
- [[Importance Sampling over Hamiltonian Terms]] — related importance sampling for qDRIFT-style simulation
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]] — first-order version (qDRIFT)
- [[Richardson Extrapolation over Randomized Step Sizes]] — Watson's incoherent analogue (no Hadamard test)
- [[Linear Combination of Unitaries (LCU)]] — the coherent version that this replaces
- [[Oblivious Amplitude Amplification (Robust)]] — needed for coherent LCU; avoided here
- [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes]] — the conditioning result ($\|\vec{a}\|_1 \in O(\log m)$) that makes randomised MPFs practical
- [[Chebyshev-Node Conditioning for Multiproduct Formulas]] — how to keep $\Xi$ logarithmic via Chebyshev nodes
