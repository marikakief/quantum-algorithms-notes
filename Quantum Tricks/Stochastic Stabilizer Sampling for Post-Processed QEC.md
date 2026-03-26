# Stochastic Stabilizer Sampling for Post-Processed QEC

> **Source:** McClean, Jiang, Rubin, Babbush, Neven, arXiv:1903.05786
> **Tags:** #trick #sampling #error-mitigation #stabilizer-codes #NISQ #measurement

## What it does

Evaluates the stabilizer-projected expectation value $\text{Tr}(P\rho P\, \Gamma)$ by stochastically sampling from the stabilizer group, avoiding explicit enumeration of all $2^m$ group elements.

## The trick

The projector $P = \frac{1}{2^m}\sum_{M \in \mathcal{S}} M$ turns the corrected observable into a sum over $2^m$ stabilizer group elements (indexed by bitstrings $\chi \in \{0,1\}^m$):

$$\mu_\Gamma = \tilde{\gamma} \sum_{\chi \in \{0,1\}^m} \sum_j \frac{\gamma_j}{2^m} \,\text{Tr}(\rho\, \Gamma_j S_\chi)$$

where $S_\chi = S_1^{\chi_1} \cdots S_m^{\chi_m}$.

**Sampling procedure:** Draw $(\chi, j)$ with probability $p_{\chi,j} = \gamma_j / 2^m$. Prepare $\rho$, measure the Pauli operator $\Gamma_j S_\chi$, record $\pm 1$. Average over $N_s$ samples.

**Variance analysis:** The estimator is a binomial variable with variance:

$$\text{Var}(\hat{\mu}_\Gamma) = \tilde{\gamma}^2 \, p_{+1}(1 - p_{+1})$$

For the depolarizing channel with mixing parameter $w$:
- $w = 0$ (perfect state): variance equals the variance of measuring $\Gamma$ directly. The correction adds *zero* overhead.
- $w > 0$: variance $\sim \frac{1}{4}(2-w)w$ (for a single Pauli eigenstate measurement).

The cost scales with the volume of the state *outside* the code space, not with the number of stabilizer group elements.

**Importance sampling:** Weight samples by $\chi$ according to the Pauli weight $W_\chi$ of $S_\chi$. For a single-qubit depolarizing channel with error rate $p$, sample $\chi$ proportional to $(1-p)^{W_\chi}$. Low-weight stabilizers carry more signal under local noise.

## When to reach for it

- Any time you use [[Stabilizer Subspace Projection for Error Mitigation|stabilizer projection]] — this is how you make it practical.
- The code has many stabilizer generators ($m$ large) so that $2^m$ is intractable to enumerate.
- You expect the state to be mostly in the code space (low $w$) — the sampling cost is cheap there.

## Complexity

- **Samples needed:** $N_s \sim 1 / (w(2-w))$ to achieve constant precision on the corrected observable, where $w$ is the non-code-space fraction.
- **Per-sample cost:** One state preparation + one Pauli measurement.
- **Independence from $m$:** The number of stabilizer generators does *not* appear in the sampling cost. A code with 100 generators costs the same to sample as one with 4, given the same state quality.

## Caveat

- Sampling cost diverges as $w \to 1$ (maximally noisy state). At high error rates, you need exponentially many samples.
- The uniform sampling scheme is simple but not optimal. The paper suggests importance sampling by Pauli weight but doesn't provide a full analysis of the variance reduction.
- Each sample requires a fresh preparation of $\rho$ — there's no reuse of measurement data across different $(\chi, j)$ pairs.

## Related notes
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]]
- [[Stabilizer Subspace Projection for Error Mitigation]]
- [[Pauli Expectation Value Estimation]] — the underlying measurement primitive
