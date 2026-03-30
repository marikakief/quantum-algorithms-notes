> **Source:** Rahul Jain, Zhengfeng Ji, Sarvagya Upadhyay, John Watrous, "QIP = PSPACE", *Journal of the ACM* **58**(6):30, 2011; originally STOC 2010; arXiv:0907.4737
> **Links:** [arXiv](https://arxiv.org/abs/0907.4737) · [Journal](https://doi.org/10.1145/2049697.2049704)
> **Tags:** #complexity #QIP #PSPACE #interactive-proofs #SDP #multiplicative-weights #foundational

---

## The computational problem

**Quantum interactive proofs vs space-bounded computation:** Is $\mathrm{QIP}$ — the class of problems with polynomial-message quantum interactive proof systems — equal to $\mathrm{PSPACE}$? The containment $\mathrm{PSPACE} \subseteq \mathrm{QIP}$ was immediate from $\mathrm{IP} = \mathrm{PSPACE}$ (Shamir 1992) plus the fact that quantum verifiers can simulate classical ones. The best upper bound prior to this paper was $\mathrm{QIP} \subseteq \mathrm{EXP}$, proved by Kitaev and Watrous (2000) via exponential-size semidefinite programming.

## What the paper does

Proves $\mathrm{QIP} = \mathrm{PSPACE}$, completing the complexity-theoretic classification of quantum interactive proofs. The missing direction $\mathrm{QIP} \subseteq \mathrm{PSPACE}$ is established by giving a PSPACE algorithm that solves a class of semidefinite programs (SDPs) capturing the power of quantum interactive proofs.

The technique: apply the **[[Matrix Multiplicative Weights Update Method for SDPs|matrix multiplicative weights update (MMW) method]]** to these SDPs in a parallelised form that can be implemented in NC(poly) = PSPACE. The result is a clean, 18-page proof — remarkably short for the importance of the theorem.

This is one of those results that feels inevitable in retrospect but required exactly the right tool. The MMW method was the key — it's the right SDP solver for this problem because it can be parallelised (unlike interior-point methods, which are inherently sequential). My assessment: the paper's lasting impact is as much about advertising MMW as a tool for complexity theory as about the specific $\mathrm{QIP} = \mathrm{PSPACE}$ result.

---

## The algorithm

### Step 1: Reduce QIP to a single-coin QMAM game

The paper relies on [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes|Marriott-Watrous (2005)]], which proved $\mathrm{QIP} = \mathrm{QMAM}$. This means every QIP protocol can be reformulated as a 3-message Arthur-Merlin game where Arthur sends only a single random bit:

1. Merlin sends a quantum register $W$ to Arthur.
2. Arthur flips a fair coin $a \in \{0, 1\}$ and announces it.
3. Merlin sends a second register $Y$.
4. Arthur performs one of two measurements $\{P_a, I - P_a\}$ depending on $a$.

With perfect completeness (Arthur accepts with probability 1 for YES instances) and soundness $\leq 1/2 + \varepsilon$ for any constant $\varepsilon > 0$.

### Step 2: Formulate as a semidefinite program

Define the operator

$$Q = \frac{1}{2}|0\rangle\langle 0| \otimes P_0 + \frac{1}{2}|1\rangle\langle 1| \otimes P_1 \in \mathrm{Pos}(\mathcal{X} \otimes \mathcal{W} \otimes \mathcal{Y})$$

where $\mathcal{X} = \mathbb{C}^2$ corresponds to Arthur's coin. Merlin's optimal cheating strategy corresponds to an SDP:

**Primal:**
$$\max \mathrm{Tr}(X) \quad \text{s.t.} \quad \Phi(X) \leq \mathbf{1}_\mathcal{X} \otimes \sigma, \quad X \in \mathrm{Pos}(\mathcal{X} \otimes \mathcal{W} \otimes \mathcal{Y}), \quad \sigma \in D(\mathcal{W})$$

**Dual:**
$$\min \|\mathrm{Tr}_\mathcal{X}(Y)\| \quad \text{s.t.} \quad \Phi^*(Y) \geq \mathbf{1}_{\mathcal{X} \otimes \mathcal{W} \otimes \mathcal{Y}}, \quad Y \in \mathrm{Pos}(\mathcal{X} \otimes \mathcal{W})$$

where $\Phi(X) = \mathrm{Tr}_\mathcal{Y}(Q^{-1/2} X Q^{-1/2})$ and strong duality holds.

The promise is that the optimal value is either $\geq 7/8$ (YES) or $\leq 5/8$ (NO).

A key simplification over the earlier QIP(2) case from Jain-Upadhyay-Watrous (2009): the register $\mathcal{X}$ is only 2-dimensional (because Arthur sends just one bit), which makes the SDP much easier to handle. This is where $\mathrm{QIP} = \mathrm{QMAM}$ really pays off.

