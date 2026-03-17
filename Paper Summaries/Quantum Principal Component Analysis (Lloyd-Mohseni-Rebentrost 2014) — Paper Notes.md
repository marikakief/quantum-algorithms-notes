> **Source:** Seth Lloyd, Masoud Mohseni, and Patrick Rebentrost, *Quantum principal component analysis*, Nature Physics **10**, 631–633 (2014); arXiv:1307.0401
> **Links:** [arXiv](https://arxiv.org/abs/1307.0401) · [Nature Physics](https://doi.org/10.1038/nphys3029)
> **Tags:** #quantum-ML #density-matrix-exponentiation #PCA #tomography #dequantization

---

## What the paper does

Two things:

1. **Density matrix exponentiation:** Given $n$ copies of an unknown quantum state $\rho$, implement the unitary $e^{-i\rho t}$ on any other state $\sigma$. This uses $n = O(t^2/\epsilon)$ copies and takes time $O(n \log d)$ for a $d$-dimensional system.

2. **Quantum PCA (qPCA):** Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] with $e^{-i\rho t}$ to decompose any state into the eigenbasis of $\rho$, revealing the principal components (eigenvectors with large eigenvalues) in time $O(R \log d)$ where $R$ is the effective rank.

The exponential speedup over classical PCA ($O(d)$ at minimum) holds when the matrix is low-rank and you have quantum access to the data. This paper launched the quantum machine learning subfield alongside [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]].

---

## Density matrix exponentiation

### The trick

The partial swap operation on $\rho \otimes \sigma$:

$$
\mathrm{tr}_1\left[e^{-iS\Delta t}(\rho \otimes \sigma)e^{iS\Delta t}\right] = \sigma - i\Delta t[\rho, \sigma] + O(\Delta t^2)
$$

where $S$ is the swap operator on the two registers. This is exactly $e^{-i\rho \Delta t}\sigma\, e^{i\rho \Delta t}$ to first order.

**Repeating with $n$ copies of $\rho$:** Each copy contributes one infinitesimal step $\Delta t = t/n$. After $n$ steps, we've implemented $e^{-i\rho t}$ on $\sigma$. By Suzuki-Trotter analysis:

$$
n = O(t^2/\epsilon) \text{ copies of } \rho \text{ suffice for accuracy } \epsilon
$$

The swap operator $S$ is sparse (it's a permutation), so $e^{-iS\Delta t}$ can be implemented efficiently in $O(\log d)$ gates.

### What this means

**Any density matrix is a Hamiltonian.** Given copies of an unknown state, you can use it to drive unitary evolution on other states — without ever learning what $\rho$ is. The state "plays an active role in its own analysis."

This works for **non-sparse** matrices, unlike standard Hamiltonian simulation which requires sparsity or special structure. The only requirement is that you have multiple copies of the state.

---

## Quantum PCA

### The algorithm

1. Prepare $n$ copies of $\rho$ and one copy of an input state $|\psi\rangle$
2. Use density matrix exponentiation to implement controlled-$e^{-i\rho t}$ for varying times $t$
3. Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] to decompose $|\psi\rangle$ into the eigenbasis of $\rho$:

$$
|\psi\rangle|0\rangle \to \sum_i \psi_i |\chi_i\rangle|\tilde{r}_i\rangle
$$

where $|\chi_i\rangle$ are eigenvectors of $\rho$ and $\tilde{r}_i \approx r_i$ are the eigenvalues.

4. If the input is $\rho$ itself, the output is $\sum_i r_i |\chi_i\rangle\langle\chi_i| \otimes |\tilde{r}_i\rangle\langle\tilde{r}_i|$ — sampling from this reveals the principal components with probability proportional to their eigenvalues.

### Complexity

- Phase estimation to accuracy $\epsilon$: time $t = O(1/\epsilon)$
- Copies required: $n = O(1/\epsilon^3)$ (from the $t^2/\epsilon$ scaling of density matrix exponentiation)
- Total time: $O(R\log d / \epsilon^3)$ for rank-$R$ density matrices

### When it's powerful

When $\rho$ is low-rank ($R \ll d$): the principal components dominate, and qPCA reveals them in time logarithmic in $d$. For rank-$d$ maximally mixed state: $t = O(d)$ needed to resolve anything — no advantage.

