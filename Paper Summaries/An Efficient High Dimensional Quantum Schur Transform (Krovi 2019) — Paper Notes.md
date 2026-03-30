> **Source:** Hari Krovi, *An efficient high dimensional quantum Schur transform*, Quantum **3**:122, 2019, arXiv:1804.00055
> **Links:** [arXiv](https://arxiv.org/abs/1804.00055) · [Quantum](https://doi.org/10.22331/q-2019-02-14-122)
> **Tags:** #quantum-algorithm #representation-theory #Schur-transform #symmetric-group #unitary-group #Fourier-transform #spectrum-estimation #tomography

---

## The computational problem

The **Schur transform** is the unitary that block-diagonalises the joint action of the symmetric group $S_n$ and the unitary group $U(d)$ on the $n$-fold tensor product $V^{\otimes n}$ of a $d$-dimensional space $V$. By Schur-Weyl duality:

$$V^{\otimes n} \cong \bigoplus_{\lambda} W_\lambda \otimes V_\lambda,$$

where $\lambda$ ranges over Young diagrams (partitions of $n$ with at most $d$ parts), $V_\lambda$ is the irrep of $S_n$, and $W_\lambda$ is the irrep of $U(d)$.

The Schur transform maps the computational basis $|e_1, \ldots, e_n\rangle$ with $e_i \in [d]$ to the block-diagonal basis $|\lambda, i, j\rangle$, where $\lambda$ is the irrep label, $i$ indexes the symmetric group irrep (in the Young–Yamanouchi basis, labelled by standard Young tableaux, SYTs), and $j$ indexes the unitary group irrep (in the Gelfand-Tsetlin basis, labelled by semi-standard Young tableaux, SSYTs).

**Input:** $n$ registers, each of dimension $d$.
**Output:** Registers $|\lambda, i, j\rangle$ in the block-diagonal basis.
**Cost measure:** Number of elementary gates, as a function of $n$, $d$, and precision $\varepsilon$.

---

## What the paper does

Gives the first fully-detailed quantum algorithm for the Schur transform that runs in $O(\mathrm{poly}(n, \log d, \log(1/\varepsilon)))$ time — polynomial in $\log d$ rather than $d$. This is an exponential improvement in dimension over Bacon, Chuang, and Harrow (BCH, 2006–2007), whose algorithm is $O(\mathrm{poly}(n, d, \log(1/\varepsilon)))$.

The key idea: instead of iteratively applying Clebsch-Gordan decompositions for $U(d)$ (the BCH approach), work with the **symmetric group** side. Block-diagonalise the **permutation modules** of $S_n$ — which are induced representations from Young subgroups — using a QFT over the symmetric group plus generalised phase estimation. This "dual" approach avoids any $U(d)$-specific subroutines whose cost scales with $d$.

**My assessment:** This is an elegant and practically relevant improvement. The Schur transform is a key primitive in quantum information — used in spectrum estimation, tomography, de Finetti reductions, universal compression, and decoherence-free subspace encoding. The BCH algorithm's $\mathrm{poly}(d)$ scaling was acceptable for qubits ($d = 2$) but ruled out applications where $d$ grows with the problem size. Krovi's algorithm makes the Schur transform genuinely scalable. The approach also introduces a QFT over permutation modules that could find independent use (the paper suggests connections to element distinctness and collision problems).

Harrow's thesis (2005) sketched a way to get $\log d$ scaling by modifying the BCH approach, but the details were never written up. Krovi's construction is a clean, self-contained alternative using different representation-theoretic machinery.

---

## The algorithm / construction

### Overview

The algorithm has four main steps, combining classical combinatorics with quantum Fourier transforms.

### Step 1: Map large alphabet to $[n]$

Given an $n$-tuple $|e_1, \ldots, e_n\rangle$ with $e_i \in [d]$, map any entries greater than $n$ to entries in $[n]$ while recording the mapping $p_e$. This works because at most $n$ distinct labels can appear in an $n$-tuple. Cost: $O(\mathrm{poly}(n, \log d))$.

**This is the step that buys the $\log d$ improvement.** After this, all subsequent processing works within $S_n$, which has size $n!$ — independent of $d$.

### Step 2: Extract type and transversal

The **type** $T = (t_1, \ldots, t_n)$ counts how many times each label appears. The **transversal** element $t \in S_n / Y_T$ records which permutation maps the canonical sorted tuple to the actual input. The Young subgroup $Y_T = S_{t_1} \times S_{t_2} \times \cdots$ is the stabiliser of the sorted tuple.

Cost: $O(\mathrm{poly}(n))$.

### Step 3: QFT over permutation modules (QFTPermMod)

This is the technical core. A permutation module $P(T)$ is the induced representation $\uparrow^{S_n}_{Y_T} \mathbf{1}$ of the trivial representation of $Y_T$. The algorithm block-diagonalises it into irreps of $S_n$ using:

1. **Ancilla preparation:** Append $\lceil \log |Y_T| \rceil$ qubits, create equal superposition over $Y_T$.
2. **Generalised phase estimation (GPE):** Perform GPE on the right regular representation $R$ of $S_n$, controlled by $Y_T$. This rotates the multiplicity-space basis from SYTs to projected vectors $\lambda(Y_T)|T_1\rangle$, which are non-zero exactly when the corresponding SYT defines a valid SSYT (i.e., the horizontal strip condition holds).
3. **QFT over $S_n$:** Apply Beals' algorithm for the QFT over the symmetric group.

**The key lemma (Lemma 2):** If any two elements of a consecutive set $A$ appear in the same column of an SYT $T$, then $\lambda(S_A)|T\rangle = 0$. This is proved by induction using the Young orthonormal representation formulas. The proof uses the factorisation $S_A = S'_A \cdot ((i, i+1) + e)$ and the fact that adjacent transpositions act as $-1$ on SYTs with the two elements in the same column.

This lemma ensures that the GPE correctly projects onto the subspace of vectors corresponding to SSYTs — which are exactly the Gelfand-Tsetlin basis vectors for $U(d)$.

Cost of QFTPermMod: $O(\mathrm{poly}(n, \log(1/\varepsilon)))$.

### Step 4: Convert to Gelfand-Tsetlin basis via RSK

The output of Step 3 labels the unitary group irrep by $(T, j)$ — a type plus an SSYT with entries in $[n]$. To get the correct SSYT with entries in $[d]$ (i.e., the Gelfand-Tsetlin basis vector), use the mapping $p_e$ from Step 1 and the RSK (Robinson-Schensted-Knuth) algorithm to permute the content of the SSYT.

The RSK row-insertion procedure is classical and runs in polynomial time. The key property used: for a transposition $(k, k+1)$ swapping the counts of labels $k$ and $k+1$, the SSYT $U = A \cdot B \cdot C$ factors into parts containing only labels $\leq k$, labels $k$ and $k+1$, and labels $> k+1$. The swap produces $A \cdot B' \cdot C$ where $B'$ has the two labels exchanged in the overhang.

### Correctness proof

Theorem 3 (main theorem) proves correctness by verifying the output basis is the Gelfand-Tsetlin basis. The argument shows that the Lie algebra generators $J^{(l)}_+, J^{(l)}_-, J^{(l)}_0$ of $\mathrm{SU}(d)$ act correctly on the produced basis — specifically, $J^{(l)}_+$ moves between SSYTs by swapping labels $l$ and $l+1$ while preserving horizontal strips labelled $d$. This confirms block-diagonalisation along the tower $U(1) \subset U(2) \subset \cdots \subset U(d)$.

---

## Key results

**Theorem 3 (Main):** The DualSchur algorithm performs the strong Schur transform — the change of basis from $|e_1, \ldots, e_n\rangle$ to $|\lambda, i, j\rangle$ — in time

$$O(\mathrm{poly}(n, \log d, \log(1/\varepsilon))).$$

**Theorem 1:** The QFTPermMod algorithm block-diagonalises any permutation module of $S_n$ in time $O(\mathrm{poly}(n, \log(1/\varepsilon)))$.

---

## Comparison with prior work

| | BCH (2006–2007) | Harrow thesis sketch (2005) | **Krovi (2019)** |
|---|---|---|---|
| **Approach** | Clebsch-Gordan for $U(d)$, iterated | Modified BCH with $U(d)$ tower | Permutation modules of $S_n$ |
| **Complexity** | $O(\mathrm{poly}(n, d, \log(1/\varepsilon)))$ | $O(\mathrm{poly}(n, \log d, \log(1/\varepsilon)))$ (claimed) | $O(\mathrm{poly}(n, \log d, \log(1/\varepsilon)))$ |
| **Detailed proof** | Yes | No (footnote-level sketch) | Yes |
| **Key subroutines** | CG decomposition for $U(d)$ | CG + GT labelling trick | QFT over $S_n$, GPE, RSK |
| **Novel primitives** | CG circuit | — | QFT over permutation modules |

---

## Limits / caveats

- The polynomial in $n$ is not explicitly optimised. The paper focuses on establishing poly$(n, \log d, \log(1/\varepsilon))$ scaling, not minimising the degree.
- **Weak vs. strong Schur sampling:** Many applications (spectrum estimation, tomography) only need *weak* Schur sampling — measuring the irrep label $\lambda$ alone. Weak Schur sampling already had $\log d$ scaling via GPE (described in Harrow's thesis). Krovi's contribution is to the *strong* Schur transform, which also resolves the symmetric and unitary group registers.
- The RSK step (Step 4) is classical and polynomial-time, but the constant factors are not analysed.
- Phase conventions in the Gelfand-Tsetlin basis are fixed up to a sign per multiplicity space. This doesn't affect projective measurements (the standard use case), but matters if subsequent coherent operations depend on the phase choice.

---

## Reusable ideas

1. [[QFT over Permutation Modules]] — Block-diagonalising induced representations $\uparrow^{S_n}_{Y_T} \mathbf{1}$ via QFT + GPE. Could apply to any problem with permutational symmetry.
2. [[Alphabet Reduction to n for Schur Transform]] — Mapping a $d$-dimensional alphabet to $[n]$ by recording which of the at most $n$ distinct labels appear. Removes $d$-dependence from all subsequent steps.
3. [[GPE for Multiplicity Space Decomposition]] — Using generalised phase estimation on the right regular representation to decompose multiplicity spaces of induced representations into their irreducible components.

---

## References within this paper

- **Bacon, Chuang, Harrow (2006, 2007)** — Original Schur transform for qubits [4] and qudits [5]. The baseline this paper improves upon. Not in the vault.
- **Harrow (2005)** — PhD thesis [18] sketching the $\log d$ extension of BCH. Also introduces GPE for weak Schur sampling. Not in the vault.
- **Beals (1997)** — QFT over the symmetric group [6]. Used as a subroutine. Not in the vault.
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — Phase estimation, the foundation of GPE.
- **O'Donnell and Wright (2015, 2016)** — Spectrum testing [34] and efficient quantum tomography [35] using Schur sampling. Not in the vault.
- **Koenig and Mitchison (2009)** — Quantum de Finetti theorem [30] via Schur-Weyl duality.
- **Gelfand and Tsetlin (1950)** — Original GT basis construction [13].
- **Fulton (1997)** — RSK algorithm and Young tableaux combinatorics [11].
- **Krovi and Russell (2015)** — QFT over quantum doubles of finite groups [31], using similar induced-representation Fourier transforms.

---

## Cross-links

### Paper notes
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation, which underpins GPE
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — group-theoretic quantum algorithms, related representation-theoretic techniques
- [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes]] — another Krovi paper (different topic)

### Trick cards
- [[QFT over Permutation Modules]] — the main new primitive from this paper
- [[Alphabet Reduction to n for Schur Transform]] — the $d \to n$ mapping trick
- [[GPE for Multiplicity Space Decomposition]] — generalised phase estimation for induced reps
