> **Source:** Ran Raz, *Exponential Separation of Quantum and Classical Communication Complexity*, STOC 1999, pp. 358–367
> **Links:** [ACM](https://doi.org/10.1145/301250.301343)
> **Tags:** #communication-complexity #exponential-gap #distributional-method #partial-function #foundational

---

## The computational problem

**Two-party communication complexity (standard model):** Alice holds input $x$, Bob holds input $y$, and they communicate interactively (qubits or bits) to compute a joint function $f(x,y)$. Unlike the SMP model (where Alice and Bob each send one message to a referee), this is the full interactive model where messages go back and forth.

Raz defines a partial function (promise problem) where Alice receives a unit vector $v \in \mathbb{R}^m$ and Bob receives a subspace $H$ of $\mathbb{R}^m$ of dimension $m/2$. The promise is:
- **YES:** $v \in H$ (the vector lies in the subspace)
- **NO:** $v$ is "far" from $H$ (specifically, $\|P_H v\| \leq \delta$ for some small constant $\delta$, where $P_H$ is the orthogonal projection onto $H$)

The inputs are discretised to finite precision. The parameter $n$ controls the input size (both $v$ and $H$ are specified using $O(n)$ bits total).

---

## What the paper does

This paper gives the first unconditional exponential separation between quantum and classical communication complexity in the standard two-party interactive model. For Raz's partial function:

$$Q(\text{Raz}) \in O(\log n), \qquad R(\text{Raz}) \in \Omega(n^{1/4})$$

where $Q$ denotes quantum bounded-error communication complexity and $R$ denotes classical randomised bounded-error communication complexity. The quantum protocol uses $O(\log n)$ qubits of communication (two rounds), while any classical protocol needs polynomially many bits.

This was a major open problem. Prior to Raz, the best known separations were:
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes|Buhrman-Cleve-Wigderson (1998)]]: near-quadratic for a total function (DISJ), exponential for a partial function but only in the exact/zero-error setting
- No exponential separation in the bounded-error model was known

---

## The quantum protocol

Alice's vector $v$ and Bob's subspace $H$ are encoded as quantum states. The key idea is that quantum communication can transmit an exponential amount of "geometric information" about a high-dimensional vector in $O(\log n)$ qubits, because a quantum state in $k$ qubits already lives in a $2^k$-dimensional Hilbert space.

**Alice's encoding:** Encode $v \in \mathbb{R}^m$ as a quantum state $|v\rangle$ on $\log m = O(\log n)$ qubits.

**Bob's test:** Bob can perform a measurement that projects onto his subspace $H$ (or its complement) to determine whether $|v\rangle$ has large or small overlap with $H$.

The protocol uses two rounds of communication and $O(\log n)$ qubits total. The YES case gives overlap 1 (vector is in the subspace), the NO case gives overlap $\leq \delta$ (essentially orthogonal). A single swap test or projection measurement distinguishes these cases with constant probability.

---

## The classical lower bound

The classical lower bound is the technically harder part. Raz introduces a **distributional argument**: he constructs a hard input distribution $\mu$ and shows that any deterministic protocol using few bits of communication cannot distinguish YES from NO instances drawn from $\mu$.

The key technical ingredient is a **norm bound on communication matrices.** For a protocol that exchanges $c$ bits, the communication matrix (describing the correlation between Alice's and Bob's outputs) has at most $2^c$ distinct rows. Raz shows that the "distinguishing power" of any such low-communication protocol against the hard distribution is bounded by an expression that decays exponentially in $m$ but only grows polynomially in $2^c$. This forces $c = \Omega(n^{1/4})$.

The argument uses properties of the uniform (Haar) measure on subspaces and the fact that high-dimensional random subspaces are nearly orthogonal to most vectors — a geometric concentration phenomenon.

By Yao's minimax principle, the distributional lower bound implies a randomised communication lower bound.

---

## Key results

**Main theorem:**

There exists a partial function $f$ on $n$-bit inputs such that:

$$Q_{1/3}(f) \in O(\log n), \qquad R_{1/3}(f) \in \Omega(n^{1/4})$$

This is an exponential separation: $O(\log n)$ vs. $\Omega(n^{1/4})$ = $\Omega(2^{\Omega(\log n)})$ relative to $\log n$.