---

## Applications

1. **Quantum self-tomography:** Use $\rho$ to discover its own eigenvectors and eigenvalues. Not full tomography ($d^2$ parameters), but reveals the principal components.

2. **Exponentiation of non-sparse matrices:** If $X = A^\dagger A$ is positive and you have quantum access to columns of $A$, construct $\rho = X/\mathrm{tr}(X)$ as a quantum state and exponentiate via the trick.

3. **State discrimination / cluster assignment:** Given training states from two classes with density matrices $\rho$ and $\sigma$, decompose a new state $|\chi\rangle$ in the eigenbasis of $\rho - \sigma$ and assign to the class with positive/negative eigenvalue. Confidence = eigenvalue magnitude.

4. **Quantum process tomography:** Apply qPCA to the Choi-Jamiołkowski state of a quantum channel.

---

## Dequantization (Tang 2018)

Ewin Tang (then an 18-year-old undergraduate) showed that **classical algorithms can match the quantum PCA speedup** under comparable data access assumptions.

**The key insight:** The quantum advantage of qPCA (and [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]]) relies on quantum access to the data — either qRAM or the ability to prepare quantum states encoding the data. Tang showed that if you grant the classical algorithm analogous "sample and query" access to the data (the ability to sample row indices proportional to their norms and to query individual entries), then classical algorithms can also achieve polylogarithmic-in-$d$ runtime for low-rank matrices.

**Tang's dequantized PCA** (2018, building on the 2018 recommendation systems result):
- Classical algorithm for PCA in time $\tilde{O}(\text{poly}(R, 1/\epsilon, \ldots)/\text{poly}(\sigma_{\min}))$
- Polynomial in rank $R$ and precision, polylogarithmic in dimension $d$
- Same scaling regime as qPCA, but classical

**What this means for qPCA:**
- The exponential speedup is real *if* the input is genuinely quantum (copies of an unknown quantum state)
- The speedup is **not real** for classical data loaded via qRAM — the qRAM assumption is doing the heavy lifting, and a classical analogue of qRAM gives the same scaling
- For quantum tomography and quantum state analysis (the original motivation), the advantage remains: you genuinely have copies of $\rho$, not classical data

This was one of the most important results in quantum algorithms: it clarified that many quantum ML speedups were **artifacts of the data access model**, not intrinsic quantum advantages. The genuine quantum advantages are for quantum data (states, channels) or settings where the sampling model doesn't apply.

---

## Reusable ideas

1. **[[Density Matrix Exponentiation via Partial Swap]]:** Given $n$ copies of $\rho$, implement $e^{-i\rho t}$ via repeated infinitesimal swap operations. Cost: $n = O(t^2/\epsilon)$ copies. Works for non-sparse, non-structured matrices — any density matrix. Converts quantum states into Hamiltonians.

2. **[[Quantum Self-Tomography via Phase Estimation]]:** Apply [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]] with density matrix exponentiation to decompose states into the eigenbasis of an unknown $\rho$. Time $O(R \log d)$ for rank-$R$ matrices — exponential over classical when the rank is low and the data is quantum.

---

## References within this paper

- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|Harrow, Hassidim & Lloyd (2009)]] — quantum matrix inversion (HHL); qPCA extends the $e^{-i\rho t}$ component
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]] — [[Gapped Phase Estimation|phase estimation]] used to extract eigenvalues from $e^{-i\rho t}$
- Berry, Ahokas, Cleve & Sanders (2007) — higher-order Suzuki-Trotter for sparse Hamiltonian simulation
- Gross, Liu, Flammia, Becker & Eisert (2010) — quantum compressive sensing ($O(Rd\log d)$ tomography)

---

## Cross-links

### Paper notes
- [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]] — HHL, which qPCA extends
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]] — phase estimation subroutine
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — modern framework that can also implement matrix functions, subsumes some of qPCA's techniques

### Trick cards
- [[Density Matrix Exponentiation via Partial Swap]]
- [[Quantum Self-Tomography via Phase Estimation]]
- [[Gapped Phase Estimation]]
- [[Hamiltonian-to-Projection via Phase Estimation]]
