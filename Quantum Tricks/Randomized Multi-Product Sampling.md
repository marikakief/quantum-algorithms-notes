**Source:** [[Randomizing Multi-Product Formulas for Hamiltonian Simulation (Faehrmann-Steudtner-Kueng-Kieferová-Eisert 2022) — Paper Notes]]
**Tags:** #trick #hamiltonian-simulation #randomization #multi-product #importance-sampling

---

## The trick

A multi-[[Product Formulas]] (MPF) approximates $U(T)$ as a linear combination of product-formula unitaries:
$$
U_{\mathrm{MPF}}(T)=\sum_q C_q V_q,
\qquad
\Xi=\sum_q |C_q|,
$$
where each $V_q=S_{2\chi}(T/\ell_q)^{\ell_q}$ is a standard Suzuki product formula at step count $\ell_q$. The coefficients $C_q$ may be signed or complex.

**Implementing this coherently** requires LCU: PREPARE + SELECT oracles, ancilla registers, and usually oblivious amplitude amplification. The randomized-MPF trick avoids coherent LCU when the goal is estimating observables.

For an observable $O$ and input state $\rho$, the MPF expectation is
$$
\mu=\mathrm{tr}(O U_{\mathrm{MPF}}\rho U_{\mathrm{MPF}}^\dagger)
=
\sum_{q,q'} C_q C_{q'}^*
\mathrm{tr}(O V_q\rho V_{q'}^\dagger).
$$
The cross terms are essential. A single-index average over $V_q\rho V_q^\dagger$ estimates an incoherent mixture, not the coherent MPF expectation.

The unbiased estimator is:

1. Set $p_q=|C_q|/\Xi$.
2. Sample an independent pair $(q,q')\sim p_q p_{q'}$.
3. Estimate the interference term
   $x_{q,q'}=\mathrm{tr}(O V_q\rho V_{q'}^\dagger)$
   with a Hadamard-test-style controlled pair of circuits.
4. Output
   $$
   \hat{\mu}
   =
   \Xi^2
   \frac{C_q}{|C_q|}
   \frac{C_{q'}^*}{|C_{q'}|}
   \hat{x}_{q,q'}.
   $$
5. Average the sampled estimates.

Then $\mathbb{E}[\hat{\mu}]=\mu$. For bounded observables, the estimator magnitude is controlled by $\Xi^2\|O\|$, so the shot/sample complexity for additive precision $\varepsilon$ has the characteristic scaling
$$
N=O(\|O\|^2\Xi^4\log(1/\delta)/\varepsilon^2).
$$

---

## When to use it

- You want observable estimation, not a full output state.
- You want MPF error suppression without coherent LCU ancilla registers.
- $\Xi$ is small. Use matching MPF or well-conditioned coefficient choices to keep it controlled; see [[Resolution Factor Minimization for Multi-Product Formulas]].
- Controlled versions of $V_q$ and $V_{q'}$ are acceptable for the observable estimator.

---

## What it doesn't do

- It does not reconstruct the evolved quantum state.
- It is not the same as sampling one product formula and measuring $O$ after it. That estimates an incoherent mixture and drops MPF interference terms.
- It still needs controlled circuit structure to estimate $V_{q'}^\dagger O V_q$-type cross terms.
- The $\Xi^4/\varepsilon^2$ scaling is the Monte Carlo variance cost of rescaling by $\Xi^2$ per sampled pair.

---

## Generality

The trick extends beyond [[Hamiltonian simulation]]. For any task where
$$
U_{\mathrm{apx}}=\sum_q C_q U_q
$$
is only needed inside observable expectations, sample coefficient pairs, estimate cross terms $\mathrm{tr}(O U_q\rho U_{q'}^\dagger)$, rescale by the phase/sign of $C_q C_{q'}^*$, and average.

This is an incoherent observable-estimation analogue of LCU, not an incoherent way to prepare the LCU output state.

---

## Related tricks

- [[Resolution Factor Minimization for Multi-Product Formulas]] — how to keep $\Xi$ small
- [[Importance Sampling over Hamiltonian Terms]] — related importance sampling for qDRIFT-style simulation
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]] — first-order randomized channel approach
- [[Richardson Extrapolation over Randomized Step Sizes]] — related incoherent extrapolation idea, but with a different estimator
- [[Linear Combination of Unitaries (LCU)]] — the coherent version that this replaces for state preparation
- [[Oblivious Amplitude Amplification (Robust)]] — needed for coherent LCU; avoided here for observable estimation
- [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes]] — the conditioning result ($\|\vec{a}\|_1 \in O(\log m)$) that makes randomized MPFs practical
- [[Chebyshev-Node Conditioning for Multiproduct Formulas]] — how to keep $\Xi$ logarithmic via Chebyshev nodes
