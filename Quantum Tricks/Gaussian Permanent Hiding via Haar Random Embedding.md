
> **Source:** Aaronson & Arkhipov, arXiv:1011.3245
> **Tags:** #trick #complexity-theory #BosonSampling #permanent #average-case #quantum-supremacy

## What it does

Converts a worst-case permanent estimation problem into a BosonSampling output probability, using a random Haar unitary to prevent an adversarial approximate sampler from knowing which probability to corrupt.

## The trick

You want to estimate $|\text{Per}(X)|^2$ for a given $X \sim \mathcal{N}(0,1)_{n \times n}^{\mathbb{C}}$. The plan: embed $X$ as a submatrix of a Haar-random $m \times m$ unitary $U$, define a boson computer using $U$, and ask a classical approximate sampler for samples from the output distribution $\mathcal{D}_U$.

The key observation: the probability of a specific output $T$ is $\Pr[T] \propto |\text{Per}(U_{T,[n]})|^2 \approx |\text{Per}(X)|^2 / m^n$ (after scaling). If you choose $T$ uniformly at random, the sampler has no way to know which output encodes the hard instance. An adversarial sampler that corrupts any fixed set of output probabilities will miss $T$ with high probability.

Using an NP oracle and universal hashing, you can extract an estimate of $\Pr[T]$ from the sampler's output, giving an estimate of $|\text{Per}(X)|^2$ to additive error $\pm \varepsilon \cdot n!$ with high probability. This puts $|\text{GPE}|^2_{\pm} \in \text{BPP}^{\text{NP}}$ conditional on the existence of an efficient approximate sampler.

The bridge between Gaussian matrices and submatrices of Haar unitaries is the [[Submatrix-of-Haar-Unitary to Gaussian Approximation]] approximation (needs $n \leq m^{1/6}$ roughly).

## When to reach for it

- Proving approximate classical simulation hardness for a sampling-based quantum model where output probabilities involve a \#P-hard object.
- The template: (1) identify what algebraic object (permanent, hafnian, etc.) computes the probabilities; (2) show a random instance of that object can be embedded in the quantum model; (3) use randomness to prevent adversarial corruption; (4) extract via hashing + NP oracle.
- Also useful as a template for other average-case hardness arguments where you want to convert worst-case hardness into hardness for typical instances.

## Complexity

Reduction overhead: polynomial (embedding + hashing + NP oracle query). The sampler runs in $\text{poly}(n)$ by assumption.

## Caveat

- Requires the [[Submatrix-of-Haar-Unitary to Gaussian Approximation]] to hold quantitatively ($n \leq m^{1/6}$ constraint, so $m = \Omega(n^6)$ modes needed for the reduction to work — much larger than the $m = O(n^2)$ used in experiments).
- Requires the Permanent Anti-Concentration Conjecture (PACC) to convert the additive estimate into a useful multiplicative one (to connect $|\text{GPE}|^2_{\pm}$ to $\text{GPE}_{\times}$).
- Only shows average-case hardness over Haar-random $U$, not for a specific $U$ chosen adversarially.

## Related notes
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]]
- [[Submatrix-of-Haar-Unitary to Gaussian Approximation]]
- [[Permanent Formula for Bosonic Amplitudes]]
- [[Post-Selection Boosting for Hardness of Sampling]]
