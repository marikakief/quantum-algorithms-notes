# Kaiser-Window Amplitude Estimation

> **Tags:** #trick #amplitude-estimation #signal-processing #tda
> **Source:** Babbush et al. arXiv:2209.13581, "Analyzing Prospects for Quantum Advantage in Topological Data Analysis" (PRX Quantum 2024)

## What it does
Reduces [[Kaiser-Window Amplitude Estimation|spectral leakage]] and constant factors in amplitude estimation by replacing rectangular windows with Kaiser windows.

## The trick
Standard AE/QPE implicitly uses sharp truncation in time/frequency, causing sidelobes (leakage). Apply a [[Kaiser-Window Amplitude Estimation|Kaiser window]] to the sampled phase-estimation kernel:
- broader main lobe, much lower sidelobes
- cleaner isolation of target amplitude/eigenphase
- better practical precision for finite resources

This is classical DSP imported into quantum phase/amplitude estimation.

## When to reach for it
- Precision-limited AE/QPE where leakage dominates error
- Spectral projector construction (e.g., Betti-number subspace)
- Fault-tolerant settings where constants matter

## Complexity
Asymptotics usually unchanged; main gain is constants / robustness for fixed query budget.

## Caveat
Window choice trades resolution vs leakage. Over-smoothing can blur nearby eigenvalues. The [[Kaiser-Window Amplitude Estimation|Kaiser window]] introduces a broader main lobe (slightly more queries needed for the same resolution) in exchange for the much lower sidelobes. The appropriate $\beta$ parameter (controlling the window shape) must be chosen for the specific leakage/resolution tradeoff at hand.

This is classical DSP engineering imported into quantum circuits. The argument is: QPE with a Kaiser-windowed query sequence is equivalent to convolution with a Kaiser filter in frequency space. Standard reference: Harris (1978) "On the use of windows for harmonic analysis with the DFT."
