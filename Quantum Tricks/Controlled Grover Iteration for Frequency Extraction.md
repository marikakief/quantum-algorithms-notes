# Controlled Grover Iteration for Frequency Extraction

> **Source:** Brassard, Høyer & Tapp, arXiv:quant-ph/9805082
> **Tags:** #trick #counting #phase-estimation #amplitude-estimation

## What it does

Converts a search oracle into a counting oracle by extracting the rotation frequency of the Grover operator via QFT.

## The trick

The Grover operator $G$ rotates the state by angle $2\theta$ in the good/bad subspace at each step, where $\sin^2\theta = t/N$. After $m$ iterations, the amplitude on good states is $\sin((2m+1)\theta)$ — a periodic function of $m$ with frequency $\theta/\pi$.

To extract this frequency:

1. Prepare $\frac{1}{\sqrt{P}}\sum_{m=0}^{P-1}|m\rangle \otimes |\Psi_0\rangle$.
2. Apply controlled-$G^m$: $|m\rangle|\Psi\rangle \mapsto |m\rangle G^m |\Psi\rangle$.
3. Measure the second register (optional — doesn't affect the outcome).
4. Apply QFT to the first register, measure to get $\tilde{f}$.
5. Compute $\tilde{t} = N\sin^2(\tilde{f}\pi/P)$.

The controlled application of $G^m$ creates interference in the $m$ register that encodes the frequency. The QFT extracts it, just as in [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev's phase estimation]], but here the eigenvalues come from the Grover rotation rather than an arbitrary unitary.

## When to reach for it

- Estimating the number of solutions to a search problem.
- Quantum Monte Carlo / mean estimation (counting is the core subroutine).
- Any amplitude estimation task where you want to know $a = \langle\Psi_a|\Psi_a\rangle$ rather than find a good state.

## Complexity

$P$ applications of $G$ (hence $P$ oracle calls) give absolute error $O(\sqrt{tN}/P + N/P^2)$ in $t$. For $\sqrt{t}$-absolute accuracy, $P = O(\sqrt{N})$; for relative $\varepsilon$ accuracy, $P = O(\sqrt{N/t}/\varepsilon)$.

## Caveat

Requires coherent controlled-$G^m$, which doubles the circuit depth per step (control qubit overhead). The $8/\pi^2 \approx 0.81$ success probability per run needs majority-vote amplification for high confidence.

The technique was later absorbed into [[Amplitude Estimation via Phase Estimation on Q|the general amplitude estimation framework]] by [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard et al. (2002)]].

## Related notes
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Phase Estimation on the Grover Operator for Counting]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
