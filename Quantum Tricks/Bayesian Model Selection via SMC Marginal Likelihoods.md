
> **Source:** Wiebe, Granade, Ferrie, Cory, arXiv:1311.5269 (PRA 89, 042314, 2014)
> **Tags:** #trick #bayesian-inference #model-selection #SMC #hamiltonian-learning #Bayes-factor

## What it does
Performs Bayesian model selection between competing Hamiltonian families essentially for free — the marginal likelihood needed for Bayes factors is already computed as the normalisation constant during each [[Sequential Monte Carlo for Quantum Parameter Estimation|SMC]] weight update.

## The trick

**The quantity we need:** the Bayes factor between models $\mathcal{M}_1$ and $\mathcal{M}_2$ is

$$B_{12} = \frac{\Pr(\mathbf{D}|\mathcal{M}_1)}{\Pr(\mathbf{D}|\mathcal{M}_2)}$$

where $\Pr(\mathbf{D}|\mathcal{M}) = \int \Pr(\mathbf{D}|\mathbf{x})\,\Pr(\mathbf{x}|\mathcal{M})\,d\mathbf{x}$ is the **marginal likelihood** — an integral over all parameters that is typically expensive to compute.

**The free lunch:** During SMC Bayesian inference (Algorithm 1 in [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes|the paper]]), after each experiment the weight update produces a normalisation constant:

$$Z_i = \sum_{j=1}^{N_p} w_j^{(i-1)}\,\Pr(D_i|\mathbf{x}_j).$$

This $Z_i$ is exactly the one-step marginal likelihood $\Pr(D_i|\mathbf{D}_{1:i-1}, \mathcal{M})$ (by definition of the Bayesian update). The cumulative marginal likelihood over all experiments is:

$$\Pr(\mathbf{D}|\mathcal{M}) = \prod_{i=1}^{N_\mathrm{exp}} Z_i.$$

**Algorithm:** Run two independent SMC particle filters, one for each model $\mathcal{M}_1$ and $\mathcal{M}_2$, with their respective parameter spaces and particle sets. At each step, compute $Z_i^{(1)}$ and $Z_i^{(2)}$. The running log-Bayes factor is:

$$\log B_{12}(N) = \sum_{i=1}^{N} \log Z_i^{(1)} - \sum_{i=1}^{N} \log Z_i^{(2)}.$$

No extra experiments are needed; the same IQLE measurements used for parameter inference feed directly into model selection. Total overhead: running a second particle filter (memory and CPU cost, not extra experiments on the quantum device).

**Model selection threshold (Jeffreys scale):**
- $|B_{12}| \in [1, 3]$: barely worth mentioning
- $|B_{12}| \in [3, 10]$: substantial
- $|B_{12}| \in [10, 100]$: strong
- $|B_{12}| > 100$: decisive

Numerically (paper Figures 10–11): Bayes factors exceed $10^{120}$ by $N \approx 200$ experiments — decisive within the first few minutes of runtime.

## When to reach for it

- Comparing two parametric Hamiltonian models (e.g., Ising vs. Heisenberg; full vs. reduced interaction graph).
- Deciding whether a newly proposed coupling term is present in an unknown device.
- Any Bayesian inference task with multiple model families where you're already using SMC — model selection comes for free once you save the $Z_i$ products.
- Overfitting detection: if model $\mathcal{M}_2$ has more parameters than $\mathcal{M}_1$ but does not fit better, $B_{12}$ will favour $\mathcal{M}_1$ automatically (Occam's razor is baked into the Bayesian framework).

## Complexity

- Extra experimental cost: **zero** — same measurements used for parameter inference.
- Extra computational cost: $O(N_p)$ per experiment (running a second particle filter), same as the first.
- Memory: $2N_p$ particles instead of $N_p$.
- Scales to $K$ models with $O(K \cdot N_p)$ cost.

## Caveat

- **Model selection, not model discovery.** Bayes factors compare within a provided menu of model classes. If the true Hamiltonian lies outside all offered models, the method returns the best-in-class — not a correct model.
- **SMC approximation error.** The $Z_i$ values are computed from a finite particle set; approximation error in $Z_i$ accumulates over experiments. With $N_\mathrm{exp}$ steps, log-Bayes-factor error grows as $O(N_\mathrm{exp}/N_p)$. Need sufficient $N_p$ to keep this below the signal.
- **Model complexity must be specified.** The prior $\Pr(\mathbf{x}|\mathcal{M})$ affects $B_{12}$ through prior volume. Overly diffuse priors penalise more complex models unfairly; overly tight priors can miss the true region. The choice matters.
- **Numerical underflow.** Computing $\prod_i Z_i$ over many experiments requires log-space arithmetic (sum of $\log Z_i$); the product itself underflows double precision almost immediately.

## Related notes
- [[Quantum Hamiltonian Learning Using Imperfect Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Hamiltonian Learning and Certification Using Quantum Resources (Wiebe-Granade-Ferrie-Cory 2014) — Paper Notes]]
- [[Sequential Monte Carlo for Quantum Parameter Estimation]]
- [[IQLE Loschmidt-Echo for Quantum Likelihood Evaluation]]
- [[Noise-Robust Bayesian Hamiltonian Learning]]
