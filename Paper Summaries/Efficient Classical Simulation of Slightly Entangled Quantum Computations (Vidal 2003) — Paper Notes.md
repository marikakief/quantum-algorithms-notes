> **Source:** Guifré Vidal, *Efficient classical simulation of slightly entangled quantum computations*, Physical Review Letters 91(14):147902, 2003
> **Links:** [arXiv:quant-ph/0301063](https://arxiv.org/abs/quant-ph/0301063) · [PRL](https://doi.org/10.1103/PhysRevLett.91.147902)
> **Tags:** #classical-simulation #entanglement #Schmidt-rank #tensor-networks #MPS #quantum-advantage

---

## The computational problem

Given an $n$-qubit quantum circuit consisting of poly$(n)$ single-qubit and two-qubit gates starting from $|0\rangle^{\otimes n}$, followed by a local measurement: simulate this computation classically.

The central question: under what conditions on the entanglement can such a computation be efficiently simulated?

---

## What the paper does

Makes the [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes|Jozsa-Linden]] observation quantitative. If the maximum Schmidt rank $\chi$ across *any* bipartition of the $n$ qubits stays bounded by poly$(n)$ throughout the computation, then the entire computation can be classically simulated in poly$(n)$ time and space. This gives a necessary condition for quantum speedup: the entanglement (as measured by $\chi$) must grow at least polynomially with $n$.

The technique — tracking Schmidt decompositions through gates using a local tensor decomposition — is the birth of the matrix product state (MPS) / tensor network simulation approach that now underpins DMRG, TEBD, and a large part of classical simulation of quantum many-body systems.

---

## The algorithm / construction

### Schmidt rank and "slightly entangled"

For a pure state $|\Psi\rangle \in \mathcal{H}_2^{\otimes n}$, partition the qubits into sets $A$ and $B$. The **Schmidt decomposition** gives:

$$|\Psi\rangle = \sum_{\alpha=1}^{\chi_A} \lambda_\alpha |\Phi_\alpha^{[A]}\rangle \otimes |\Phi_\alpha^{[B]}\rangle$$

where $\chi_A$ is the Schmidt rank for the partition $A:B$.

Define the **entanglement measure**:

$$\chi \equiv \max_A \chi_A$$

over all bipartitions. The state is "slightly entangled" if $\chi = \text{poly}(n)$.

### Local tensor decomposition (MPS form)

The state $|\Psi\rangle$ is decomposed into $n$ tensors $\{\Gamma^{[l]}\}_{l=1}^n$ and $n-1$ vectors $\{\lambda^{[l]}\}_{l=1}^{n-1}$:

$$c_{i_1 i_2 \cdots i_n} = \sum_{\alpha_1, \ldots, \alpha_{n-1}} \Gamma^{[1]i_1}_{\alpha_1} \lambda^{[1]}_{\alpha_1} \Gamma^{[2]i_2}_{\alpha_1 \alpha_2} \lambda^{[2]}_{\alpha_2} \cdots \Gamma^{[n]i_n}_{\alpha_{n-1}}$$

This is what we'd now call a **matrix product state** (MPS) in canonical form. The key feature: the $2^n$ amplitudes $c_{i_1 \cdots i_n}$ are parameterised by roughly $(2\chi^2 + \chi)n$ numbers — linear in $n$ for fixed $\chi$.

The decomposition is constructed by iterated Schmidt decompositions, peeling off one qubit at a time from left to right. The vectors $\lambda^{[l]}$ store the Schmidt coefficients for the bipartition $[1\cdots l] : [(l+1)\cdots n]$.

### Gate updates

**Lemma 1 (single-qubit gate):** A unitary $U$ on qubit $l$ only updates $\Gamma^{[l]}$:

$$\Gamma'^{[l]i}_{\alpha\beta} = \sum_{j=0,1} U^i_j \Gamma^{[l]j}_{\alpha\beta}$$

Cost: $O(\chi^2)$ operations.

**Lemma 2 (two-qubit gate on adjacent qubits):** A unitary $V$ on qubits $l, l+1$ updates $\Gamma^{[l]}$, $\lambda^{[l]}$, and $\Gamma^{[l+1]}$. The procedure:

1. Contract $\Gamma^{[l]}$, $\lambda^{[l]}$, $\Gamma^{[l+1]}$ into a $2\chi \times 2\chi$ matrix $\Theta^{ij}_{\alpha\gamma}$
2. Apply $V$ to get the new two-site tensor
3. Diagonalise the reduced density matrix $\rho'^{[DK]}$ to extract new Schmidt vectors and coefficients
4. Read off $\Gamma'^{[l]}$, $\lambda'^{[l]}$, $\Gamma'^{[l+1]}$

Cost: $O(\chi^3)$ operations (dominated by the eigendecomposition).

**Non-adjacent gates:** Handled by applying $O(n)$ SWAP gates to bring the two qubits to adjacent positions, then applying the gate, then swapping back.

**Measurement:** Computing the expectation value of any product operator from the MPS form takes $n \cdot \text{poly}(\chi)$ operations.

---

## Key results

**Theorem 1:** If $\chi_n = \text{poly}(n)$ throughout a pure-state quantum computation, then the computation can be classically simulated with poly$(n)$ memory and computational time.

**Theorem 2:** If $\chi_n$ grows subexponentially in $n$, then the quantum computation can be classically simulated with subexponential resources — so no exponential speedup is possible.

