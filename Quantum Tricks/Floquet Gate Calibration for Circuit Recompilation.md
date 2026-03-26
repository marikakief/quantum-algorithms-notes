# Floquet Gate Calibration for Circuit Recompilation

> **Source:** Tazhigulov, Sun, Haghshenas, Zhai, Tan, Rubin, Babbush, Minnich, Chan, arXiv:2203.15291; Neill et al., Nature 594, 508 (2021)
> **Tags:** #trick #gate-calibration #NISQ #superconducting #circuit-optimization #Sycamore

## What it does

Characterizes the actual implemented parameters of a native two-qubit gate (here $\sqrt{iSWAP}^\dagger$) via Floquet-style repeated-gate experiments, then uses the corrected gate model in classical circuit optimization instead of assuming ideal parameters.

## The trick

An "excitation-number conserving" two-qubit gate like $\sqrt{iSWAP}^\dagger$ is parametrized by 5 angles: $\theta, \phi, \zeta, \gamma, \chi$. Ideally $\theta = \pi/4$ with all others zero. In practice, each physical qubit pair has slightly different actual parameters due to fabrication variation and drift.

**Calibration protocol:**
1. Prepare a known input state on the two qubits.
2. Apply the $\sqrt{iSWAP}^\dagger$ gate repeatedly ($n$ times, varying $n$).
3. The population oscillations as a function of $n$ (Floquet-like) reveal the actual gate parameters via fitting.
4. Repeat for each qubit pair used in the experiment.

**Integration with recompilation:** Feed the measured 5-angle gate model (not the ideal gate) into the classical variational circuit optimizer. The recompiled circuit now targets fidelity against the actual hardware behavior, not the idealized specification.

**Limitation:** Calibration is performed on isolated two-qubit gates. When single-qubit microwave gates (PhasedXZ) are interleaved with two-qubit gates in a brickwork circuit, the microwave pulses can shift the effective two-qubit gate parameters through cross-talk. This uncharacterized error is a residual systematic effect.

## When to reach for it

- Any NISQ experiment using native parametric two-qubit gates (transmon $\sqrt{iSWAP}$, $\mathrm{fSim}$, etc.).
- When the classical circuit compilation assumes specific gate parameters and coherent gate errors dominate.
- Particularly useful when combined with [[Classical Circuit Recompilation for Depth Reduction|classical recompilation]] — the compiler can absorb coherent gate errors into the variational optimization if it knows the true gate.
- Before any experiment where the circuit will be run many times at the same operating point (the calibration overhead is amortized).

## Complexity

- **Calibration overhead:** $O(K)$ circuits per qubit pair, where $K$ is the number of Floquet steps. Typically $K \sim 20$–$50$. For $M$ qubit pairs: $O(KM)$ calibration circuits total.
- **Integration cost:** Zero additional quantum cost — the calibrated parameters just change the classical optimization target.
- **Drift:** Must be repeated before each experiment session (parameters drift on timescales of hours to days).

## Caveat

- Does not capture cross-talk between simultaneous or adjacent gate operations. In brickwork circuits, the interleaving of microwave (single-qubit) and flux (two-qubit) pulses creates coherent errors not present in the isolated-gate calibration.
- The Arute et al. (2020) free-fermion experiment avoided microwave gates entirely, which is why it could deploy more gates than this paper's simulations — the Floquet calibration was fully accurate.
- For architectures where the two-qubit gate parameters depend on the single-qubit context, a more expensive tomographic characterization (e.g., gate set tomography) would be needed.

## Related notes
- [[Simulating Challenging Correlated Molecules and Materials on the Sycamore Processor (Tazhigulov, Sun, Babbush, Chan et al 2022) — Paper Notes]]
- [[Commuting Hamiltonian Rescaling for Error Mitigation]] — used together with Floquet calibration in the same experiments
- [[Classical Circuit Recompilation for Depth Reduction]] — the recompilation that consumes the calibrated gate model
