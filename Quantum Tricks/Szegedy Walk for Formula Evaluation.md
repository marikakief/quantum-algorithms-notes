
> **Source:** Ambainis, Childs, Reichardt, Špalek, Zhang, FOCS 2007; arXiv:quant-ph/0703015 & arXiv:0704.3628
> **Tags:** #trick #quantum-walk #formula-evaluation #szegedy #phase-estimation

## What it does
Evaluates a Boolean NAND formula by detecting whether the weighted adjacency Hamiltonian of its tree has a zero-energy eigenstate, using a coined quantum walk and phase estimation.

## The trick

Given a NAND formula tree $T$ with weighted adjacency matrix $H$:

1. **Encode the input** by disconnecting leaves that evaluate to 1 in the paper's NAND convention (set their edge weight to 0)
2. **Spectral dichotomy:** If $\varphi(x) = 0$, $H$ has a zero-energy eigenstate with large overlap on a known "tail" vertex. If $\varphi(x) = 1$, all eigenstates near zero energy have negligible overlap on the tail.
3. **Quantize via [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes|Szegedy's theorem]]:** For the specially normalized formula Hamiltonian, construct a row-stochastic $P$ with $H = P \circ P^T$. The coined walk $U = (2\Pi - I)S$ has eigenphases determined by the eigenvalues $\lambda_a$ of this $H$.
4. **Phase estimation** on $U$: under this phase convention, zero-energy eigenstates of $H$ map to eigenphases around $\pi/2$ and $3\pi/2$ of $U$. Detect whether the measured phase is near these values.

The weight assignment $H_{pv} = (s_v/s_p)^{1/4}$ (where $s_v$ counts leaves under $v$ in the known formula structure, with leaves counted with multiplicity) is chosen so that: (a) the zero-eigenstate construction telescopes through the tree (Lemma 4), and (b) the spectral gap bound follows from a careful ratio-of-amplitudes argument (Lemma 6).

## When to reach for it
- Evaluating any Boolean formula on black-box inputs
- Game tree evaluation (AND-OR trees = MIN-MAX trees)
- When you need to detect spectral properties of a graph Hamiltonian that depends on oracle data
- As a template for "quantum walk + phase estimation" algorithms

## Complexity
- Balanced formulas: $O(\sqrt{N})$ queries (optimal)
- General formulas: $N^{1/2 + O(1/\sqrt{\log N})}$ after [[Formula Rebalancing for Quantum Evaluation|rebalancing]]

## Caveat
Requires classical preprocessing to compute coin biases (principal eigenvector of $H$). The formula structure must be known in advance — only the leaf values are black-box. For highly unbalanced formulas, the rebalancing step introduces a sub-polynomial overhead.

Later span-program algorithms, especially Reichardt's work, give the cleaner optimal $\Theta(\sqrt{N})$ query bound for read-once formula evaluation; this card is about the 2007 quantum-walk construction.

## Related notes
- [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
- [[Coined Quantum Walk Search on Graphs]]
- [[Gapped Phase Estimation]]