### Step 3: Solve the SDP via matrix multiplicative weights

The MMW algorithm (Figure 1 in the paper) iterates $T = O(\log N / \varepsilon^3 \delta)$ times, where $N = \dim(\mathcal{X} \otimes \mathcal{W} \otimes \mathcal{Y})$:

1. **Initialise:** $W_0 = \mathbf{1}$, $\rho_0 = W_0/N$, $Z_0 = \mathbf{1}_\mathcal{W}$, $\xi_0 = Z_0/M$.

2. **Each iteration $t$:**
   - Compute projection $\Pi_t$ onto the positive eigenspace of $\Phi(\rho_t) - \gamma \cdot \mathbf{1}_\mathcal{X} \otimes \xi_t$.
   - Set $\beta_t = \langle \Pi_t, \Phi(\rho_t) \rangle$.
   - If $\beta_t \leq \varepsilon$: **accept** (construct a primal feasible solution).
   - Otherwise: update via matrix exponentials:
   $$W_{t+1} = \exp\left(-\varepsilon\delta \sum_{j=0}^{t} \Phi^*(\Pi_j/\beta_j)\right), \quad \rho_{t+1} = W_{t+1}/\mathrm{Tr}(W_{t+1})$$
   $$Z_{t+1} = \exp\left(\varepsilon\delta \sum_{j=0}^{t} \mathrm{Tr}_\mathcal{X}(\Pi_j/\beta_j)\right), \quad \xi_{t+1} = Z_{t+1}/\mathrm{Tr}(Z_{t+1})$$

3. If all $T$ iterations complete without acceptance: **reject** (construct a dual feasible solution).

The prover and verifier play a matrix game: $\rho_t$ is the "learner" (prover's strategy), $\Pi_t$ is the "adversary" (constraint violations). The matrix exponential updates ensure the learner converges — this is the matrix analogue of the classical multiplicative weights update, where scalar weights $w_i \leftarrow w_i \cdot e^{-\eta \ell_i}$ are replaced by matrix exponentials.

### Step 4: Implement in PSPACE

The key observation: $T = O(\log N)$ iterations, and each iteration requires only NC-computable matrix operations (matrix exponentials, spectral decompositions, traces). So the algorithm is:

1. Compute $Q$ from Arthur's circuit description — this is in NC(poly).
2. Run $O(\log N)$ iterations of the MMW algorithm — each iteration is in NC, and their composition over $O(\log N)$ steps remains in NC.
3. NC(poly) = PSPACE.

The approximation analysis is straightforward but careful: all matrix operations are done to $K = O(\log N)$ bits of precision, and the error propagation through $T$ iterations stays controlled.

---

## Key results

**Theorem (main).** $\mathrm{QIP} = \mathrm{PSPACE}$.

This completes the picture:

$$\mathrm{BQP} \subseteq \mathrm{QMA} \subseteq \mathrm{QIP} = \mathrm{PSPACE}$$

and also:

$$\mathrm{QIP}(k) = \mathrm{QIP}(3) = \mathrm{QIP} = \mathrm{PSPACE} \quad \text{for all } k \geq 3$$

using the earlier result [[PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) — Paper Notes|$\mathrm{QIP} = \mathrm{QIP}(3)$]] from Kitaev-Watrous (2000)/Watrous (1999).

**Key lemmas:**

- **Lemma 1:** For any $P \in \mathrm{Pos}(\mathcal{X} \otimes \mathcal{Z})$ with $\dim(\mathcal{X}) = 2$: $P \leq 2 \cdot \mathbf{1}_\mathcal{X} \otimes \mathrm{Tr}_\mathcal{X}(P)$. This follows from the Pauli twirl identity $\sum_{\sigma \in \{I, X, Y, Z\}} (\sigma \otimes I) P (\sigma \otimes I) = 2 \cdot \mathbf{1} \otimes \mathrm{Tr}_\mathcal{X}(P)$ and is specific to $\dim(\mathcal{X}) = 2$. This is where the single-coin structure of QMAM is used.

- **Lemma 2:** For $0 \leq P \leq \mathbf{1}$ and $\eta > 0$: $\exp(\eta P) \leq \mathbf{1} + \eta e^\eta P$ and $\exp(-\eta P) \leq \mathbf{1} - \eta e^{-\eta} P$. Standard matrix exponential sandwich bounds that drive the convergence analysis.

---

## Comparison with prior work

