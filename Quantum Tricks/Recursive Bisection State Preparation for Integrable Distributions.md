
> **Source:** Grover & Rudolph, arXiv:quant-ph/0208112 (2002)
> **Tags:** #trick #state-preparation #amplitude-loading #log-concave #sampling

## What it does
Prepares a quantum state $|\psi\rangle = \sum_i \sqrt{p_i}\,|i\rangle$ encoding a discretised probability distribution, using $O(\log N)$ controlled rotations instead of $O(\sqrt{N})$ amplitude amplification rounds. Works for any distribution whose CDF is efficiently computable.

## The trick

Build the state one qubit at a time, from coarse to fine resolution:

1. **Start** with a 1-qubit state: $\sqrt{p_L}|0\rangle + \sqrt{p_R}|1\rangle$, where $p_L, p_R$ are the total probabilities in the left and right halves of the domain. This is a single rotation.

2. **Refine:** Given an $m$-qubit state $|\psi_m\rangle = \sum_{i=0}^{2^m-1} \sqrt{p_i^{(m)}}|i\rangle$, add qubit $m+1$ by:
   - For each region $i$, compute $f(i) = \Pr[x \in \text{left half of region } i \mid x \in \text{region } i]$ using the classical CDF algorithm (run coherently)
   - Store $\theta_i = \arccos\sqrt{f(i)}$ in an ancilla
   - Controlled-rotate the new qubit by $\theta_i$
   - Uncompute the ancilla

3. **Repeat** until $m = n = \log_2 N$.

Each level adds one qubit and requires one call to the integration oracle + one controlled rotation. Total: $n$ rounds.

**Why it works:** At each level, the conditional split ratio $f(i)$ is just the ratio of integrals — how much of region $i$'s probability mass falls in its left sub-half. The controlled rotation maps $\sqrt{p_i^{(m)}}|i\rangle$ to $\sqrt{\alpha_i}|i\rangle|0\rangle + \sqrt{\beta_i}|i\rangle|1\rangle$ with $\alpha_i + \beta_i = p_i^{(m)}$, which is exactly the finer discretisation.

## When to reach for it
- Loading Gaussian, exponential, Poisson, or other log-concave distributions into superposition
- Quantum Monte Carlo: preparing the risk-neutral distribution for derivative pricing
- Any amplitude estimation application where the input distribution has known, integrable form
- Non-uniform priors for [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|quantum search]]

## Complexity
$O(n)$ controlled rotations, each conditioned on $m$ qubits. Per rotation: one coherent evaluation of $\int_a^b p(x)\,dx$ + one arcsine computation + controlled $R_y$. Total depth is $O(n \cdot C_{\text{int}})$ where $C_{\text{int}}$ is the cost of the coherent integration.

## Caveat
- Only works when the CDF is efficiently computable. For arbitrary unstructured distributions, you need black-box methods ([[Inequality-Test Amplitude Transduction|inequality-test]] or [[Comparator-Based Direct Amplitude Sampling from Fixed-Point Coefficients|comparator-based]]) at $O(\sqrt{N})$ cost.
- Each rotation requires computing $\arccos\sqrt{f(i)}$ on a quantum computer — this is quantum arithmetic, and it's not cheap. [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes|Sanders-Low-Scherer-Berry (2019)]] addressed this cost in the black-box setting, but for Grover-Rudolph you still pay it (though only $O(n)$ times rather than $O(\sqrt{N})$ times).
- For multivariate distributions, the bisection tree grows as $O(dn)$ for $d$ dimensions.
- **No QMC speedup with numerical integration.** Herbert (2021, [arXiv:2101.02240](https://arxiv.org/abs/2101.02240)) showed that if the CDF integrals in the bisection steps are computed numerically (by sampling), the state-preparation error propagates into mean estimates at $\tilde{\Omega}(1/\hat{\epsilon}^2)$ — matching classical Monte Carlo. The quadratic speedup from [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes|quantum Monte Carlo]] only survives if the CDF is analytically computable or if a complex function is applied before the expectation.

## Related notes
- [[Creating Superpositions That Correspond to Efficiently Integrable Probability Distributions (Grover-Rudolph 2002) — Paper Notes]]
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]]
- [[Inequality-Test Amplitude Transduction]]
- [[Quantum Computational Finance — Monte Carlo Pricing of Financial Derivatives (Rebentrost-Gupt-Bromley 2018) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
