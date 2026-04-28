> **Source:** Andrew M. Childs, Robin Kothari, Matt Kovacs-Deak, Aarthi Sundaram, Daochen Wang, *Quantum divide and conquer*, ACM Transactions on Quantum Computing **6**(2), Article 17 (2025); arXiv:2210.06419
> **Links:** [arXiv](https://arxiv.org/abs/2210.06419) · [ACM TQC](https://dl.acm.org/doi/10.1145/3723884)
> **Tags:** #query-complexity #divide-and-conquer #adversary-method #span-programs #string-problems #quantum-speedup

---

## The computational problem

**Setting:** Quantum query complexity of combinatorial string problems — compute a function $f$ on an input of size $n$ using as few quantum oracle queries as possible. The question is whether classical divide-and-conquer algorithms admit systematic quantum analogues that yield genuine, non-trivial speedups.

Classical divide and conquer splits a problem of size $n$ into $a$ subproblems of size $n/b$ plus auxiliary work $C^{\text{aux}}(n)$, giving the recurrence
$$C(n) \leq a \cdot C(n/b) + C^{\text{aux}}(n).$$
Solving this via the master theorem gives the classical complexity. The paper asks: can quantum computation systematically replace $a$ by $\sqrt{a}$ in the recurrence?

---

## What the paper does

Introduces a **quantum divide-and-conquer framework** that, under appropriate conditions, converts the classical recurrence into the quantum recurrence
$$C_Q(n) \leq \sqrt{a} \cdot C_Q(n/b) + O\!\left(C^{\text{aux}}_Q(n)\right).$$

The mechanism exploits a bidirectional, complexity-preserving correspondence between **span programs** (whose complexity equals the [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|general adversary bound]]) and bounded-error quantum algorithms. Crucially, the adversary method serves as both an upper *and* lower bound, so the recurrence simultaneously characterises achievability and optimality.

The speedups obtained are not reachable by standard tools such as Grover search, [[Standard Amplitude Amplification.md|amplitude amplification]], or [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|quantum walk search]] applied off-the-shelf. They require genuinely recursive use of the adversary structure.

---

## The algorithm / construction

**Core construction:** Given a recursive decomposition of a problem into $a$ independent subproblems of size $n/b$ and auxiliary work, build a span program for the full problem by combining span programs for the subproblems using a quantum "AND of ORs" (or appropriate Boolean combination) composition, then apply the span-program-to-algorithm conversion of [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|Špalek-Szegedy 2006]] / Reichardt.

The adversary quantity satisfies $\text{Adv}(f) \leq \sqrt{a} \cdot \text{Adv}(f_{n/b}) + O\!\left(C^{\text{aux}}_Q(n)\right)$ when the $a$ subproblems contribute adversary witnesses that are "orthogonally combined." Solving this recurrence yields the quantum query complexity directly.

At each recursive level, adversary and algorithmic techniques are interleaved: the adversary bound provides the composition guarantee, while the algorithm realises the resulting span program using $O(\text{Adv}(f))$ queries.

**Applications:**

1. **Regular language recognition:** Given a string $x \in \{0,1\}^n$ and a fixed DFA $M$ with $s$ states, determine whether $M$ accepts $x$. Classically $\Theta(n)$ queries. Quantum: $\tilde{O}(n^{1/2})$, tight up to log factors by the adversary lower bound. The divide step splits the string in half; the span program composes automata states.

2. **String rotation (decision version):** Is $x$ a cyclic rotation of a fixed string $y$? Near-optimal quantum query complexity via the framework.

3. **String suffix (decision version):** Is $x$ a suffix of $y$? Similar.

4. **Longest increasing subsequence (LIS):** For the natural parameterised version (is $\text{LIS}(x) \geq k$?), the framework gives an improved query complexity.

5. **Longest common subsequence (LCS):** Parameterised version similarly benefits.

---

## Key results

- **Main theorem:** The quantum divide-and-conquer recurrence $C_Q(n) \leq \sqrt{a} \cdot C_Q(n/b) + O(C^{\text{aux}}_Q(n))$ holds whenever the classical algorithm has the corresponding structure and the subproblems admit independent span program witnesses.

- **Regular languages:** $\tilde{O}(\sqrt{n})$ quantum query complexity for any fixed DFA. This matches the adversary lower bound and is a quadratic improvement over the classical $\Theta(n)$.

