> **Source:** Michael H. Freedman, Alexei Kitaev, and Zhenghan Wang, *Simulation of topological field theories by quantum computers*, Communications in Mathematical Physics **227**:587–603, 2002; arXiv:quant-ph/0001071
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0001071) · [Journal](https://doi.org/10.1007/s002200200635)
> **Tags:** #TQFT #topological #BQP #simulation #mapping-class-group #modular-functor #foundational

---

## The computational problem

**TQFT simulation:** Given a unitary topological modular functor (UTMF) $V$ and a diffeomorphism $h: \Sigma \to \Sigma$ of a surface $\Sigma$, written as a word of length $n$ in standard generators (Dehn twists and braid moves), simulate the transformation $V(h): V(\Sigma) \to V(\Sigma)$ on a quantum computer.

The difficulty: the state space $V(\Sigma)$ has no natural tensor product structure (unlike the computational Hilbert space $({\mathbb{C}}^2)^{\otimes k}$ of a quantum computer), and the Hamiltonian of the topological theory vanishes identically ($H \equiv 0$). Standard simulation via Trotter formulas doesn't apply.

---

## What the paper does

Proves that any unitary topological modular functor can be efficiently simulated by a quantum circuit. Specifically: $V(h)$ is simulated by a quantum circuit of length $\leq c \cdot n \cdot \log b_1(\Sigma)$, where $n$ is the word length of $h$, $b_1(\Sigma)$ is the first Betti number of $\Sigma$, and $c$ is a constant depending only on $V$.

This has two consequences:

1. **TQFTs don't give extra computational power.** Any computation a TQFT-based system can do is within BQP. TQFTs cannot define a model of computation stronger than the standard quantum circuit model.

2. **TQFTs offer a new perspective on quantum computation.** Their rich mathematical structure — mapping class groups, modular functors, pants decompositions — might suggest new quantum algorithms. This hope was realised by [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes|AJL (2006)]], which used the Temperley-Lieb algebra (directly connected to TQFTs) to approximate the Jones polynomial.

The paper is a foundational piece connecting topological quantum field theory to computational complexity. It supports a "quantum Church's thesis" — that the quantum circuit model is the unique model of physically-realisable quantum computation, just as the Turing machine is for classical computation.

---

## The construction

### Background: UTMFs and their state spaces

A UTMF assigns to each labelled surface $\Sigma$ a finite-dimensional Hilbert space $V(\Sigma)$, and to each diffeomorphism $h: \Sigma \to \Sigma'$ a unitary map $V(h): V(\Sigma) \to V(\Sigma')$. The key axioms:

| Axiom | Statement |
|---|---|
| Disjoint union | $V(\Sigma_1 \sqcup \Sigma_2) = V(\Sigma_1) \otimes V(\Sigma_2)$ |
| Gluing | $V(\Sigma_g) = \bigoplus_{x \in L} V(\Sigma, (\ell, x, \hat{x}))$ — gluing boundary components sums over labels |
| Disk | $V(D, a) \cong \mathbb{C}$ if $a = 1$, else $0$ |
| Annulus | $V(A, (a,b)) \cong \mathbb{C}$ if $a = \hat{b}$, else $0$ |
| Algebraic | All basic data (F-matrices, S-matrices, mapping class group actions) are algebraic over $\mathbb{Q}$ |

The algebraic axiom prevents pathological examples where, say, a boundary twist multiplies by a real number encoding an uncomputable function. All known TQFTs satisfy it.

### The simulation strategy

**Core idea:** Embed $V(\Sigma)$ as an invariant subspace of the computational space $W = X^{\otimes k}$, where $X = \bigoplus_{(a,b,c) \in L^3} V_{abc}$ is the "qupit" space (the direct sum of all pants Hilbert spaces) and $k$ is the number of pants in a decomposition of $\Sigma$.

The steps:

1. **Choose a pants decomposition** $D$ of $\Sigma$ — cut $\Sigma$ into $k$ three-punctured spheres ("pants") along "cuff" curves.