The resource scaling is:

$$\text{Classical description of } n\text{-qubit state} \approx n \cdot \exp(E_\chi) \text{ parameters}$$

where $E_\chi \equiv \log_2 \chi$. An efficient simulation requires $E_\chi = O(\log n)$.

---

## Comparison with prior work

| Result | What it shows |
|---|---|
| Valiant (2001), Terhal-DiVincenzo (2002) | Free fermions (quadratic interactions) are classically simulable |
| [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes\|Gottesman (1997)]] | Clifford circuits on stabiliser states are classically simulable |
| Jozsa-Linden (2002) | Computations with bounded-width entanglement (constant number of qubits per factor) are simulable |
| **Vidal (2003)** | **Quantitative:** computations with $\chi = \text{poly}(n)$ are simulable; entanglement growth is *necessary* for quantum advantage |

The Jozsa-Linden result is a special case: if the state factors into blocks of at most $k$ qubits, then $\chi \leq 2^k$, which is constant. Vidal's result handles the much broader class where entanglement is bounded but need not factor into independent blocks.

---

## Limits / caveats

- The Schmidt rank $\chi$ is defined relative to a fixed linear ordering of qubits. A different ordering could give a very different $\chi$ — so the simulation efficiency depends on finding a good ordering. This is the same issue that plagues DMRG in 2D systems.

- $E_\chi$ (log of the Schmidt rank) is *not* a continuous function of the state. A slight perturbation can change $\chi$ dramatically. Vidal notes this explicitly and suggests using truncated Schmidt decompositions with a fidelity tolerance $\delta$, keeping only the $\chi_\delta$ largest Schmidt values such that $\sum_{\alpha=1}^{\chi_\delta} |\lambda_\alpha|^2 \geq 1 - \delta$. This is exactly what modern TEBD and MPS algorithms do.

- For mixed states, the same framework applies but now $\chi$ measures total correlations (classical + quantum), not just entanglement. The paper explicitly notes this does *not* rule out speedups via very noisy mixed states (cf. Knill-Laflamme one-clean-qubit model).

- The result says nothing about which *problems* have slightly entangled evolutions. The connection to physics came separately: Vidal-Latorre-Rico-Kitaev (2002) showed that non-critical 1D spin chains at zero temperature typically have $\chi = \text{poly}(n)$, making them amenable to this simulation.

- Non-adjacent two-qubit gates require $O(n)$ SWAP operations, which adds an $O(n)$ overhead per gate and means the total simulation cost for a depth-$D$ circuit is $O(nD \cdot \text{poly}(\chi))$.

---

## Reusable ideas

1. [[MPS Canonical Form for Efficient State Tracking]] — Represent an $n$-qubit state as a chain of local tensors with Schmidt coefficients on the bonds; total parameter count scales as $n \cdot \text{poly}(\chi)$ instead of $2^n$.

2. [[Local Gate Update on MPS via Schmidt Redecomposition]] — When a two-qubit gate acts on adjacent sites, contract the two-site tensor, apply the gate, and re-decompose via eigendecomposition of the reduced density matrix. Cost: $O(\chi^3)$ per gate, independent of $n$.

3. [[Entanglement as Simulation Complexity Measure]] — Use $\log_2 \chi$ (the log-Schmidt-rank) as a direct measure of classical simulation cost. A computation is classically tractable iff this quantity stays $O(\log n)$.

---

## References within this paper

- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]] — the quantum speedup that motivates the question
- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes|Feynman (1982)]] — classical simulation of quantum systems is hard in general
- Valiant (2001), Terhal-DiVincenzo (2002) — classical simulation of free fermions
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes|Gottesman (1997)]] — Clifford/stabiliser simulation
- [[On the Role of Entanglement in Quantum-Computational Speed-Up (Jozsa-Linden 2003) — Paper Notes|Jozsa-Linden (2003, quant-ph/0201143)]] — entanglement necessary for speedup (qualitative version)
- Vidal-Latorre-Rico-Kitaev (2002, quant-ph/0211074) — entanglement scaling in critical vs. non-critical 1D systems; suggests non-critical chains are efficiently simulable
- Knill-Laflamme (1998) — mixed-state computation with little entanglement but possible speedup

---

## Cross-links

### Paper notes
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]] — another class of efficiently simulable circuits
- [[Classical Simulation of Commuting Quantum Computations Implies Collapse of the Polynomial Hierarchy (Bremner-Jozsa-Shepherd 2011) — Paper Notes]] — classical simulation hardness from a different angle (sampling)
- [[The Computational Complexity of Linear Optics (Aaronson-Arkhipov 2011) — Paper Notes]] — BosonSampling hardness; complementary to entanglement-based simulability
- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes]] — the original observation that quantum simulation is classically hard
- [[An Area Law for One-Dimensional Quantum Systems (Hastings 2007) — Paper Notes]] — proves that gapped 1D systems satisfy an area law, explaining *why* the Schmidt rank stays bounded and MPS simulation works
- [[Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) — Paper Notes]] — extends the area law to any 1D state with exponentially decaying correlations

### Trick cards
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Local Gate Update on MPS via Schmidt Redecomposition]]
- [[Entanglement as Simulation Complexity Measure]]

### Sequel
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]] — the practical algorithm (TEBD) that turns this framework into a working simulation method for 1D many-body dynamics via [[Trotter Gate Decomposition for 1D MPS Simulation|Trotter decomposition]]
