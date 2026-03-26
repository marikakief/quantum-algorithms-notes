# Classical Circuit Recompilation for Depth Reduction

> **Source:** Tazhigulov, Sun, Haghshenas, Zhai, Tan, Rubin, Babbush, Minnich, Chan, arXiv:2203.15291; Sun et al., PRX Quantum 2, 010317 (2021)
> **Tags:** #trick #circuit-compilation #NISQ #variational #depth-reduction #recompilation

## What it does

Compresses an exact target unitary (known classically) into a fixed-depth circuit of native hardware gates by variationally optimizing the circuit parameters to maximize state-specific fidelity.

## The trick

Given a target unitary $U$ and an initial state $|\psi_0\rangle$, construct a brickwork ansatz $\bar{U}(\vec{\theta})$ built from the hardware's native gate set (e.g., alternating layers of $\sqrt{iSWAP}^\dagger$ and PhasedXZ). Optimize:

$$\min_{\vec{\theta}} \| U|\psi_0\rangle - \bar{U}(\vec{\theta})|\psi_0\rangle \|$$

The optimization is **state-specific**: the compressed circuit only needs to reproduce the action of $U$ on the specific initial state $|\psi_0\rangle$, not on all inputs. This is strictly easier than full unitary compilation and allows much deeper compression.

For QITE, the target $U$ encodes imaginary-time evolution followed by optional real-time evolution. The exact $U$ is computed classically (possible because the systems are small), then compressed into a circuit of fixed depth — typically chosen to achieve $>90\%$ fidelity.

**Circuit structure:** Each layer consists of:
- A layer of single-qubit PhasedXZ gates
- A layer of two-qubit $\sqrt{iSWAP}^\dagger$ gates (respecting hardware connectivity)

The number of layers (circuit depth) is a hyperparameter trading fidelity for noise resilience.

## When to reach for it

- Benchmarking quantum hardware on small-to-moderate problems where the exact answer is classically known.
- Any NISQ experiment where the target unitary has more structure than a fixed-depth circuit can represent, but the action on a specific input state is simpler.
- When Trotter decomposition of a time-evolution operator produces circuits far too deep for the hardware (e.g., the 6-site Kitaev-Heisenberg model exceeds supremacy gate count after just 3 Trotter time units).
- When combined with [[Floquet Gate Calibration for Circuit Recompilation|Floquet calibration]], the recompilation can absorb known coherent errors.

## Complexity

- **Classical optimization:** Scales as $O(4^n)$ in the worst case for $n$-qubit unitaries (the Hilbert space dimension), but state-specific optimization on $2^n$-dimensional vectors is tractable for $n \lesssim 30$.
- **Quantum cost:** The resulting circuit has a fixed, shallow depth — that's the whole point.
- **Fidelity–depth tradeoff:** More layers give higher fidelity but also more hardware noise. The optimal depth depends on the gate error rate.

## Caveat

- **Not scalable to classically intractable problems.** The entire procedure requires classically constructing $U|\psi_0\rangle$, which is exponentially expensive for large systems. This is a fundamental limitation that cannot be circumvented.
- The $>90\%$ recompilation fidelity still contributes measurable error. In the P-cluster experiment, the gap between "exact" and "no-noise recompiled" results is visible in the correlation functions.
- State-specific compilation means a separate optimization for each initial state in the QITE thermal average. For $2^n$ basis states, this is $2^n$ separate classical optimizations.
- The variational landscape may have local minima, especially for deeper circuits. In practice, good initialization (e.g., from shorter circuits) helps.

## Related notes
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]]
- [[Floquet Gate Calibration for Circuit Recompilation]] — provides the corrected gate model that the recompilation optimizes over
- [[Commuting Hamiltonian Rescaling for Error Mitigation]] — postprocessing applied after the recompiled circuits are run
- [[Variational Quantum Eigensolver (VQE)]] — related variational approach, but VQE optimizes for energy rather than state fidelity
- [[Trotterized Time Evolution for Chemistry]] — the Trotter decomposition that recompilation replaces
