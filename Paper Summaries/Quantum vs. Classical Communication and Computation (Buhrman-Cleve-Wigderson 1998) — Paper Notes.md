> **Source:** Harry Buhrman, Richard Cleve, Avi Wigderson, *Quantum vs. Classical Communication and Computation*, STOC 1998; arXiv:quant-ph/9802040
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/9802040) · [STOC](https://doi.org/10.1145/276698.276713)
> **Tags:** #communication-complexity #query-complexity #exponential-gap #quadratic-gap #foundational

---

## The computational problem

Two-party communication complexity: Alice holds $g : \{0,1\}^n \to \{0,1\}$, Bob holds $h : \{0,1\}^n \to \{0,1\}$ (each encoding an $N = 2^n$-bit string). They want to compute some function of $g$ and $h$ while minimising qubit/bit communication. The paper also studies black-box query complexity, where $f : \{0,1\}^n \to \{0,1\}$ is accessed via oracle queries.

The key problems are:
- **Disjointness (DISJ):** $\mathrm{DISJ}(f,g) = \bigvee_x (f(x) \wedge g(x))$
- **Equality variant (EQ'):** A partial function where $\mathrm{EQ}'(f,g) = 1$ if $\Delta(f,g) = 0$ and $\mathrm{EQ}'(f,g) = 0$ if $\Delta(f,g) = N/2$

---

## What the paper does

Introduces a general simulation technique that converts any quantum black-box algorithm making $t$ oracle calls into a quantum communication protocol with $O(tn)$ qubits of communication. This single idea, combined with existing quantum algorithms and classical lower bounds, produces the first asymptotic separations between quantum and classical two-party communication complexity: a near-quadratic gap for bounded-error (DISJ) and an exponential gap for exact vs. zero-error (EQ'). Running the reduction backwards also gives new black-box lower bounds (PARITY, MAJORITY).

---

## The simulation technique

### The core reduction (Theorem 2.1)

This is the central construction. Given any quantum algorithm computing $F(f)$ using $t$ oracle calls to $f$, the reduction produces a communication protocol for computing $F(L(g,h))$ where $L : \{0,1\}^2 \to \{0,1\}$ combines Alice's and Bob's bits pointwise: $L(g,h)(x) = L(g(x), h(x))$.

**Protocol for each oracle call:** The state being processed is in superposition $\sum \lambda_{x,y} |x\rangle|y\rangle$. To simulate an $L(g,h)$-gate:

1. Alice appends ancilla $|0\rangle$, applies $|x\rangle|y\rangle|0\rangle \mapsto |x\rangle|y\rangle|g(x)\rangle$
2. Alice sends $n+2$ qubits to Bob
3. Bob applies $|x\rangle|y\rangle|g(x)\rangle \mapsto |x\rangle|L(g(x),h(x)) \oplus y\rangle|g(x)\rangle$
4. Bob sends $n+2$ qubits back to Alice
5. Alice uncomputes: $|x\rangle|L(g(x),h(x)) \oplus y\rangle|g(x)\rangle \mapsto |x\rangle|L(g(x),h(x)) \oplus y\rangle|0\rangle$

Each oracle call costs $2(n+2)$ qubits of communication. The reduction preserves the success probability exactly.

The key insight: quantum parallelism means the simulated oracle call works on the full superposition simultaneously — Alice and Bob don't need to communicate separately for each branch.

### Deriving the communication results

| Setting $L$ to | And $F$ to | Gets you | Via |
|---|---|---|---|
| AND | OR | DISJ protocol | [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes\|Grover's algorithm]] |
| XOR | BAL (Deutsch-Jozsa) | EQ' protocol | [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes\|Deutsch-Jozsa algorithm]] |
| AND | SIGMA$_d$ | AC$^0$ formula protocol | Nested Grover (this paper) |

### Deriving black-box lower bounds (reverse direction)

Setting $L = $ AND and $F = $ PARITY gives the inner product communication problem. Since $Q(\mathrm{IP}) \in \Omega(N)$, and the simulation uses $t \cdot O(n)$ qubits, we get $T(\mathrm{PARITY}) \in \Omega(2^n/n)$. The MAJORITY lower bound follows by reduction from PARITY.

---

## Key results

**Theorem 1.6 (Quadratic gap for DISJ):**

$$Q(\mathrm{DISJ}) \in O(\sqrt{N} \log N), \qquad Q_0(\mathrm{DISJ}) \in \Omega(N)$$

Combined with the classical $C(\mathrm{DISJ}) \in \Omega(N)$ (Kalyanasundaram-Schnitger / Razborov), this gives a near-quadratic separation between bounded-error quantum and classical communication complexity.

**Theorem 1.7 (Exponential gap for EQ'):**

$$Q_0(\mathrm{EQ}') \in O(\log N), \qquad C_0(\mathrm{EQ}'), C^E_0(\mathrm{EQ}') \in \Omega(N)$$

The upper bound uses the Deutsch-Jozsa algorithm (1 oracle call → $O(\log N)$ qubits). The classical lower bound uses the Frankl-Rödl forbidden intersection theorem, which bounds the size of set families avoiding a given Hamming distance.

**Theorem 1.12 (PARITY lower bound):**

$$T(\mathrm{PARITY}) \in \Omega(2^n/n)$$

**Theorem 1.15 (Near-quadratic speedup for bounded-depth predicates):**

$$T(\mathrm{SIGMA}_d) \in O(\sqrt{2^n} \cdot n^{d-1})$$

Achieved by nesting Grover search: approximate the inner gates, then search over the outer gates. Uses the $G$-$G^\dagger$ trick for approximate oracle simulation.

---

## Comparison with prior and subsequent work

| Result | Type | Gap |
|---|---|---|
| This paper (DISJ, bounded-error) | Total function | Near-quadratic |
| This paper (EQ', exact vs. zero-error) | Partial function | Exponential |
| [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes\|Buhrman-Cleve-Watrous-de Wolf (2001)]] | Total function (SMP model) | Exponential |
| [[Exponential Separation of Quantum and Classical Communication Complexity (Raz 1999) — Paper Notes\|Raz (1999)]] | Partial function (standard model) | Exponential |

This paper came first chronologically. The exponential gap for EQ' is for a partial function and in the exact/zero-error model (not bounded-error). Getting an exponential separation for a *total* function required the SMP model and [[Quantum Fingerprinting via Error-Correcting Codes|quantum fingerprinting]] (Buhrman-Cleve-Watrous-de Wolf 2001). Getting it for a partial function in the standard bounded-error model was Raz (1999).

The paper notes that Beals-Buhrman-Cleve-Mosca-de Wolf (1998) shows any total black-box function with $T$ quantum queries has a classical algorithm with $O(T^6)$ queries — so the simulation approach here can never yield more than a sixth-root gap for total functions.

---

## Limits / caveats

- The DISJ separation is only near-quadratic ($O(\sqrt{N} \log N)$ vs. $\Omega(N)$). Closing the log factor was left open.
- The exponential gap (EQ') requires a partial function — the result says nothing about total functions in the standard model.
- The simulation adds an $O(n)$ overhead per oracle call, so the communication protocol is never as tight as a purpose-built quantum protocol.
- The PARITY and MAJORITY lower bounds were subsequently tightened to optimal by Beals et al. (1998) and Farhi et al. (1998).
- The paper's approach inherently can't separate bounded-error quantum from classical by more than a polynomial for total functions, due to the polynomial method.

---

## Reusable ideas

1. **[[Black-Box to Communication Reduction]]:** Any $t$-query quantum algorithm becomes a $t \cdot O(n)$-qubit communication protocol by having Alice and Bob jointly simulate each oracle call. The quantum parallelism is preserved because the simulation acts on the full superposition. Running the reduction backwards converts communication lower bounds to query lower bounds.

2. **[[Approximate Oracle via Compute-CNOT-Uncompute]]:** To build an approximate oracle for a derived function $g$ (like AND over a subset of inputs), run a quantum algorithm $G$ that computes $g$ with high probability, CNOT the answer onto a target qubit, then uncompute with $G^\dagger$. The approximation error is $O(\sqrt{2^n} \cdot 2^{-k/2})$ for $k$ repetitions. This makes approximate oracles composable for nested search.

3. **[[Nested Grover Search for Bounded-Depth Predicates]]:** For $\Sigma_d$-type predicates (alternating quantifiers), nest $d$ levels of Grover search. At each level, use approximate oracles for the inner levels. Total cost: $O(\sqrt{2^n} \cdot n^{d-1})$ — a near-quadratic speedup for any fixed depth $d$.

---

## References within this paper

- Yao (1979, 1993) — introduced both classical communication complexity and quantum communication complexity
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes|Deutsch-Jozsa (1992)]] — the balanced-vs-constant algorithm, used here for the EQ' protocol
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — database search, basis for the DISJ protocol
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes|Boyer-Brassard-Høyer-Tapp (1998)]] — refined Grover analysis, used for unknown-$t$ search subroutine
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994)]] — exponential black-box speedup (different setting)
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — Abelian HSP, mentioned as a generalisation of Simon
- Kalyanasundaram-Schnitger (1987) / Razborov (1990) — classical $\Omega(N)$ lower bound for DISJ
- Kremer (1995) — MSc thesis establishing basic quantum communication complexity framework; $\Omega(N)$ lower bound for IP
- Holevo (1973) — $m$ qubits carry at most $m$ classical bits; shows quantum advantage isn't trivial
- Frankl-Rödl (1987) — forbidden intersection theorem, used for the EQ' classical lower bound
- Beals-Buhrman-Cleve-Mosca-de Wolf (1998) — polynomial method for black-box quantum lower bounds; $O(T^6)$ classical simulation of $T$-query quantum algorithms for total functions
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes|Bennett-Bernstein-Brassard-Vazirani (1997)]] — $\Omega(\sqrt{N})$ search lower bound; quantum subroutine error accumulation

---

## Cross-links

### Paper notes
- [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]] — direct successor; achieves exponential separation for a total function in the SMP model using [[Quantum Fingerprinting via Error-Correcting Codes|quantum fingerprints]]
- [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]] — the Deutsch-Jozsa algorithm is the engine behind the EQ' protocol
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — Grover search drives the DISJ protocol
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]] — refined analysis used for the unknown-$t$ oracle simulation
- [[Strengths and Weaknesses of Quantum Computing (Bennett-Bernstein-Brassard-Vazirani 1997) — Paper Notes]] — the error accumulation bound that justifies composing approximate oracles
- [[Exponential Separation of Quantum and Classical Communication Complexity (Raz 1999) — Paper Notes]] — achieves exponential bounded-error separation for a partial function, building on the questions raised here
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]] — later achieves the optimal query separation; related theme of quantum-classical gaps

### Trick cards
- [[Black-Box to Communication Reduction]]
- [[Approximate Oracle via Compute-CNOT-Uncompute]]
- [[Nested Grover Search for Bounded-Depth Predicates]]
- [[Quantum Fingerprinting via Error-Correcting Codes]] — the 2001 follow-up technique
- [[Swap Test for State Comparison]] — used in the 2001 follow-up
