# Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes

> **Source:** Andris Ambainis and Ashley Montanaro, *Quantum algorithms for search with wildcards and combinatorial group testing*, arXiv:1210.1148 (2012/2013)
> **Links:** [arXiv](https://arxiv.org/abs/1210.1148) · [PDF](https://arxiv.org/pdf/1210.1148)
> **Tags:** #query-complexity #quantum-learning #group-testing #state-discrimination #pretty-good-measurement #adversary-method

---

## The computational problem

The paper studies two oracle-identification problems.

### Search with wildcards

There is an unknown string $x\in\{0,1\}^n$. A wildcard query is a string $s\in\{0,1,*\}^n$, or equivalently a pair $(S,y)$ with $S\subseteq[n]$ and $y\in\{0,1\}^{|S|}$. The oracle returns

$$
Q_x(S,y)=1 \quad\Longleftrightarrow\quad x_S=y.
$$

The task is to recover all of $x$ with bounded error.

Classically, each query returns one bit, so $\Omega(n)$ queries are necessary.

### Combinatorial group testing

There is an unknown string $x\in\{0,1\}^n$ with Hamming weight $|x|\le k$. A query is a subset $S\subseteq[n]$ and returns

$$
Q_x(S)=\bigvee_{i\in S}x_i.
$$

The task is to identify all 1-indices. Classical query complexity is

$$
\Theta(k\log(n/k)).
$$

## What the paper does

The wildcard result is the more interesting one. Ambainis and Montanaro give a quantum algorithm using

$$
O(\sqrt n\log n)
$$

queries on average, with an $\Omega(\sqrt n)$ lower bound. The algorithm does not use [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]] or [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes|quantum walk search]]. It repeatedly prepares subset states and applies a [[Pretty-Good Measurement for Wildcard Subset States|pretty-good measurement]] that reconstructs almost all missing bits.

For combinatorial group testing, the paper gives a simple

$$
O(k\log k)
$$

Las Vegas quantum algorithm, independent of $n$, and an $\Omega(\sqrt k)$ lower bound. The exact complexity remains open; the paper explicitly corrects an earlier claimed $O(\sqrt{k}\,\mathrm{polylog}(k))$ upper bound.

My assessment: the wildcard algorithm is the keeper. The group-testing algorithm is clean and useful, but the gap between $O(k\log k)$ and $\Omega(\sqrt k)$ is wide.

## The algorithm / construction

### Subset states for wildcard search

For each $k$, define

$$
|\psi_x^k\rangle=
\binom nk^{-1/2}
\sum_{S\subseteq[n], |S|=k}|S\rangle|x_S\rangle.
$$

This state is a coherent superposition over $k$ revealed positions of $x$.

The key state-discrimination lemma says: if

$$
k=n-O(\sqrt n),
$$

then there is a POVM which, given $|\psi_x^k\rangle$, outputs a guess $\tilde x$ whose expected Hamming distance from $x$ is $O(1)$.

The POVM is the pretty-good measurement. The Gram matrix satisfies

$$
G_{xy}=\langle\psi_x^k|\psi_y^k\rangle
=\frac{\binom{n-d(x,y)}{k}}{\binom nk},
$$

so it depends only on $x\oplus y$ and is diagonalised by the Fourier transform over $\mathbb{Z}_2^n$. The eigenvalues are

$$
\lambda(s)=2^{n-k}\frac{\binom{n-|s|}{n-k}}{\binom nk}.
$$

The analysis uses this Fourier-diagonal form to bound the expected Hamming error of the PGM.

### Bootstrapping wildcard search

The wildcard algorithm builds a chain

$$
n_0<n_1<\cdots<n_\ell=n,
\qquad n_{i-1}=\lceil n_i-\sqrt{n_i}\rceil,
$$

so $\ell=O(\sqrt n)$.

1. Prepare $|\psi_x^{n_0}\rangle$ by querying $n_0=O(\sqrt n)$ positions.
2. At stage $s$, transform $|\psi_x^{n_{s-1}}\rangle$ into a superposition over larger sets $S'$ of size $n_s$, each containing a smaller subset whose bits are known coherently.
3. Apply the PGM subroutine on each induced $n_{s-1}$-subset to guess $x_{S'}$.
4. Verify the guess with one wildcard query.
5. If there are errors, find and flip them by coherent binary search using $O(\log n)$ wildcard queries per error.

Since the PGM has $O(1)$ expected errors at each stage, each stage costs $O(\log n)$ expected queries. Across $O(\sqrt n)$ stages, the total is

$$
O(\sqrt n\log n).
$$

This is the trick card [[PGM Bootstrapping for Wildcard Search]].

### Group testing: isolate one defective, then use one-query identification

When $|x|\le 1$, a group-testing OR query on subset $S$ computes

$$
\bigvee_i x_i s_i = x\cdot s,
$$

because at most one $x_i$ is 1. Then [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani]] recovers the defective index with one query.

For general $|x|\le k$, randomly sample a subset $S$ by including each unknown index with probability about $1/k$. With constant probability, $S$ contains exactly one defective item. Run the one-defective routine on the restricted universe and add any found indices to the known set. If only an upper bound $k$ is known, try guesses $1,2,4,\ldots,k$.