| Result | Year | What it establishes |
|---|---|---|
| $\mathrm{IP} = \mathrm{PSPACE}$ (Shamir) | 1992 | Classical interactive proofs |
| $\mathrm{PSPACE} \subseteq \mathrm{QIP}$ | immediate | From $\mathrm{IP} = \mathrm{PSPACE}$ |
| $\mathrm{QIP} = \mathrm{QIP}(3)$ (Kitaev-Watrous) | 2000 | Quantum IPs parallelise to 3 messages |
| $\mathrm{QIP} \subseteq \mathrm{EXP}$ (Kitaev-Watrous) | 2000 | Via exponential-size SDP |
| $\mathrm{QIP} = \mathrm{QMAM}$ ([[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes\|Marriott-Watrous]]) | 2005 | Single-coin AM game suffices |
| $\mathrm{QIP}(2) \subseteq \mathrm{PSPACE}$ (Jain-Upadhyay-Watrous) | 2009 | MMW for 2-message case |
| **$\mathrm{QIP} = \mathrm{PSPACE}$** (this paper) | **2009/2011** | **Full classification** |

The jump from $\mathrm{QIP}(2) \subseteq \mathrm{PSPACE}$ to $\mathrm{QIP} \subseteq \mathrm{PSPACE}$ was enabled by using QMAM instead of attacking QIP(3) directly. The 2-dimensional coin register $\mathcal{X}$ makes the SDP structurally simpler than the QIP(2) SDP (where $\mathcal{X}$ has arbitrary dimension), despite the additional density operator $\sigma$.

---

## Limits / caveats

- The result is purely complexity-theoretic — it classifies QIP but doesn't give efficient quantum algorithms for PSPACE-complete problems.
- The PSPACE simulation is exponentially slow compared to the quantum interactive proof itself. The content is "a classical computer with polynomial space can simulate anything a quantum prover-verifier system can do", not "here's a fast way to do it".
- The paper handles only the case of a single prover. Multi-prover quantum interactive proofs ($\mathrm{QMIP}^*$) are a completely different and much harder story — that's the territory of $\mathrm{MIP}^* = \mathrm{RE}$ (Ji-Natarajan-Vidick-Wright-Yuen 2020).

---

## Reusable ideas

1. **[[Matrix Multiplicative Weights Update Method for SDPs]]:** The matrix version of the classical multiplicative weights update, applied to semidefinite programs. Maintains a matrix "state" $\rho_t$ that converges to the optimal SDP solution. Can be parallelised to $O(\log N)$ depth when the oracle (constraint violation) is NC-computable. The key property: each iteration costs one matrix exponential and one spectral decomposition, both in NC.

2. **[[QMAM Reduction to Single-Coin SDP]]:** The structural simplification from $\mathrm{QIP} = \mathrm{QMAM}$: any quantum interactive proof can be reformulated so that the verifier sends only a single random bit, leading to an SDP where the "coin register" $\mathcal{X}$ is 2-dimensional. This dimension-2 structure enables the Pauli twirl bound (Lemma 1) that makes the MMW analysis work.

---

## References within this paper

- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes|Marriott & Watrous (2005)]] — $\mathrm{QIP} = \mathrm{QMAM}$; single-coin Arthur-Merlin characterisation that enables the SDP formulation
- Kitaev & Watrous (2000) — $\mathrm{QIP} = \mathrm{QIP}(3)$ and $\mathrm{QIP} \subseteq \mathrm{EXP}$; the parallelisation and SDP framework this paper improves on
- Jain, Upadhyay & Watrous (2009) — $\mathrm{QIP}(2) \subseteq \mathrm{PSPACE}$; the direct precursor using the same MMW technique
- Arora, Hazan & Kale (2005) / Kale (2007) — the matrix multiplicative weights update framework
- Arora & Kale (2007) / Warmuth & Kuzmin (2006) — MMW applied to semidefinite programming
- Shamir (1992) / Lund-Fortnow-Karloff-Nisan (1992) — $\mathrm{IP} = \mathrm{PSPACE}$ via arithmetisation
- Borodin (1977) — $\mathrm{NC}(\mathrm{poly}) = \mathrm{PSPACE}$

---

## Cross-links

### Paper notes
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]] — provides the QMAM characterisation this paper depends on
- [[PSPACE Has Constant-Round Quantum Interactive Proof Systems (Watrous 2003) — Paper Notes]] — the precursor showing $\mathrm{PSPACE} \subseteq \mathrm{QIP}(2)$
- [[Succinct Quantum Proofs for Properties of Finite Groups (Watrous 2000) — Paper Notes]] — QMA results by the same author
- [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]] — another Watrous paper on quantum complexity

### Trick cards
- [[Matrix Multiplicative Weights Update Method for SDPs]]
- [[QMAM Reduction to Single-Coin SDP]]
- [[Jordan's Lemma Subspace Decomposition for Alternating Measurements]] — used in the Marriott-Watrous result this paper depends on
- [[Trace Trick for QMA-to-PP Reduction]] — another technique from Marriott-Watrous