**Subsequent improvement (journal version and later work):** The classical lower bound was later improved to $\Omega(\sqrt{n})$ in the journal version, and Klartag and Regev (2011) extended the separation to one-way quantum protocols, settling a long-standing open question.

---

## Comparison with prior and subsequent work

| Result | Model | Function type | Separation |
|---|---|---|---|
| [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes\|BCW (1998)]] | Standard, bounded-error | Total (DISJ) | Near-quadratic |
| [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes\|BCW (1998)]] | Standard, exact/zero-error | Partial (EQ') | Exponential |
| **This paper** | **Standard, bounded-error** | **Partial** | **Exponential** |
| [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes\|BCWW (2001)]] | SMP | Total (equality) | Exponential |
| Klartag-Regev (2011) | One-way | Partial | Exponential |

Raz's paper filled the most important gap: an exponential separation in the standard bounded-error model. The limitation is that it uses a partial function — the question of whether an exponential separation exists for a total function in the standard model remained open (and is related to the BQP vs. BPP question).

---

## Limits / caveats

- The function is a **partial function** (promise problem). The separation says nothing about total functions in the standard model.
- The quantum protocol uses **two rounds** of communication. Whether one-way quantum communication suffices for an exponential separation was open until Klartag-Regev (2011).
- The original STOC lower bound is $\Omega(n^{1/4})$, not the full $\Omega(\sqrt{n})$ or $\Omega(n)$. The gap between the quantum upper bound $O(\log n)$ and the classical lower bound, while exponential, has a polynomial base that was later improved.
- The inputs require finite-precision representation of real vectors and subspaces, which involves some discretisation machinery.
- The paper was not posted to arXiv (published only in STOC proceedings and later in a journal version).

---

## Reusable ideas

1. **[[Distributional Method for Quantum Communication Lower Bounds]]:** Construct a hard input distribution where YES and NO instances are geometrically "close" under any low-communication classical protocol. Use the fact that $c$ bits of classical communication can only distinguish $2^c$ distinct behaviors, while the underlying geometric space has dimension exponential in $n$. Combined with Yao's minimax, this gives a lower bound on randomised communication complexity. This distributional technique became a standard tool for subsequent quantum communication separations.

2. **[[Quantum State as Geometric Encoding]]:** Encode a high-dimensional real vector $v \in \mathbb{R}^m$ as a quantum state $|v\rangle$ on $\log m$ qubits. The exponential dimension of Hilbert space means $O(\log n)$ qubits can carry geometric information about an $n$-dimensional vector. Bob can then test geometric properties (subspace membership, inner products) via projective measurements. This encoding is at the heart of quantum fingerprinting, quantum data structures, and many other quantum communication advantages.

---

## References within this paper

- Yao (1979, 1993) — communication complexity model; minimax principle for distributional lower bounds
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes|Buhrman-Cleve-Wigderson (1998)]] — the direct predecessor; near-quadratic bounded-error separation for DISJ, exponential exact/zero-error separation for EQ'
- Kremer (1995) — basic quantum communication complexity framework
- Holevo (1973) — $m$ qubits convey at most $m$ classical bits (yet Raz shows quantum communication complexity can still be exponentially smaller)
- Nayak (1999) — quantum lower bounds for certain communication problems
- Ambainis, Schulman, Ta-Shma, Vazirani, Wigderson (1999) — quantum lower bounds for sampling problems

---

## Cross-links

### Paper notes
- [[Quantum vs. Classical Communication and Computation (Buhrman-Cleve-Wigderson 1998) — Paper Notes]] — direct predecessor; the two papers together establish the landscape of quantum-classical communication separations
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]] — achieves exponential separation for a total function but in the SMP model; uses the same geometric encoding idea
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] — later optimal quantum-classical query separation; related theme of using Fourier/geometric structure
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]] — relates communication separations to circuit complexity separations

### Trick cards
- [[Distributional Method for Quantum Communication Lower Bounds]]
- [[Quantum State as Geometric Encoding]]
- [[Quantum Fingerprinting via Error-Correcting Codes]] — a different encoding approach achieving the same exponential advantage
- [[Black-Box to Communication Reduction]] — Buhrman-Cleve-Wigderson's general technique relating the two settings
