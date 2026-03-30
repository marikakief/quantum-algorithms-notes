# Hadamard Test for Trace Estimation

> **Source:** E. Knill, R. Laflamme, arXiv:quant-ph/9802037 (1998); widely used in subsequent literature
> **Tags:** #trick #trace-estimation #DQC1 #phase-estimation #Hadamard-test

## What it does

Estimates the normalised trace $\frac{1}{2^n}\mathrm{tr}(U)$ (or more generally, $\langle \psi | U | \psi \rangle$ for a known state) using a single ancilla qubit and controlled-$U$.

## The trick

Given an $n$-qubit unitary $U$ implemented as a quantum circuit:

1. Prepare ancilla qubit $a$ in $|0\rangle$, target register in state $|\psi\rangle$ (or $I/2^n$ for DQC1)
2. Apply Hadamard $H$ to qubit $a$
3. Apply controlled-$U$ (controlled by $a$) to the target register
4. Apply $H$ to qubit $a$ again
5. Measure qubit $a$ in the computational basis

The probability of measuring $|0\rangle$ is:

$$p_0 = \frac{1}{2}(1 + \mathrm{Re}\langle\psi|U|\psi\rangle)$$

To get the imaginary part, replace the first $H$ with $H \cdot S^\dagger$ (i.e., prepare the ancilla in $|0\rangle - i|1\rangle$ instead of $|0\rangle + |1\rangle$):

$$p_0 = \frac{1}{2}(1 + \mathrm{Im}\langle\psi|U|\psi\rangle)$$

**For DQC1** (target register in $I/2^n$): the expectation value $\langle \sigma_z^{(a)} \rangle$ directly gives $\mathrm{Re}(\mathrm{tr}(U)/2^n)$ or $\mathrm{Im}(\mathrm{tr}(U)/2^n)$, depending on the preparation of the ancilla.

**For spectral estimation**: apply the controlled version of $U(t) = e^{-iHt}$ for many values of $t$, collect $\mathrm{tr}(e^{-iHt})/2^n$, then Fourier transform. This gives a broadened energy spectrum $\sum_i \delta(\omega - \lambda_i)$ convolved with a resolution kernel.

## When to reach for it

- Estimating expectation values $\langle \psi | U | \psi \rangle$ when you don't want to (or can't) do full [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]]
- Spectral density estimation for Hamiltonians
- Partition function estimation (trace of $e^{-\beta H}$ via Wick rotation or other techniques)
- Fidelity estimation between a known unitary and an unknown process
- The core subroutine inside [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes|quantum subspace methods]] for computing matrix elements of the Hamiltonian in a Krylov basis

## Complexity

$O(1/\varepsilon^2)$ repetitions to estimate the trace to additive precision $\varepsilon$. Each repetition requires one controlled-$U$ application. No amplitude amplification is used — this is a shot-noise-limited protocol.

When combined with [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|phase estimation]], Heisenberg-limited precision $O(1/\varepsilon)$ is achievable, but that requires coherent processing of multiple controlled-$U^{2^k}$ operations and pure ancillas.

## Caveat

The controlled-$U$ gate can be expensive. If $U$ is a circuit of $G$ gates, the controlled version typically costs $O(G)$ additional gates (with some overhead for controlling each gate). For some unitaries, constructing controlled-$U$ is the dominant cost.

In the DQC1 setting, the signal is inherently $O(1/2^n)$ for extracting specific matrix elements (as opposed to the normalised trace), requiring exponential repetitions. The normalised trace itself has $O(1)$ signal-to-noise.

## Related notes
- [[Power of One Bit of Quantum Information (Knill-Laflamme 1998) — Paper Notes]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
- [[Quantum Algorithms Revisited (Cleve-Ekert-Macchiavello-Mosca 1998) — Paper Notes]]
- [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
