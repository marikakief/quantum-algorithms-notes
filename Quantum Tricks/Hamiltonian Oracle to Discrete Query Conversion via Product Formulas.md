# Hamiltonian Oracle to Discrete Query Conversion via Product Formulas

> **Source:** Childs, Cleve, Jordan, Yonge-Mallo, arXiv:quant-ph/0702160
> **Tags:** #trick #Hamiltonian-simulation #query-complexity #product-formulas #quantum-walks

## What it does
Converts the Farhi-Goldstone-Gutmann / Childs-Cleve-Jordan-Yonge-Mallo style Hamiltonian-oracle algorithm, where the oracle Hamiltonian is self-inverse and the driving Hamiltonian is time-independent, into a standard discrete-query algorithm with subpolynomial query overhead.

## The trick
Suppose a quantum algorithm evolves under $H_O + H_D$ for total time $T$, where $H_O$ is the specific standard-query Hamiltonian oracle and $H_D$ is a time-independent driving Hamiltonian. Two steps:

**Step 1: Simulate $e^{-iH_O t}$ with discrete queries.** For this self-inverse oracle Hamiltonian, two standard queries $U_O$ (plus a controlled rotation) implement $e^{-iH_O t}$ *exactly* for any $t$:
- Apply $U_O$
- Apply controlled-$R(t) = \begin{pmatrix} \cos t & i\sin t \\ i\sin t & \cos t \end{pmatrix}$
- Apply $U_O$ to uncompute

Cost: 2 queries per simulation of $e^{-iH_O t}$, regardless of $t$.

**Step 2: Approximate $e^{-i(H_O + H_D)T}$ with product formulas.** Use a $p$th-order Suzuki-Trotter decomposition. By standard results ([[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders 2005]]), this requires $O((hT)^{1+1/2p})$ Trotter steps, where $h = \max(\|H_O\|, \|H_D\|)$.

For any fixed $\delta > 0$, choose the Suzuki order $p$ large enough in advance to get total queries $O(T^{1+\delta})$.

## When to reach for it
- You have the FGG/CCJY self-inverse Hamiltonian-query setup and need to express it in the standard query model
- The driving Hamiltonian $H_D$ is time-independent and efficiently simulable apart from query cost
- You want to prove query complexity upper bounds for problems originally solved in the Hamiltonian model

## Complexity
$O(T^{1+1/2p})$ queries for a $p$th-order product formula, where $T$ is the continuous-time algorithm's runtime. For $T = O(\sqrt{N})$, this gives $O(N^{1/2+\varepsilon})$ queries.

## Caveat
The conversion adds a subpolynomial overhead that prevents this product-formula argument from achieving exactly $O(\sqrt{N})$ from a $O(\sqrt{N})$-time Hamiltonian algorithm -- you pick up an $N^{\varepsilon}$-type factor for fixed product-formula order choices. It does not cover arbitrary continuous-time query algorithms, non-self-inverse oracle Hamiltonians, or general time-dependent driving Hamiltonians without additional machinery.

The approach also doesn't address gate complexity (total number of elementary gates). For that, see [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes|Berry-Cleve-Gharibian (2012)]], which compresses ancilla use to polylog.

## Related notes
- [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes]]
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]
- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