- **String rotation / suffix:** Near-optimal quantum query complexities, which were previously unknown.

- **LIS / LCS:** Improved parameterised quantum query complexities that do not follow from any previously known technique.

- **Tightness:** In all cases the adversary method (which is tight for total Boolean functions by [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|Špalek-Szegedy]]) certifies that the obtained complexities are essentially optimal, so the framework does not leave a gap.

---

## Comparison with prior work

| Technique | What it does | Where it falls short for these problems |
|---|---|---|
| Grover search | $\sqrt{N}$ speedup for unstructured search | No recursive structure, can't compose |
| Amplitude amplification | Quadratic speedup for known subroutines | Doesn't give the $\sqrt{a}$ composition |
| Quantum walks (Szegedy) | Quadratic speedup for Markov chains | Requires Markov structure, not string recursion |
| [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|AND-OR formula evaluation]] | $\sqrt{N}$ for balanced formulas | Restricted to AND-OR structure |
| **This paper** | General divide-and-conquer with $\sqrt{a}$ | Requires classical D&C structure to exist |

The paper is a natural companion to [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes|NAND tree evaluation]] and [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes|AND-OR formula evaluation]], which also exploit recursive quantum walk / adversary composition — but those are restricted to specific formula structures, while this paper provides a general framework applicable to string problems with arbitrary DFA structure.

Prior to this work, no systematic method existed for converting classical divide-and-conquer into quantum speedups for problems outside the formula-evaluation paradigm. The key novelty is using the two-way adversary-algorithm correspondence to guarantee both achievability and optimality simultaneously, without crafting a new ad hoc algorithm for each problem.

---

## Limits / caveats

- The framework applies to **query complexity**, not gate complexity. Translating to an efficient circuit requires an efficient span-program-to-circuit conversion, which may add polylogarithmic overhead.
- The $\sqrt{a}$ speedup requires that the $a$ subproblems be **independent** (no entanglement between inputs to different subproblems). Correlated subproblems are not handled.
- Auxiliary work $C^{\text{aux}}_Q(n)$ must already be known and efficient. If the auxiliary work dominates, the recurrence gives no speedup over the classical analogue.
- The framework yields query complexity optimality (via the adversary bound), but it does not immediately imply that no better algorithm exists in non-query models.
- Applications are to **decision problems** over strings; extending to search or approximation versions requires additional ideas.

---

## Reusable ideas

- **$\sqrt{a}$ composition rule:** Whenever a classical algorithm recurses into $a$ independent branches, one can hope for a quantum algorithm with $\sqrt{a}$ factor instead of $a$ at each level, by combining span programs rather than classical OR.
- **Adversary as upper bound:** The general adversary bound is not just a lower bound tool. Via the span program equivalence ([[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes|Špalek-Szegedy 2006]]), it immediately constructs the algorithm that achieves the bound.
- **Interleaving levels:** At each recursion depth, the adversary and algorithmic picture can be switched freely. This gives flexibility in proving the recurrence for whichever subproblems are easier to analyse in one language vs. the other.
- **Regular language structure:** The DFA state propagation under the divide step is the key technical ingredient. This idea may transfer to other problems with automaton-like recursive structure (e.g., context-free language recognition).

---

## References within this paper

- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]] — span program ↔ algorithm correspondence
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]] — original adversary method
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]] — polynomial method baseline
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] — precursor: recursive adversary for formulas
- [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes]] — related formula evaluation
- [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes]] — polynomial method lower bounds

---

## Cross-links

**Papers that use or extend the adversary/span-program machinery this paper builds on:**
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]
- [[Quantum Lower Bounds by Quantum Arguments (Ambainis 2000) — Paper Notes]]
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]]

**Other Childs query-complexity work:**
- [[An Example of the Difference Between Quantum and Classical Random Walks (Childs-Farhi-Gutmann 2002) — Paper Notes]]
- [[Exponential Quantum Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]]
- [[Symmetries, Graph Properties, and Quantum Speedups (Ben-David-Childs-Gilyén-Kretschmer-Podder-Wang 2020) — Paper Notes]]
- [[Quantum Property Testing for Bounded-Degree Graphs (Ambainis-Childs-Liu 2011) — Paper Notes]]

**Lower bound context:**
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[Optimal Lower Bounds for Quantum Automata and Random Access Codes (Nayak 1999) — Paper Notes]]
