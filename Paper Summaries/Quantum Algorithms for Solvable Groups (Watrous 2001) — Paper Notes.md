> **Source:** John Watrous, *Quantum algorithms for solvable groups*, arXiv:quant-ph/0011023 (2001); Proc. 33rd STOC, 60–67 (2001)
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0011023)
> **Tags:** #hidden-subgroup #solvable-groups #group-theory #black-box-groups #non-abelian

---

## The computational problem

**Setting:** Black-box groups (Babai-Szemerédi 1984). Group elements are $n$-bit strings; a group oracle computes $|g\rangle|h\rangle \mapsto |g\rangle|gh\rangle$ at unit cost. No classical polynomial-time algorithm can compute the order of a black-box group, even for abelian groups.

**Main result:** Polynomial-time quantum algorithm for computing the order of a solvable group given generators. As byproducts: membership testing, subgroup equality, normality testing — all in quantum polynomial time.

---

## What the paper does

Extends quantum group algorithms beyond the abelian case for the first time (in terms of complete solutions, not just HSP reductions). The key insight: solvable groups have a **subnormal series** with cyclic factors — a chain

$$
\{1\} = H_0 \trianglelefteq H_1 \trianglelefteq \cdots \trianglelefteq H_m = G
$$

where each $H_j/H_{j-1}$ is cyclic of order $r_j$. The group order is $|G| = \prod_{j=1}^m r_j$. The algorithm climbs this chain, at each step:
1. Using copies of $|H_{j-1}\rangle$ to find $r_j$ (via Shor-style order finding)
2. Converting copies of $|H_{j-1}\rangle$ into copies of $|H_j\rangle$

The remarkable feature: the algorithm produces the **uniform superposition** $|H\rangle = |H|^{-1/2}\sum_{h \in H} |h\rangle$ over any subgroup $H$, despite having no way to enumerate or even classically represent $H$'s elements. This quantum state serves as a proxy for the subgroup itself.

---

## The algorithm

### Preprocessing (classical)

Given generators $g_1, \ldots, g_k$ of $G$:
1. Test solvability via Monte Carlo algorithm (Babai et al. 1995)
2. Compute derived series $G^{(0)} = G \supset G^{(1)} = [G,G] \supset \cdots \supset G^{(n)} = \{1\}$
3. Relabel to get elements $h_1, \ldots, h_m$ with $H_j = \langle h_1, \ldots, h_j \rangle$ forming a subnormal series with cyclic factors

### Step 1: Order finding relative to a subgroup

Given copies of $|H\rangle$ and an element $g \in G$, find $r = r_H(g)$, the smallest positive integer such that $g^r \in H$.

This is Shor's order-finding algorithm with a twist: **register 2 starts in $|H\rangle$ instead of $|1\rangle$**.

1. Register $A$: prepare uniform superposition $\frac{1}{\sqrt{N}}\sum_{a=0}^{N-1}|a\rangle$
2. Register $R$: initialise to $|H\rangle$
3. Apply QFT$_N$ to $A$, left-multiply $R$ by $g^a$, apply QFT$_N^\dagger$ to $A$
4. Measure $A$ → get $b$, where $b/N \approx k/r$ for random $k \in \mathbb{Z}_r$
5. Extract $r$ via continued fractions + LCM over $O(\log(1/\epsilon))$ trials

The state of $R$ after multiplying by $g^a$ is $|g^a H\rangle$ — a uniform superposition over the **coset** $g^a H$. Since $H \trianglelefteq \langle g \rangle H$, the cosets $\{H, gH, g^2H, \ldots, g^{r-1}H\}$ are the distinct cosets. The QFT on $A$ then reveals the period $r$, exactly as in Shor.

### Step 2: Converting $|H\rangle$ to $|\langle g \rangle H\rangle$

Given $l$ copies of $|H\rangle$, $r = r_H(g)$ known, $g$ normalising $H$:

1. For each copy $R_i$, prepare ancilla $A_i$ in $|0\rangle$
2. QFT$_r$ on $A_i$, left-multiply $R_i$ by $g^{a_i}$, QFT$_r$ on $A_i$
3. Measure all $A_i$ → get values $b_1, \ldots, b_l$

After measurement, each register is in state $|\psi_i\rangle = \frac{1}{\sqrt{r}} \sum_{a_i} e_r(a_i b_i)|g^{a_i}H\rangle$.

**The conversion trick:** Pick any $k$ with $\gcd(b_k, r) = 1$. For each $i \neq k$: left-multiply $R_i$ by the contents of $R_k$ raised to the power $c \equiv b_i b_k^{-1} \pmod{r}$.

The key calculation: $|\psi_k\rangle$ is an eigenvector of the left-multiplication operator $M_{g^j h}$ with eigenvalue $e_r(-jb_k)$. So multiplying $R_i$ by $R_k^c$ collapses the phases in $R_i$, converting it to $|\langle g \rangle H\rangle$.

Result: $l-1$ copies of $|\langle g \rangle H\rangle$ from $l$ copies of $|H\rangle$ (one copy used as the "phase reference").

### Main algorithm

Start with $k(m+1)$ copies of $|H_0\rangle = |1\rangle$ (trivial to prepare).

