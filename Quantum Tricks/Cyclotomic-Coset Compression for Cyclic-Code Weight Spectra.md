# Cyclotomic-Coset Compression for Cyclic-Code Weight Spectra

> **Source:** Geraci-Lidar, arXiv:quant-ph/0703023
> **Tags:** #trick #coding-theory #finite-fields #symmetry #Gauss-sums

## What it does

Reduces the number of irreducible-cyclic-code weight evaluations by grouping indices into Frobenius orbits.

## The trick

For an irreducible cyclic $[n,k]$ code over $\mathbb F_q$, write

$$
q^k-1=nN.
$$

The nonzero codewords split into at most $N$ cyclic-shift weight classes. Geraci-Lidar refine this by grouping the indices $\{0,1,\ldots,N-1\}$ into $q$-cyclotomic cosets:

$$
\{j,jq,jq^2,\ldots,jq^{r(j)}\}\pmod N.
$$

The weight expression $S(i)$ is invariant under $i\mapsto iq$ because the Frobenius map $x\mapsto x^q$ permutes $\mathbb F_{q^k}$ and leaves the trace additive character unchanged:

$$
\exp\!\left(\frac{2\pi i}{q}\operatorname{Tr}(b^q)\right)=
\exp\!\left(\frac{2\pi i}{q}\operatorname{Tr}(b)\right).
$$

So one evaluates $S(i)$ once per cyclotomic coset, then weights the result by the coset size.

The number of cosets is

$$
N_C=\sum_{f\mid N}\frac{\varphi(f)}{\operatorname{ord}_q f}\leq N.
$$

## When to reach for it

Use it when a cyclic or finite-field construction has an expensive quantity invariant under Frobenius powers. Replace the full index set by orbit representatives and carry multiplicities forward.

## Complexity

Coset representatives and sizes can be computed in time linear in $N$. The savings depend on the orbit structure: the algorithm's repeated weight-evaluation count drops from $N$ to $N_C$.

## Caveat

This improves constants or exponents only when the cyclotomic cosets are large. Some instances have $N$ already small; others have little extra compression. It does not remove the need for the Gauss-sum phase estimates themselves.

## Related notes

- [[On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes]]
- [[Exact Weight Recovery by Divisibility-Gap Rounding]]
- [[Potts Partition Function via Cocycle-Code Weight Enumerators]]
- [[Efficient Quantum Algorithms for Estimating Gauss Sums (van Dam-Seroussi 2002) — Paper Notes]]