This gives expected

$$
O(k\log k)
$$

queries and is Las Vegas, because a final complement query checks whether all defectives have been found. See [[Random Isolation plus Bernstein-Vazirani for Group Testing]].

## Key results

### Theorem 1: wildcard search

There is a quantum algorithm for search with wildcards using

$$
O(\sqrt n\log n)
$$

queries on average. Any bounded-error quantum algorithm needs

$$
\Omega(\sqrt n)
$$

queries.

### Lemma 3: PGM on subset states

For

$$
|\psi_x^k\rangle=
\binom nk^{-1/2}\sum_{|S|=k}|S\rangle|x_S\rangle
$$

and $k=n-O(\sqrt n)$, there is a POVM whose output $\tilde x$ satisfies

$$
\mathbb{E}[d(x,\tilde x)]=O(1).
$$

### Theorem 2: combinatorial group testing

There is a quantum algorithm for combinatorial group testing using

$$
O(k\log k)
$$

queries on average, independent of $n$. Any bounded-error quantum algorithm needs

$$
\Omega(\sqrt k)
$$

queries.

### Lemma 4: one-defective group testing

If $|x|\le 1$, combinatorial group testing can be solved exactly with one quantum query.

### Lower bounds

The wildcard lower bound uses a strong positive weighted adversary scheme: weights connect strings at Hamming distance 1. For a wildcard query $(S,t)$, the number of neighbouring inputs separated by the query is at most $|S|$ on a yes-input and at most 1 on the corresponding no-input, yielding the $\Omega(\sqrt n)$ bound. The group-testing lower bound follows by reducing wildcard search on $k$ bits to a promised group-testing instance with $k$ two-bit blocks.

## Comparison with prior work

| Work | Model | Result / role |
|---|---|---|
| van Dam (1998) | standard oracle interrogation | learns $n$ bits with $n/2+O(\sqrt n)$ queries |
| Farhi et al. (1999) | standard oracle interrogation lower bound | $n/2+\Omega(\sqrt n)$ lower bound |
| Iwama et al. (2010) | quantum counterfeit coin | $O(k^{1/4})$ queries in a quantum scale model |
| Cleve et al. (2012) | substring queries | $3n/4+o(n)$ upper bound and $\Omega(n/\log^2 n)$ lower bound |
| This paper | wildcard queries | $O(\sqrt n\log n)$ upper bound, $\Omega(\sqrt n)$ lower bound |
| Classical CGT | OR subset queries | $\Theta(k\log(n/k))$ |
| This paper | quantum CGT | $O(k\log k)$ upper bound, $\Omega(\sqrt k)$ lower bound |

## Limits / caveats

- The wildcard upper and lower bounds differ by a $\log n$ factor.
- The group-testing bounds are far apart: $O(k\log k)$ versus $\Omega(\sqrt k)$. The paper leaves the true quantum query complexity of CGT open.
- The PGM is described existentially/algorithmically at the query level. Gate complexity of implementing the measurement is not the focus.
- The algorithm uses expected query complexity on the worst-case input, not an input distribution.
- The reduction from wildcard search to group testing gives a lower bound, not a matching upper bound.

## Reusable ideas

1. [[PGM Bootstrapping for Wildcard Search]] — use a pretty-good measurement on coherent subset states to recover all but $O(1)$ hidden bits, then verify and correct.
2. [[Random Isolation plus Bernstein-Vazirani for Group Testing]] — randomly restrict the universe until one defective remains, then use the one-query BV routine.
3. [[Wildcard-to-Group-Testing Block Reduction]] — encode each wildcard bit as a two-item block with exactly one defective to transfer lower bounds to CGT.
4. [[Neighbour-Edge Adversary for Wildcard Queries]] — weight Hamming-neighbour input pairs and count how many a wildcard query can separate.

## References within this paper

- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes|Beals et al. (1998)]] — standard-model oracle lower bounds.
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|Špalek-Szegedy (2006)]] — adversary framework cited for the lower bound.
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes|Bernstein-Vazirani (1993)]] — conceptual ancestor of the one-defective CGT routine.
- Belavkin (1975), Hausladen-Wootters (1994), Eldar-Forney (2001) — PGM / square-root measurement.
- van Dam (1998) — quantum oracle interrogation.
- Iwama et al. (2010) — quantum counterfeit coin problem.
- Atici-Servedio (2007) — quantum junta learning/testing.
- Porat-Rothschild (2008) — explicit classical non-adaptive group testing schemes.
- Zhang (2005) — strong weighted adversary bound.

## Cross-links

### Paper notes

- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]
- [[Quantum Query Complexity of Learning Multilinear Polynomials (Montanaro 2012) — Paper Notes]]

### Trick cards

- [[PGM Bootstrapping for Wildcard Search]]
- [[Random Isolation plus Bernstein-Vazirani for Group Testing]]
- [[Wildcard-to-Group-Testing Block Reduction]]
- [[Neighbour-Edge Adversary for Wildcard Queries]]
- [[Finite-Field Bernstein-Vazirani via Trace Characters]]