For $j = 1, \ldots, m$:
1. Use $k-1$ copies of $|H_{j-1}\rangle$ to compute $r_j$
2. Use 1 copy as phase reference to convert remaining copies to $|H_j\rangle$

Output $|G| = \prod r_j$.

**Resource budget:** $k = O(n \cdot \log(m/\epsilon))$ copies suffice for total error $< \epsilon$.

---

## Applications

| Problem | Reduction |
|---|---|
| Order of solvable group | Direct (main algorithm) |
| Membership: $h \in \langle g_1, \ldots, g_k \rangle$? | $|\langle g_1, \ldots, g_k\rangle| = |\langle g_1, \ldots, g_k, h\rangle|$? |
| Subgroup equality | Mutual containment via membership |
| Normality testing | $g_i^{-1} h_j g_i \in \langle h_1, \ldots, h_l \rangle$ for all $i, j$? |
| Abelian factor group structure | QFT on factor group using $|H\rangle$ states as coset representatives |

### Computing over factor groups

For $H \trianglelefteq G$ with $G/H$ abelian: represent elements of $G/H$ by quantum states $|gH\rangle$. These states are identical when $gH = g'H$ and orthogonal otherwise. The group oracle on $G$ naturally implements multiplication in $G/H$:

$$
U_G |gH\rangle|g'H\rangle = |gH\rangle|gg'H\rangle
$$

This lets you run abelian group algorithms (structure decomposition, isomorphism testing) on $G/H$ without classical representations of coset elements.

---

## Why this matters

This is the first polynomial-time quantum algorithm for a natural class of **non-abelian** groups that goes beyond the Hidden Subgroup Problem. The HSP approach (Fourier sample from cosets) doesn't directly work for non-abelian groups because the representation theory is harder. Watrous sidesteps this by:

1. Exploiting the **solvability structure** (subnormal series with cyclic factors) rather than trying to solve the general non-abelian HSP
2. Using **quantum states as group-element proxies** ($|H\rangle$ represents the subgroup $H$, even though you can't classically list $H$'s elements)
3. **Climbing the subnormal chain** iteratively, reducing each step to an abelian (Shor-style) problem

The paper doesn't solve graph isomorphism (which would need the full non-abelian HSP for symmetric groups, which are not solvable for $n \geq 5$). But it shows that meaningful non-abelian group problems are quantum-tractable.

### Retracted claim

The original version claimed an isomorphism-testing algorithm for solvable groups, based on comparing derived series factor groups. This was incorrect — isomorphic factor groups don't imply isomorphic groups. (Corrected in v2; acknowledged to Miklós Sántha.)

---

## Reusable ideas

1. **[[Subgroup State Preparation via Subnormal Chain]]:** Build $|H_j\rangle$ iteratively from $|H_{j-1}\rangle$ by QFT over the cyclic factor, left-multiplication, and phase-reference conversion. Converts copies of one subgroup superposition into copies of the next larger subgroup. The "eigenstate as phase reference" trick in the conversion step is particularly elegant.

2. **[[Quantum States as Coset Representatives]]:** Represent elements of a factor group $G/H$ by quantum states $|gH\rangle$ (uniform superposition over the coset). The group oracle implements factor group multiplication automatically. This lets you run abelian algorithms on non-abelian structures by working modulo a normal subgroup.

---

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994/1997)]] — order-finding and QFT; the building block for each step of the subnormal chain
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes|Simon (1994/1997)]] — the original hidden subgroup algorithm
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995/1997)]] — Abelian stabilizer problem framework; generalises Shor's approach
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes|Deutsch (1985)]] — quantum computational model
- Babai & Szemerédi (1984) — black-box group model; proved classical impossibility of polynomial-time order computation
- Babai, Cooperman, Finkelstein, Luks & Seress (1995) — Monte Carlo algorithms for solvability testing, derived series computation
- Beals (1997) — polynomial-time QFT over symmetric groups (doesn't solve graph isomorphism)
- Ettinger & Høyer (1999), Ettinger, Høyer & Knill (1999) — non-abelian HSP hardness results
- Mosca & Ekert (1999) — HSP as eigenvalue estimation framework
- Bennett (1973) — [[Reversible Computation via Compute-Copy-Uncompute|reversible computation]]
- Rötteler & Beth (1998) — HSP for wreath products (another non-abelian class)

---

## Cross-links

### Paper notes
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]] — predecessor: uses $|H\rangle$ as quantum proof for group non-membership; this paper makes $|H\rangle$ preparable
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — the order-finding subroutine used at each step
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — the Abelian HSP special case
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — the Abelian stabilizer framework that Watrous extends
- [[Quantum Theory, the Church-Turing Principle and the Universal Quantum Computer (Deutsch 1985) — Paper Notes]] — computational model

### Trick cards
- [[Subgroup State Preparation via Subnormal Chain]]
- [[Quantum States as Coset Representatives]]
- [[Coset Sampling via Fourier Transform]] — the Abelian technique used at each step of the chain
- [[Order-Finding via QFT and Continued Fractions]] — the order-finding subroutine
- [[Quantum Fourier Transform Circuit]]
