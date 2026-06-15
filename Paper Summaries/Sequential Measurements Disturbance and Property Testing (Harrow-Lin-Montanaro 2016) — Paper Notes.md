# Sequential Measurements Disturbance and Property Testing (Harrow-Lin-Montanaro 2016) — Paper Notes

> **Source:** Aram W. Harrow, Cedric Yen-Yu Lin, and Ashley Montanaro, *Sequential measurements, disturbance and property testing*, arXiv:1607.03236 (2016)
> **Links:** [arXiv](https://arxiv.org/abs/1607.03236) · [PDF](https://arxiv.org/pdf/1607.03236)
> **Tags:** #quantum-information #property-testing #sequential-measurements #gentle-measurement #query-complexity #QMA

---

## The problem

Given one copy of a state $\rho$ and many two-outcome measurements

$$
M_i=\{\Lambda_i,I-\Lambda_i\},\qquad i\in[n],
$$

decide whether at least one measurement accepts $\rho$ with high probability, or all measurements accept with low probability.

Naively trying the measurements one after another fails: low-probability outcomes can still rotate the state through an anti-Zeno chain. The paper gives ways to implement a quantum OR over measurements without assuming the tests commute.

## What the paper does

The technical core is a corrected quantum OR bound: with one copy of $\rho$, one can distinguish

$$
\exists i:\operatorname{tr}(\Lambda_i\rho)\approx 1
$$

from

$$
\mathbb E_i[\operatorname{tr}(\Lambda_i\rho)]\ll 1/n.
$$

The paper then plugs this into query-efficient property testers:

- $G$-isomorphism of functions in $O((\log |G|)/\epsilon)$ queries;
- Boolean function isomorphism in $O(n\log n/\epsilon)$ queries;
- graph isomorphism testing in $O(n\log n/\epsilon)$ queries;
- linear / affine isomorphism of Boolean functions in $O(n^2/\epsilon)$ queries;
- finite quantum-state properties in $O((\log |P|)/\epsilon^2)$ copies;
- unitary $S$-isomorphism in $O((\log |S|)/\epsilon^2)$ uses;
- product-across-some-cut testing for $n$-partite pure states in $O(n/\epsilon^2)$ copies.

My read: the property-testing applications are clean query/copy-complexity results, but the main reusable thing is the measurement-disturbance primitive. It patches a real hole in Aaronson's earlier quantum OR argument and gives a general pattern: average the tests into one POVM, then amplify spectral support rather than sequentially consuming the state.

## Technical setup

For a POVM element $\Lambda$ with implementation

$$
\Lambda\otimes |0\rangle\langle0|^{\otimes m}=\Delta\Pi\Delta,
$$

where

$$
\Delta=I\otimes |0\rangle\langle0|^{\otimes m},
$$

Algorithm 1 alternates the measurements $\{\Pi,I-\Pi\}$ and $\{\Delta,I-\Delta\}$ for $N$ rounds. It accepts if either $\Pi$ accepts or the ancilla-check $\Delta$ fails.

For a family of projectors, set

$$
\Lambda=\frac1n\sum_{j=1}^n\Lambda_j.
$$

This average can be implemented by a Fourier-index construction:

$$
\Pi=\sum_{i=0}^{n-1}\Lambda_{i+1}\otimes Q|i\rangle\langle i|Q^{-1},
$$

so

$$
\Delta\Pi\Delta=\left(\frac1n\sum_j\Lambda_j\right)\otimes |0\rangle\langle0|.
$$

## Key technical results

**Theorem 9.** Let $p_{\rm acc}(N)$ be the acceptance probability of Algorithm 1 on $(\Lambda,\rho)$, and let $P_{\ge\delta}$ project onto the eigenspace of $\Lambda$ with eigenvalues at least $\delta$. Then

$$
(1-e^{-1})\operatorname{tr}(P_{\ge 1/(2N)}\rho)
\le p_{\rm acc}(N)
\le 2N\operatorname{tr}(\Lambda\rho).
$$

This is the spectral heart of the argument. The procedure detects whether $\rho$ has support on the non-negligible eigenspaces of the average measurement.

**Corollary 11.** For projectors $\Lambda_1,\ldots,\Lambda_n$, if either

$$
\exists i:\operatorname{tr}(\Lambda_i\rho)\ge 1-\epsilon
$$

or

$$
\mathbb E_j[\operatorname{tr}(\Lambda_j\rho)]\le\delta,
$$

then a one-copy test accepts with probability at least $(1-\epsilon)^2/7$ in the first case and at most $4\delta n$ in the second.

The proof uses the gentle measurement lemma to show that if some $\Lambda_i$ accepts $\rho$ almost certainly, then $\rho$ cannot lie mostly in the small-eigenvalue subspace of $\Lambda$.

**Lemma 12.** Given controlled unitaries $U_1,\ldots,U_n$ and copies of $|\psi\rangle$, decide whether some $U_i|\psi\rangle=|\psi\rangle$ or all satisfy

$$
|\langle\psi|U_i|\psi\rangle|\le1-\epsilon
$$

using

$$
O((\log n)/\epsilon)
$$

copies of $|\psi\rangle$.

The per-unitary test is a Hadamard-type invariant-state test applied to $k$ copies; $k=O((\log n)/\epsilon)$ suppresses false positives to $O(1/n)$, and Corollary 11 ORs over the unitaries.

## Property-testing applications

### $G$-isomorphism of functions

For $f,g:X\to Y$ and permutations $\sigma\in G$, define

$$
|f\rangle=\frac1{\sqrt{|X|}}\sum_{x\in X}|x\rangle|f(x)\rangle.
$$

Use the state

$$
|\psi\rangle=\frac1{\sqrt2}(|0\rangle|f\rangle+|1\rangle|g\rangle)
$$

and a unitary $U'_\sigma$ such that

$$
\langle\psi|U'_\sigma|\psi\rangle=\langle f\circ\sigma|g\rangle=1-d(f\circ\sigma,g).
$$

Then Lemma 12 gives an $\epsilon$-tester using

$$
O((\log |G|)/\epsilon)
$$

queries.

Consequences:

- Boolean function isomorphism: $O(n\log n/\epsilon)$ quantum queries versus classical $\Omega(2^{n/2}/n^{1/4})$.
- Graph isomorphism testing: $O(n\log n/\epsilon)$ quantum queries, improving the previous $\widetilde O(n^{7/6})$ query algorithm.
- Linear / affine Boolean function isomorphism: $O(n^2/\epsilon)$ quantum queries versus classical $\Omega(2^{n/2})$.
- HSP-style property testing: $O((\log |G|)/\epsilon)$ queries for testing whether $f$ is constant on cosets of some nontrivial subgroup, with a property-testing promise.

### Quantum states and unitaries

For a finite set $P$ of pure states, testing membership in $P$ costs

$$
O((\log |P|)/\epsilon^2)
$$

copies, with no dependence on the minimum pairwise separation inside $P$.

For a finite set $P\subset U(d)$, the Choi state

$$
|U\rangle=(U\otimes I)|\Phi\rangle
$$

turns unitary-property testing into state-property testing. Thus membership in $P$ costs

$$
O((\log |P|)/\epsilon^2)
$$

uses of the unknown unitary.

For unitary $S$-isomorphism — deciding whether

$$
UVU^\dagger=W
$$

for some $U\in S$, or all such conjugates are $\epsilon$-far — the paper gives

$$
O((\log |S|)/\epsilon^2)
$$

uses of $V$ and $W$.

### Genuine multipartite entanglement

To test whether an $n$-partite pure state is product across some nontrivial cut, combine the product test for a fixed cut with Corollary 11 over all $2^{n-1}-1$ cuts. The copy complexity is

$$
O(n/\epsilon^2).
$$

This detects the negation of genuine multipartite entanglement: product across at least one cut versus $\epsilon$-far from product across every cut.

## De-Merlinizing correction

Aaronson's claimed quantum OR bound in the QMA/qpoly de-Merlinization paper had a disturbance gap: it asserted that if sequential measurements move the state far, then one of the measured events must have occurred with noticeable probability. The anti-Zeno example shows this is false.

Harrow--Lin--Montanaro replace that step with Corollary 13. If Bob's amplified verifier has witness dimension $d=2^W$, apply Algorithm 1 to the average over computational-basis witnesses. In the yes case, some witness accepts with probability $\eta$; in the no case, all witnesses accept with probability $\zeta$. The acceptance probabilities become at least $\eta^2/7$ versus at most $2\zeta\lceil d/\eta\rceil$.

This restores Aaronson's conclusion:

$$
Q^1(f)=O(QMA_w^1(f)\,w\log^2 w).
$$

## Alternative disturbance-test protocol

Appendix A gives a second protocol. It keeps a coherent branch containing the original state and, with small probability, checks whether the working branch has drifted away. The loop alternates:

1. a low-probability disturbance test against the original branch;
2. a random measurement $\Lambda_j$ on the working branch.

Gao's improved quantum union bound controls false acceptance in the all-low case. The parameters are worse, but the protocol is conceptually useful: if measurements have made the state far from the start, that disturbance itself is evidence for the OR case.

## Reusable ideas

1. [[Average-Measurement Marriott-Watrous OR Test]] — replace sequentially trying many measurements by spectral amplification of $\Lambda=\frac1n\sum_j\Lambda_j$.
2. [[Fourier-Indexed Implementation of an Average POVM]] — implement the uniform average of many projectors with an index register and QFT basis.
3. [[Hadamard Invariance Test for Unitary Orbits]] — convert $U|\psi\rangle=|\psi\rangle$ testing into a two-outcome measurement whose false-positive rate decays on tensor powers.
4. [[Disturbance-as-Evidence Sequential Measurement Test]] — use a low-probability branch-comparison test to detect anti-Zeno drift.
5. [[Function Isomorphism Testing via Orbit States]] — encode $f,g$ as orbit states and test whether a group action fixes their superposition.

## Limits / caveats

- The property testers are query-efficient, not necessarily time-efficient. Iterating over $G$ is hidden inside measurement implementations unless $G$ has usable structure.
- The one-copy OR bound is strongest when the yes case has near-certain acceptance for some measurement. Intermediate acceptances require amplification/tensor powers.
- The finite-state tester has two-sided error; Wang's separated-state result gives one-sided error but depends on minimum pairwise distance.
- For graph isomorphism, this is a property-testing result under an $\epsilon$-far promise, not a full graph-isomorphism decision algorithm.
- The result does not say one can try exponentially many arbitrary tests in polynomial time. The average measurement must itself have an efficient implementation, or the query/copy bound hides the real cost.

## References within this paper

- Aaronson (2006) — source of the flawed quantum OR/de-Merlinization lemma corrected here.
- Marriott--Watrous (2005) — in-place amplification template adapted into Algorithm 1.
- Gao (2015) — improved quantum union bound for sequential projective measurements.
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes|Ambainis--Childs--Liu (2011)]] and [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes|Bravyi--Harrow--Hassidim (2011)]] — broader quantum property-testing context.
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes|Ettinger--Høyer--Knill (2004)]] — sequential subgroup testing comparison.
- Harrow--Montanaro (2010/2013) — product test used for the multipartite-entanglement application.
- Wang (2011) — prior finite-set/unitary-property testers.

## Cross-links

### Paper notes

- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]]
- [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]]
- [[The Quantum Query Complexity of the Hidden Subgroup Problem Is Polynomial (Ettinger-Høyer-Knill 2004) — Paper Notes]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]

### Trick cards

- [[Average-Measurement Marriott-Watrous OR Test]]
- [[Fourier-Indexed Implementation of an Average POVM]]
- [[Hadamard Invariance Test for Unitary Orbits]]
- [[Disturbance-as-Evidence Sequential Measurement Test]]
- [[Function Isomorphism Testing via Orbit States]]
