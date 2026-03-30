# Hamiltonian Oracle to Discrete Query Conversion via Product Formulas

> **Source:** Childs, Cleve, Jordan, Yonge-Mallo, arXiv:quant-ph/0702160
> **Tags:** #trick #Hamiltonian-simulation #query-complexity #product-formulas #quantum-walks

## What it does
Converts any quantum algorithm designed for the Hamiltonian oracle model (continuous-time access to $H_O$) into a standard discrete-query algorithm, with only subpolynomial query overhead.

## The trick
Suppose a quantum algorithm evolves under $H_O + H_D$ for total time $T$, where $H_O$ is the oracle Hamiltonian and $H_D$ is a time-independent driving Hamiltonian. Two steps:

**Step 1: Simulate $e^{-iH_O t}$ with discrete queries.** Two standard queries $U_O$ (plus a controlled rotation) implement $e^{-iH_O t}$ *exactly* for any $t$:
- Apply $U_O$
- Apply controlled-$R(t) = \begin{pmatrix} \cos t & i\sin t \\ i\sin t & \cos t \end{pmatrix}$
- Apply $U_O$ to uncompute

Cost: 2 queries per simulation of $e^{-iH_O t}$, regardless of $t$.

**Step 2: Approximate $e^{-i(H_O + H_D)T}$ with product formulas.** Use a $p$th-order Suzuki-Trotter decomposition. By standard results ([[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry-Ahokas-Cleve-Sanders 2005]]), this requires $O((hT)^{1+1/2p})$ Trotter steps, where $h = \max(\|H_O\|, \|H_D\|)$.

Choosing $p$ large enough: total queries $= O(T^{1+\delta})$ for any $\delta > 0$.

## When to reach for it
- You have a quantum algorithm designed in the Hamiltonian oracle model (e.g., quantum walk algorithms with continuous-time evolution) and need to express it in the standard query model
- The driving Hamiltonian $H_D$ is time-independent (or piecewise constant)
- You want to prove query complexity upper bounds for problems originally solved in the Hamiltonian model

## Complexity
$O(T^{1+1/2p})$ queries for a $p$th-order product formula, where $T$ is the continuous-time algorithm's runtime. For $T = O(\sqrt{N})$, this gives $O(N^{1/2+\varepsilon})$ queries.

## Caveat
The conversion adds a subpolynomial overhead that prevents achieving exactly $O(\sqrt{N})$ from a $O(\sqrt{N})$-time Hamiltonian algorithm — you always pick up an $N^{\varepsilon}$ factor (or $\log$ factors if using higher-order methods). For time-dependent driving Hamiltonians $H_D(t)$, the approach needs more sophisticated simulation techniques and the overhead may be worse.

The approach also doesn't address gate complexity (total number of elementary gates). For that, see [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes|Berry-Cleve-Gharibian (2012)]], which compresses ancilla use to polylog.

## Related notes
- [[Discrete-Query Quantum Algorithm for NAND Trees (Childs-Cleve-Jordan-Yonge-Mallo 2009) — Paper Notes]]
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]
- [[Gate-Efficient Discrete Simulations of Continuous-Time Quantum Query Algorithms (Berry-Cleve-Gharibian 2012) — Paper Notes]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
