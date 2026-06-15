# On Quantum Algorithms for Noncommutative Hidden Subgroups (Ettinger-Hoyer 1998) — Paper Notes

> **Source:** Mark Ettinger and Peter Høyer, *On quantum algorithms for noncommutative hidden subgroups*, arXiv:quant-ph/9807029
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9807029)
> **Tags:** #hidden-subgroup-problem #nonabelian #dihedral-hsp

---

## Metadata

- **Authors:** Mark Ettinger and Peter Høyer
- **Year:** 1998
- **arXiv:** [quant-ph/9807029](https://arxiv.org/abs/quant-ph/9807029)
- **Zoo entry:** Quantum Algorithm Zoo #143
- **Topic:** Nonabelian hidden subgroup problem; dihedral HSP; query/sample complexity versus post-processing.
- **Status in vault:** Added from Algorithm Zoo HSP batch.

## Computational problem

Given a finite group $G$ and an oracle $\gamma:G\to R$ that is constant and distinct on left cosets of a subgroup $H\leq G$, find generators for $H$.

The paper asks what survives from the abelian HSP algorithm once $G$ is noncommutative. Its main worked case is the dihedral group

$$D_N=\mathbb Z_N\rtimes \mathbb Z_2,$$

where the action of the nontrivial element of $\mathbb Z_2$ is inversion on $\mathbb Z_N$.

## What the paper does

This is an early nonabelian-HSP paper. It proves that the dihedral subgroup can be identified with only $\Theta(\log N)$ oracle evaluations, but the known classical post-processing is exponential. That distinction matters: the quantum samples contain enough information, but extracting the hidden reflection efficiently is already the hard part.

The paper also sketches a representation-theoretic view of nonabelian Fourier sampling. It does not yet give the later coset-state/PGM language, but the diagnostic is the same: nonabelian HSP is not only about getting samples; it is about choosing and processing the right measurement basis.

## Algorithmic structure

1. **Reduce general dihedral subgroups to the reflection case.**
   - Restrict the hiding function to the rotation subgroup $\mathbb Z_N\times\{0\}$.
   - Use the abelian HSP algorithm to find $H\cap (\mathbb Z_N\times\{0\})$.
   - Quotient by this normal rotation part. The remaining uncertainty is either trivial or a hidden order-2 subgroup $\{(0,0),(k_0,1)\}$.

2. **Sample a distribution depending on $k_0$.**
   - Apply Fourier transform over $\mathbb Z_N$, a Hadamard on the $\mathbb Z_2$ register, query the hiding function, then undo the transforms.
   - For hidden reflection $(k_0,1)$, measuring the first two registers gives probabilities
     $$\Pr[(a,0)]={1\over N}\cos^2(\pi k_0a/N),\qquad
     \Pr[(a,1)]={1\over N}\sin^2(\pi k_0a/N).$$
   - The samples are not uniform; their cosine bias encodes $k_0$.

3. **Use logarithmically many samples.**
   - With $m\geq 64\ln N$ samples from the cosine distribution, maximizing
     $$\sum_i \cos(2\pi \tilde{k}z_i/N)$$
     over candidate $\tilde{k}$ recovers $\min\{k_0,N-k_0\}$ with probability at least $1-1/(2N)$.
   - Hoeffding bounds justify the reuse of the same sample set across all candidate $k$.

4. **Classical post-processing is the bottleneck.**
   - The maximization over all $k$ checks $N$ candidates, so it is exponential in the input length $\log N$ by the method given.
   - This is the early version of the dihedral-HSP bottleneck that later becomes the subset-sum/sieve problem in Kuperberg and Regev.

## Key results

- Abelian HSP over a finite abelian group uses $O(\log |G|)$ oracle evaluations and polynomial-time post-processing.
- Dihedral HSP over $D_N$ uses $\Theta(\log N)$ oracle evaluations to identify the hidden subgroup with high probability.
- For the hidden reflection case, at most $89\log N+7$ oracle evaluations suffice.
- The paper separates **query complexity** from **time complexity** for nonabelian HSP: few quantum queries do not imply an efficient algorithm.

## Assessment

Historically important, technically modest by current standards. The paper is not an efficient dihedral-HSP algorithm. Its value is that it isolates the information-theoretic side of the problem before EHK made the query-complexity statement fully general.

The cosine/sine-biased sample picture here is the same obstruction seen later in a different basis. Kuperberg's labelled qubits store the hidden reflection as phases $e^{2\pi i ks/N}$ with random known labels $k$; this paper instead analyzes classical samples from a biased Fourier distribution. Both contain enough information, but neither gives an immediate polynomial-time reconstruction.

For the vault, this is the clean precursor to:

- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger--Høyer--Knill (2004)]], where polynomial query complexity is shown for every finite group.
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]], where the hidden reflection samples are processed by a sieve.
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]], where the same obstruction is recast as a space/time trade-off.

## Reusable ideas

- **Information before computation:** prove the coset-state samples identify $H$ before solving the measurement/post-processing problem.
- **Bias-as-parameter encoding:** convert a hidden reflection into cosine/sine-biased samples over $\mathbb Z_N$.
- **Quotient first:** remove the rotation subgroup by an abelian HSP call, then handle the nonnormal reflection part separately.

## Cross-links

- [[Hidden Subgroup Problem]]
- [[Dihedral HSP Query Samples vs Postprocessing]]
- [[Cosine-Bias Sampling for Dihedral Hidden Reflections]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|EHK (2004)]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg (2005)]]
- [[A Subexponential Time Algorithm for the Dihedral Hidden Subgroup Problem with Polynomial Space (Regev 2004) — Paper Notes|Regev (2004)]]

## References

- Mark Ettinger and Peter Høyer, *On quantum algorithms for noncommutative hidden subgroups*, arXiv:quant-ph/9807029.
