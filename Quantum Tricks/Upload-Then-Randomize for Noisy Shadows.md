# Upload-Then-Randomize for Noisy Shadows

> **Source:** Kannan, Putterman, and Cotler, arXiv:2605.02057
> **Tags:** #trick #classical-shadows #fault-tolerance #measurement #noise

## What it does

Restores randomized-measurement protocols for fragile experimental states by uploading the state before applying the randomizing circuit.

## The trick

Classical shadows work by applying a random unitary or random measurement basis before readout. For a $k$-local Pauli $P_k$, the measurement ensemble is useful when the resulting shadow norm is small; see [[Shadow Norm as Measurement-Observable Match]].

For states produced by an experiment, the randomizing circuit may itself be the problem. If a depth-$d$ brickwork circuit acts directly on the physical state, depolarising noise between layers damps the Pauli information roughly according to the weights encountered along the Heisenberg evolution:

$$
\omega_{\lambda,d}(P_k)=\mathbb E\!\left[
3^{-l_d}\exp\!\left(-\lambda\sum_{i=1}^d l_i\right)
\right].
$$

The upload-then-randomize pattern changes the order:

```text
bad order:    physical state → random circuit with raw noise → measurement
better order: physical state → upload once → logical random circuit → measurement
```

After uploading, the logical state is

$$
\rho_{\rm log}=U_{\rm enc}D_{\eta}^{\otimes n}(\rho)U_{\rm enc}^{\dagger}.
$$

The logical shadow circuit can be analysed as noiseless fault-tolerant processing. The only correction needed for a $k$-local Pauli is the input-noise rescaling

$$
\operatorname{tr}(\overline P_k\rho_{\rm log})=(1-\eta)^k\operatorname{tr}(P_k\rho).
$$

So the randomized measurement remains a variance-reduction tool instead of becoming a noise amplifier.

## When to reach for it

- Classical shadows on states produced outside the quantum processor.
- Any protocol where the measurement design deliberately spreads a local observable into a larger Pauli support before measurement.
- Near-fault-tolerant experiments where measurement flexibility is valuable but raw coherent control is noisy.

## Complexity

For $P_k$ supported on $k$ contiguous qubits, the uploaded protocol in the paper uses a fault-tolerant shallow-shadow circuit of depth $d=\Theta(\log k)$ and achieves

$$
N=O\!\left(
\frac{3^{3k/4}\operatorname{poly}(k)}{(1-\eta)^{2k}\epsilon^2}
\log\frac1\delta
\right).
$$

Compared with raw noisy brickwork shadows in the paper's small-noise shallow-shadow setup, this gives

$$
\frac{N_{\rm raw}}{N_{\rm inj}}\ge \exp(\Omega(\lambda k))
$$

for small $\lambda$ when the upload noise is not too much larger than the raw noise.

## Caveat

Uploading does not remove the local input-noise penalty $(1-\eta)^{-2k}$. For large $k$ or poor transduction, this can still be severe. The win is that the later randomized circuit no longer accumulates unprotected physical noise layer by layer.

The displayed bounds are not generic classical-shadow bounds. They use the paper's brickwork ensemble, contiguous-support observable model, and comparison between raw noisy preprocessing and uploaded logical preprocessing.

## Related notes

- [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]]
- [[Quantum Uploading as One-Time Input Noise]]
- [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]
- [[Classical Shadow Channel Inversion]]
- [[Shadow Norm as Measurement-Observable Match]]
- [[Median-of-Means Shadow Prediction]]
- [[Triply Efficient Shadow Tomography (King, Gosset, Kothari, Babbush 2024) — Paper Notes]]
