
> **Source:** Wiebe, Granade, Ferrie, Cory, arXiv:1311.5269 (PRA 89, 042314, 2014)
> **Tags:** #trick #hamiltonian-learning #noise-robustness #depolarizing #Bayesian-inference #IQLE

## What it does
Shows that depolarizing noise on the experimental device degrades Bayesian Hamiltonian learning **gracefully** — shrinking the per-experiment information gain by factor $(1 - \mathcal{N})$ rather than destroying inference entirely — so learning still converges exponentially, just slower.

## The trick

If the untrusted device applies depolarizing noise with visibility $\mathcal{N}$ (meaning $\mathcal{N} = 0$ is noiseless, $\mathcal{N} = 1$ is fully depolarized), the output state before the IQLE inversion step is:

$$\rho_\mathcal{N}(t) = (1 - \mathcal{N})\,|\psi(t)\rangle\langle\psi(t)| + \frac{\mathcal{N}}{2^n}\,I.$$

For an IQLE experiment targeting outcome $D = |0\rangle$ (Loschmidt echo), the measured likelihood becomes:

$$\Pr(0|\mathbf{x};\mathcal{N}) = (1-\mathcal{N})\,\frac{1 + F(t,\mathbf{x},\mathbf{x}^-)}{2} + \frac{\mathcal{N}}{2^n}$$

where $F = |\langle\psi_0|e^{+iH(\mathbf{x}^-)t}e^{-iH(\mathbf{x})t}|\psi_0\rangle|^2$ is the echo fidelity.

**The key observation:** the structure is $(1-\mathcal{N}) \times$ (noiseless likelihood) plus a constant shift of $\mathcal{N}/2^n$. Bayesian updates from comparing likelihoods between hypotheses depend on likelihood *ratios* — the constant shift is the same for all hypotheses and cancels. The contracting factor $(1-\mathcal{N})$ reduces the magnitude of each weight update by $(1-\mathcal{N})$, which translates to a learning rate reduction:

$$\gamma(\mathcal{N}) \approx (1-\mathcal{N})\,\gamma_0.$$

So $N(\delta, \mathcal{N}) \approx N(\delta, 0)/(1-\mathcal{N})$ experiments are needed to reach quadratic loss $\delta$. For $\mathcal{N} = 0.5$, you need roughly twice as many experiments — not an exponential blowup.

## When to reach for it

- Assessing the practical viability of Hamiltonian learning on a noisy intermediate-scale device.
- Understanding why certain Bayesian inference pipelines degrade gracefully under noise (vs. approaches that break catastrophically).
- Designing noise budgets: given a noise tolerance $\mathcal{N}$, budget the extra experiments needed.
- More broadly: any Bayesian inference task where the likelihood can be written as $(1-\mathcal{N}) \times f(\mathbf{x}) + \mathcal{N} \times \text{const}$ — the same graceful degradation applies.

## Complexity

- Noiseless experiments needed: $O(d \log 1/\delta)$.
- With depolarizing noise $\mathcal{N}$: $O(d \log(1/\delta) / (1-\mathcal{N}))$.
- Overhead is polynomial in $1/(1-\mathcal{N})$, not exponential.
- Validated numerically for $\mathcal{N} \in \{0.1, 0.3, 0.5\}$ on random 4-qubit Ising Hamiltonians.

## Caveat

- Analysis assumes **depolarizing** noise specifically. For correlated, non-Markovian, or coherent noise (systematic miscalibration), the analysis must be redone — the nice linear structure may not hold.
- If the *trusted simulator* (not just the untrusted device) is noisy, the argument must be extended. The paper addresses this via superoperator modeling rather than analytic bounds (see [[Superoperator Noise Incorporation for Robust QHL]]).
- At high noise $\mathcal{N} \to 1$, the experiment ceases to be informative (likelihoods become uniform); the factor $(1-\mathcal{N})^{-1}$ diverges, so there is an effective noise threshold beyond which finite experiments are not enough.

## Related notes
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]]
- [[Superoperator Noise Incorporation for Robust QHL]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]]
