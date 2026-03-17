# Qubitization of Block-Encoded Dirac Operators

> **Tags:** #trick #qubitization #block-encoding #tda
> **Source:** Babbush et al. arXiv:2209.13581, "Analyzing Prospects for Quantum Advantage in Topological Data Analysis" (PRX Quantum 2024)

## What it does
Turns a [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] of Dirac/Laplacian into a walk operator whose eigenphases encode the spectrum, enabling precise spectral processing.

## The trick
1. Build [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] of Dirac operator $D$ (or scaled Laplacian)
2. Apply [[Qubitization Iterate|qubitization]] to produce unitary walk $W$
3. Eigenvalue $\lambda$ of $D$ maps to eigenphase $\theta = \arccos(\lambda/\alpha)$ (or equivalent transform)
4. Run phase estimation / polynomial filters on $W$ instead of simulating $e^{-iDt}$ directly

In TDA, this supports kernel detection (Betti numbers) via phase-space filtering.

## When to reach for it
- You already have good [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] ([[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]+[[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]])
- Need repeated spectral transforms (filters, thresholding)
- Want query-optimal dependence on simulation time/precision

## Complexity
Query complexity scales with [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] normalisation and target precision/gap; often better constants than product-formula simulation for spectral tasks.

## Caveat
Great for spectral extraction; not always best for plain fixed-time evolution when Hamiltonian simulation primitives are already cheap.

## Related Paper Notes

- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
