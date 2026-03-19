# Mixing Lemma for Block-Encodings

> **Source:** John M. Martyn, Patrick Rall, *Halving the Cost of Quantum Algorithms with Randomization*, arXiv:2409.03744, npj Quantum Information **11**, 51 (2025), Lemma 2
> **Tags:** #trick #block-encoding #channel-approximation #mixing-lemma #QSP #QSVT

## What it does

Extends the Hastings-Campbell mixing lemma from bare unitaries to block-encoded operators. This is the key technical step that makes [[Stochastic QSP via Chebyshev Tail Truncation|Stochastic QSP]] work: QSP circuits act in a block-encoded subspace, so you can't directly apply the unitary-level mixing lemma.

## The trick

**Setup:** A [[Block-Encoding Composition Algebra|block-encoding]] of operator $A$ is a unitary $U_A$ such that $(\langle 0| \otimes I) U_A (|0\rangle \otimes I) = A/\alpha$. A QSP circuit $U_{P(A)}$ implements a polynomial transformation of $A$ as its top-left block.

**Unitary mixing lemma (Hastings-Campbell, Lemma 1):** If unitaries $\{U_j\}$ with probabilities $\{p_j\}$ satisfy:
- $\|U_j - V\| \le a$ (spectral norm, each individual circuit)
- $\|\sum_j p_j U_j - V\| \le b$ (average circuit close to target)

Then the randomized channel $\Lambda(\rho) = \sum_j p_j U_j \rho U_j^\dagger$ satisfies:
$$\|\Lambda - \mathcal{V}\|_\diamond \le a^2 + 2b$$

**Why this doesn't directly apply to QSP:** QSP circuits approximate $F(A)$ only in the $(0,0)$ block — the full unitary $U_{P_j}$ acts on a larger Hilbert space and does not approximate the target unitary $U_F$ in full spectral norm. The individual-error bound $\|U_{P_j} - U_F\| \le a$ does not hold for the full unitary.

**The block-encoding version (Lemma 2):** For block-encoded operators implementing $P_j(A)$ and $F(A)$, with projector $\Pi = |0\rangle\langle 0| \otimes I$:

If the block-level errors satisfy:
- $\|P_j(A) - F(A)\| \le a$ (spectral norm on the block, each individual)
- $\|\sum_j p_j P_j(A) - F(A)\| \le b$ (average block error)

Then the channel $\Lambda_A(\rho_S) = \mathrm{tr}_{\mathrm{anc}}[\sum_j p_j U_{P_j} (\Pi_0 \otimes \rho_S) U_{P_j}^\dagger]$ satisfies:
$$\|\Lambda_A - \mathcal{F}_A\|_\diamond \le a^2 + 2b$$

The proof tracks the block error through the ancilla trace-out, showing that the diamond-norm bound transfers from the block to the full channel. The key insight is that the channel only "sees" the $(0,0)$ block, so full-unitary closeness is not needed.

## When to reach for it

Whenever you want to apply randomized compiling (mixing-lemma arguments) to an algorithm that uses block-encodings or QSP/QSVT circuits. The unitary mixing lemma does not apply directly to block-encoded circuits; this lemma closes that gap.

Concretely: any situation where you have an ensemble of block-encoded operators approximating a target, and you want to conclude that the resulting randomized channel is close to the target channel in diamond norm.

## Complexity

The lemma itself adds no query overhead — it's an analysis tool. The cost is whatever the ensemble of block-encoded circuits costs to run.

## Caveat

- The output is a quantum channel, not a single unitary. You cannot use this to compose block-encoded unitaries coherently.
- The bound $a^2 + 2b$ assumes the individual circuits are run once per sample. For protocols that run a circuit multiple times coherently, this argument doesn't apply.
- The proof requires that the projector $\Pi_0 = |0\rangle\langle 0| \otimes I$ is the same for all circuits in the ensemble — if different circuits have different block-encoding ancilla structures, additional care is needed.

## Related notes

- [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]]
- [[Stochastic QSP via Chebyshev Tail Truncation]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[Mixing-Lemma Upgrade from Average-Channel Error to Diamond Norm]]
- [[Block-Encoding Composition Algebra]]
- [[QSVT Meta-Template]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]] — original Hastings-Campbell mixing lemma context
