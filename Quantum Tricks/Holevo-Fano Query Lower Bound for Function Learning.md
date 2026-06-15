# Holevo-Fano Query Lower Bound for Function Learning

> **Source:** Montanaro, arXiv:1105.3310
> **Tags:** #trick #lower-bound #query-complexity #information-theory #learning

## What it does

Lower-bounds quantum queries for identifying a function class by bounding how much classical information the oracle interaction can transmit.

## The trick

Suppose the unknown target $f$ is sampled uniformly from a finite class $\mathcal{F}$, and a quantum algorithm must identify it with bounded error.

View each query as a communication round:

1. The learner sends the query registers to the oracle.
2. The oracle applies

$$
|x\rangle|y\rangle\mapsto |x\rangle|y+f(x)\rangle.
$$

3. The registers return to the learner.

If each round sends $m$ qubits to the oracle and back, Holevo's theorem bounds the mutual information gained after $r$ rounds by $O(rm)$ bits. If $X$ is the random target and $Y$ is the learner's output,

$$
I(X:Y)\le O(rm).
$$

Fano's inequality says bounded-error recovery of $X$ requires mutual information comparable to

$$
\log_2 |\mathcal{F}|.
$$

Therefore

$$
r=\Omega\!\left(\frac{\log |\mathcal{F}|}{m}\right).
$$

In Montanaro's polynomial-learning setting, $m=(n+1)\log_2 q$ and

$$
|\mathcal{F}|=q^{1+n+\binom n2+\cdots+\binom nd},
$$

so

$$
r=\Omega(n^{d-1}).
$$

## When to reach for it

- Exact identification / learning of a large concept class.
- Query lower bounds where the target has many possible descriptions.
- Problems where each query can carry only $O(n)$ qubits of address/data information.

## Complexity

The generic template gives

$$
\Omega\!\left(\frac{\log |\mathcal{F}|}{\text{qubits communicated per query}}\right)
$$

queries.

## Caveat

This is an information bound. It can be loose when the bottleneck is not total information but the structure of how information is accessed. Here it is tight because the finite-difference algorithm matches it.

## Related notes

- [[Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes]]
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes]]
