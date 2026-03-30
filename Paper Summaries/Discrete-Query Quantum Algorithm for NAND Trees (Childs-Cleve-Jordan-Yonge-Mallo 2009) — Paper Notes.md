> **Source:** Andrew M. Childs, Richard Cleve, Stephen P. Jordan, David Yonge-Mallo, *Discrete-query quantum algorithm for NAND trees*, Theory of Computing 5:119–123, 2009
> **Links:** [arXiv:quant-ph/0702160](https://arxiv.org/abs/quant-ph/0702160) · [ToC](https://doi.org/10.4086/toc.2009.v005a005)
> **Tags:** #quantum-walks #query-complexity #NAND-trees #formula-evaluation #Hamiltonian-simulation

---

## The computational problem

Evaluate a balanced binary NAND tree of depth $n$ with $N = 2^n$ input leaves. Given black-box (query) access to the input bits $x_1, \ldots, x_N$, determine the value of the root $f_n(x_1, \ldots, x_N)$, where:

$$f_n(x_1, \ldots, x_{2^n}) = \neg\bigl(f_{n-1}(x_1, \ldots, x_{2^{n-1}}) \wedge f_{n-1}(x_{2^{n-1}+1}, \ldots, x_{2^n})\bigr)$$

with $f_0(x) = x$.

---

## What the paper does

Shows that the Farhi-Goldstone-Gutmann (FGG) continuous-time quantum walk algorithm for NAND trees can be converted into a standard discrete-query algorithm using $O(N^{1/2+\varepsilon})$ queries for any fixed $\varepsilon > 0$. This bridges the gap between the Hamiltonian oracle model (where FGG achieve $O(\sqrt{N} \log N)$ time) and the conventional quantum query model.

The result is short and clean — essentially a two-page observation that high-order product formula simulation of the FGG Hamiltonian gives the required query complexity. Combined with the $\Omega(\sqrt{N})$ lower bound of Barnum-Saks, this pins the quantum query complexity of NAND trees to $\Theta(N^{1/2+o(1)})$.

---

## The algorithm / construction

### FGG's Hamiltonian oracle model

In the Hamiltonian oracle model, the input is accessed via a Hamiltonian $H_O$ acting on $n+1$ qubits:

$$H_O |b, k\rangle = -x_k |\neg b, k\rangle$$

The algorithm evolves under $H_O + H_D$ for time $T = O(\sqrt{N} \log N)$, where $H_D$ is a time-independent driving Hamiltonian designed so that the walk on the formula tree distinguishes the two root values.

### Converting to discrete queries

The paper observes that two standard queries (Eq. 3: $U_O|k, a\rangle = |k, a \oplus x_k\rangle$) suffice to simulate evolution by $H_O$ for arbitrary time $t$:

1. Apply $U_O$ to the index and ancilla registers
2. Apply a controlled-$R(t)$ gate: $R(t) = \begin{pmatrix} \cos t & i\sin t \\ i\sin t & \cos t \end{pmatrix}$
3. Apply $U_O$ again to uncompute

This implements $e^{-iH_O t}$ exactly using 2 queries, regardless of $t$.

### Product formula simulation

Since $H_D$ is time-independent, the total evolution $e^{-i(H_O + H_D)T}$ can be simulated using a $p$th-order Suzuki-Trotter decomposition. By [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]], a $p$th-order approximation requires $O((hT)^{1+1/2p})$ steps, where $h = \max(\|H_O\|, \|H_D\|) \leq 3$.

Choosing $p$ arbitrarily large makes the exponent $1 + 1/2p$ arbitrarily close to 1. Since $T = O(\sqrt{N} \log N)$ and each step uses 2 queries to $U_O$, the total query count is:

$$O(T^{1+\delta}) = O(N^{1/2+\varepsilon})$$

for any fixed $\varepsilon > 0$.

---

## Key results

**Main result:** The quantum query complexity of evaluating a balanced binary NAND tree on $N$ leaves is $O(N^{1/2+\varepsilon})$ for any $\varepsilon > 0$.

Combined with the $\Omega(\sqrt{N})$ lower bound from Barnum-Saks, the quantum query complexity is $\Theta(N^{1/2+o(1)})$.

---

## Comparison with prior work

| Algorithm | Query complexity | Model |
|---|---|---|
| Best classical randomised (Saks-Wigderson) | $\Theta(N^{0.753\ldots})$ | Classical queries |
| Farhi-Goldstone-Gutmann (2007) | $O(\sqrt{N} \log N)$ | Hamiltonian oracle |
| **Childs-Cleve-Jordan-Yonge-Mallo (2009)** | **$O(N^{1/2+\varepsilon})$** | **Standard queries** |
| [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes\|Ambainis-Childs-Reichardt-Špalek-Zhang (2007)]] | $O(N^{1/2+o(1)})$ | Standard queries, *any* formula |
| Barnum-Saks (2004) | $\Omega(\sqrt{N})$ lower bound | Standard queries |
| [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes\|Berry-Cleve-Gharibian (2012)]] | $N^{1/2+o(1)}$ gates | Gate-efficient conversion |

The ACRŠ Z paper (appearing around the same time) achieves $O(N^{1/2+o(1)})$ for *arbitrary* AND-OR formulas (not just balanced NAND), via a different technique (coined quantum walk + phase estimation). This paper's contribution is showing that the FGG continuous-time approach also works in the standard model, via a simpler argument.

---

## Limits / caveats

- The $\varepsilon$ in $O(N^{1/2+\varepsilon})$ comes from the product formula overhead. It's not $O(\sqrt{N})$ exactly — there's a subpolynomial overhead. The ACRŠ Z paper also has a similar $o(1)$ overhead for general formulas.

- This paper only handles balanced binary NAND trees. The ACRŠ Z generalisation to arbitrary read-once formulas requires different techniques.

- The Hamiltonian-to-discrete conversion is generic: it works whenever the Hamiltonian oracle uses a time-independent driving term. It does *not* apply to time-dependent driving Hamiltonians without additional work.

---

## Reusable ideas

1. [[Hamiltonian Oracle to Discrete Query Conversion via Product Formulas]] — Convert a continuous-time Hamiltonian oracle algorithm into a discrete-query algorithm by simulating $e^{-i(H_O + H_D)t}$ with product formulas, where each $e^{-iH_O \Delta t}$ step costs a constant number of standard queries.

---

## References within this paper

- Farhi-Goldstone-Gutmann (2007, quant-ph/0702144) — the continuous-time NAND tree algorithm that this paper discretises
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders (2005)]] — high-order product formula simulation of sparse Hamiltonians
- Childs (2004, PhD thesis) — sparse Hamiltonian simulation results
- Barnum-Saks (2004) — $\Omega(\sqrt{N})$ lower bound for quantum evaluation of read-once functions
- Saks-Wigderson (1986) — $\Theta(N^{0.753\ldots})$ classical randomised complexity of NAND trees

---

## Cross-links

### Paper notes
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]] — the full generalisation to arbitrary formulas
- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]] — makes the conversion gate-efficient (polylog ancillae)
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]] — earlier exponential speedup via continuous-time quantum walk (glued trees)
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]] — the simulation result used here
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]] — general continuous/discrete quantum walk equivalence

### Trick cards
- [[Hamiltonian Oracle to Discrete Query Conversion via Product Formulas]]