2. **The gluing axiom gives an embedding** $i_D: V(\Sigma) \hookrightarrow W = X^{\otimes k}$. Each pants contributes one tensor factor $X$, and the gluing sums over labels on the cuffs.

3. **F-moves and S-moves** change the pants decomposition. An F-move acts on a 4-punctured sphere (two adjacent pants), replacing one cuff with another. An S-move does the same on a punctured torus. Under the functor, these become unitary maps $F: X \otimes X \to X \otimes X$ (two-qupit gates) or $S: X \to X$ (one-qupit gates).

4. **Dehn twists and braid moves** along cuff curves of the *current* pants decomposition act as single-qupit gates $\theta(\omega): X \to X$ on the relevant tensor factor.

5. **To apply a generator along a non-cuff curve $\omega$:** use $O(\log b_1(\Sigma))$ F- and S-moves to transform the pants decomposition until $\omega$ becomes a cuff, apply the generator, then reverse the moves.

### The braid case (genus 0, identical labels)

For a punctured sphere with $q$ punctures and all labels equal:
- Two systematic pants decompositions $\vec{\alpha}$ and $\vec{\beta}$ are related by $q$ F-moves
- Braid generators along $\alpha$-curves act locally on $i_\alpha(V(\Sigma))$; generators along $\beta$-curves act locally on $i_\beta(V(\Sigma))$
- The transformation $T: W \to W$ (composition of $q$ extended F-moves) satisfies $T \circ i_\alpha = i_\beta$

A word $\omega$ in the braid group is simulated by composing gates $T$, $T^{-1}$, $\theta(\alpha_i)$, $\theta(\beta_j)$. For example, $\beta_5 \alpha_1 \beta_2^{-1} \alpha_1 \alpha_3$ is simulated as:

$$\tau = T^{-1} \circ \theta(\beta_5) \circ T \circ \theta(\alpha_1) \circ T^{-1} \circ \theta(\beta_2^{-1}) \circ T \circ \theta(\alpha_1) \circ \theta(\alpha_3)$$

With a localised variant (only modifying the decomposition near the relevant curve), the length satisfies $\text{length}(\tau) \leq 7 \cdot \text{length}(\omega)$.

### The general case

For arbitrary genus and labels, many more embeddings $i_D$ are needed (one per pants decomposition encountered). Each Dehn twist or braid move along a curve $\omega$ is handled by:
1. F/S moves from the base decomposition $D_0$ to a decomposition $D_\omega$ containing $\omega$ as a cuff: cost $O(\log b_1(\Sigma))$
2. Apply the generator $\theta(\omega)$: cost 1
3. Reverse the moves back to $D_0$: cost $O(\log b_1(\Sigma))$

Total: $\text{length}(\tau) \leq c \cdot n \cdot \log b_1(\Sigma)$.

### Extension to TQFTs (bordisms)

Non-product bordisms (3-manifolds with boundary) correspond to 2-handle attachments, which act as projectors on $V(\Sigma)$. These are simulated by intermediate measurements — the quantum circuit continues only if the desired measurement outcome occurs. This gives a "partial quantum circuit" (probabilistically abortive computation).

---

## Key results

**Theorem 2.2:** For a UTMF $V$ and a diffeomorphism $h$ of length $n$ in standard mapping class group generators, $V(h)$ is *exactly* simulated by a quantum circuit over $\mathbb{C}^p$ (qupits) of length $\leq c(V) \cdot n \cdot \log b_1(\Sigma)$.

**Extension:** If $h$ is written as a composition of Dehn twists and braid moves along curves of combinatorial length $\ell(h)$, then the simulation has length $\leq 11 \cdot \ell(h)$.

**Scholium 3.1:** For a TQFT bordism $b: \Sigma_0 \to \Sigma_1$ with complexity (number of F, S, M moves and 2-handle attachments) $= c$, the bordism map $b_*$ is simulated by a partial quantum circuit of length $\leq c'(V) \cdot c$.

---

