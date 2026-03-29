> **Source:** Dorit Aharonov and Itai Arad, *The BQP-hardness of approximating the Jones Polynomial*, Israel Journal of Mathematics **176**(1):1–52 (2010); arXiv:quant-ph/0605181
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0605181)
> **Tags:** #jones-polynomial #BQP-complete #universality #braid-group #complexity

---

## The computational problem

Same as [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL (2006)]]: approximate the Jones polynomial of the plat closure of a braid at $e^{2\pi i/k}$, to within the additive accuracy of the AJL algorithm. The question here is: for which values of $k$ is this problem BQP-hard?

---

## What the paper does

Extends the BQP-hardness of Jones polynomial approximation from constant $k$ (the Freedman-Larsen-Wang result, which covered $k = 5$ and $k \geq 7$) to **all $k$ that are polynomial in the input size**. This matches exactly the range where the [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL algorithm]] is efficient, proving the problem is **BQP-complete** for all these values.

As a side benefit, the paper gives a substantially simpler proof of the constant-$k$ case than Freedman et al., avoiding advanced Lie algebra representation theory. The proof is accessible to a CS audience.

### The universality problem

To show BQP-hardness, you need to show that the images of braid generators in the path model representation are *universal* for quantum computation — i.e., they generate a dense subgroup of $SU(n)$ (or $SU(2)$ per encoded qubit). For constant $k$, this was known from Freedman-Larsen-Wang, but their proof used the classification of closed subgroups of $SU(n)$ via Lie algebras.

For polynomial $k$, the situation is worse: the representation dimension grows with $k$, so the "two-qubit gates" from the path model act on a space whose dimension grows. Standard universality tools don't directly apply.

---

## The key technical tools

### 1. The Bridge Lemma

Given a group $G$ acting on a tensor product $V \otimes W$, if $G$ acts irreducibly on $V$ and on $W$ separately, and there exists a "bridge" element that doesn't preserve the tensor product structure, then $G$ acts irreducibly (hence densely) on $V \otimes W$.

### 2. The Decoupling Lemma

Given generators that are close to the identity, you can decouple their action on different subspaces by commutator constructions. If the generators are $\varepsilon$-close to $I$, the commutator $[A,B]$ is $\varepsilon^2$-close, but its action on the "interesting" subspace can remain non-trivial.

These lemmas provide a general toolkit for proving density of subgroups in $SU(n)$, applicable beyond the Jones polynomial context.

### The encoding

Following Kitaev and independently Wocjan-Yard: encode one logical qubit into two paths of length 4 in the path model representation. The two basis paths correspond to $|0\rangle$ and $|1\rangle$. Braid generators on 4 strands then give single-qubit gates; braid generators spanning two blocks of 4 strands give entangling gates. The claim is that these generate all of $SU(2^n)$.

### The proof strategy

1. Show single-qubit universality: the image of $B_4$ in the path model generates a dense subgroup of $SU(2)$ for each encoded qubit.
2. Show entangling gates exist between adjacent qubits.
3. Use the Bridge and Decoupling Lemmas to lift from local universality to global universality on $n$ qubits.

For constant $k$, step 1 reduces to checking that two specific matrices generate a dense subgroup of $SU(2)$, which follows from the classification of finite subgroups of $SU(2)$. For polynomial $k$, the matrices approach the identity (they're $O(1/k)$ rotations), making density harder to establish — you need finer control over the Lie algebra generated.

---

## Key results

**Theorem 5.1 (Main result):** For any $k = \text{poly}(n)$ with $k \geq 5$ and $k \neq 6$, the problem of approximating $V_{B^{\text{pl}}}(e^{2\pi i/k})$ to within the AJL accuracy is BQP-hard.

Combined with the AJL algorithm, this gives:

**Corollary:** The Jones polynomial approximation problem (plat closure, additive accuracy) is **BQP-complete** for all $k = \text{poly}(n)$, $k \geq 5$, $k \neq 6$.

**Classical implication (via Kuperberg):** The *multiplicative* approximation of the Jones polynomial at the same values is #P-hard.

---

## Comparison with prior work

| Paper | Scope of BQP-hardness |
|---|---|
| Freedman-Larsen-Wang (2002) | Constant $k = 5$ and $k \geq 7$; Lie algebra proof |
| **This paper (2006/2011)** | All polynomial $k \geq 5$, $k \neq 6$; elementary proof |
| [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes\|AJL (2006)]] | Algorithm for all $k$; BQP-hardness was open for non-constant $k$ |

---

## Limits / caveats

- The exclusion of $k = 6$ is real: at $k = 6$ the path model representation factors through a finite group and isn't universal.
- The BQP-hardness is for the plat closure only. The trace closure problem remains of unknown complexity.
- The proof requires the braid to have $\Omega(k^2)$ strands to achieve universality — so very short braids at large $k$ are not covered.
- The additive approximation caveat from AJL still applies: BQP-hardness doesn't mean the approximation is useful for all links, just that there exist links for which it captures the full power of quantum computation.

---

## Reusable ideas

1. **[[Bridge Lemma for Tensor Product Universality]]:** If a group acts irreducibly on each factor of $V \otimes W$ and has an element that doesn't preserve the tensor product structure, it acts irreducibly on $V \otimes W$. A clean, general tool for proving universality of multi-qubit gates.

2. **[[Decoupling via Iterated Commutators]]:** To isolate the action of generators on a specific subspace, take commutators: $[A, [A, B]]$ amplifies the action on the "interesting" directions while suppressing the "boring" ones. Useful whenever you have gates that are close to the identity and need to show their Lie algebra spans the full tangent space.

---

## References within this paper

- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|Aharonov, Jones & Landau (2006)]] — the algorithm whose hardness this paper proves
- Freedman, Larsen & Wang (2002) — original BQP-hardness for constant $k$
- Freedman, Kitaev, Wang (2002) — simulation of TQFT by quantum computers
- Kuperberg (2009) — multiplicative hardness via additive hardness + bridge
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes|Aharonov, Arad, Eban & Landau (2007)]] — extends to Tutte plane using non-unitary representations

---

## Cross-links

### Paper notes
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]] — the algorithm
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]] — the Tutte plane extension
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]] — another BQP-completeness proof by Aharonov et al.

### Trick cards
- [[Bridge Lemma for Tensor Product Universality]] — the tensor product universality tool (extended to non-unitary groups in [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes|the Tutte plane paper]])
- [[Decoupling via Iterated Commutators]] — the Lie algebra generation technique (likewise extended)
- [[Hadamard Test for Trace Estimation]] — used in the algorithm this paper analyses
- [[Jørgensen Inequality for Non-Unitary Universality Seeding]] — replaces the finite-subgroup-of-$SO(3)$ argument for non-unitary seeding in the Tutte plane extension