## Comparison with prior/subsequent work

| Paper | What it does | Relationship |
|---|---|---|
| [[Simulating Physics with Computers (Feynman 1982) — Paper Notes\|Feynman (1982)]] | Proposes quantum simulation | FKW extends the programme to topological theories |
| [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes\|Lloyd (1996)]] | Simulates local $H \neq 0$ systems | FKW handles $H \equiv 0$ (topological) case |
| Freedman-Larsen-Wang (2002) | Shows certain UTMFs are *universal* for QC | Converse direction: TQFT can simulate QC |
| [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes\|AJL (2006)]] | Quantum algorithm for Jones polynomial | The "new algorithm" that TQFTs inspired |
| [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes\|Aharonov-Arad (2011)]] | BQP-hardness for polynomial $k$ | Extends FLW universality result |

---

## Limits / caveats

- The simulation is *exact*, which is stronger than the bounded-error model (BQP). But the paper notes that threshold theorems (Kitaev, Aharonov-Ben-Or, Knill-Laflamme-Zurek) handle the noisy case with only polylog overhead.
- The converse question — can TQFTs efficiently simulate universal quantum computation? — is conjectured yes but was open at the time of writing. It was later answered affirmatively by Freedman-Larsen-Wang (2002, companion paper) for specific TQFTs.
- The algebraic axiom (all basic data algebraic over $\mathbb{Q}$) is needed to avoid pathological examples but holds for all known TQFTs.
- The simulation uses qupits of dimension $p = \dim(X)$, not qubits. This is a notational convenience — any bounded-dimensional factor gives an equivalent theory.

---

## Reusable ideas

1. [[Pants Decomposition Embedding for TQFT Simulation]] — The technique of embedding a topologically-defined state space (no tensor product structure) into a tensor product space via pants decomposition, then simulating topological operations as sequences of local gates (F-moves and S-moves). The key insight is that the gluing axiom provides the embedding, and elementary moves between decompositions translate to local gates.

2. [[F-Move and S-Move as Quantum Gates]] — Using the recoupling data (6j-symbols / F-matrices and S-matrices) of a modular functor as quantum gates. These encode the change of basis when switching between different pants decompositions. Each is a 1- or 2-qupit gate, and sequences of them navigate between different embeddings of $V(\Sigma)$.

---

## References within this paper

- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes|Feynman (1982)]] — origin of the quantum simulation programme; FKW extends it to topological theories
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]] — Hamiltonian simulation via Trotter product formula; FKW handles the $H = 0$ case where Trotter doesn't apply
- Freedman, *P/NP, and the quantum field computer* (1998) — the observation that TQFT transformations produce computationally hard evaluations (Jones/Tutte polynomials)
- Kitaev, *Quantum computations: algorithms and error correction* (1997) — threshold theorem for fault tolerance; rapid approximation of gates
- Jones (1987) — the Jones polynomial, whose evaluation connects to TQFT via Witten's work
- Witten (1989) — connects Chern-Simons theory to the Jones polynomial
- Hatcher-Thurston (1980) — presentation of mapping class groups; completeness of F, S, M moves

---

## Cross-links

### Paper notes
- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes]] — the vision paper that started quantum simulation
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]] — Hamiltonian simulation for $H \neq 0$
- [[A Polynomial Quantum Algorithm for Approximating the Jones Polynomial (Aharonov-Jones-Landau 2006) — Paper Notes]] — the quantum algorithm that TQFTs inspired
- [[The BQP-Hardness of Approximating the Jones Polynomial (Aharonov-Arad 2011) — Paper Notes]] — universality via Bridge and Decoupling Lemmas
- [[Polynomial Quantum Algorithms for the Tutte Plane (Aharonov-Arad-Eban-Landau 2007) — Paper Notes]] — extends to Tutte polynomial

### Trick cards
- [[Pants Decomposition Embedding for TQFT Simulation]]
- [[F-Move and S-Move as Quantum Gates]]
- [[Algebraic Gate Design via Temperley-Lieb Representations]]
