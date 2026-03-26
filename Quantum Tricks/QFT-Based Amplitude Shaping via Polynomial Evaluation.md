# QFT-Based Amplitude Shaping via Polynomial Evaluation

> **Source:** Jordan, Shutty, Wootters, Zalcman, Schmidhuber, King, Isakov, Khattar, Babbush, arXiv:2408.08292
> **Tags:** #trick #quantum-optimization #QFT #amplitude-shaping #state-preparation #DQI

## What it does

Prepares a quantum state whose amplitudes are a chosen polynomial function of an objective value, so that computational-basis measurement is biased toward strings with high objective value.

## The trick

For an objective function $f(\mathbf{x})$ whose Fourier transform is sparse (has $m$ nonzero coefficients), prepare a superposition of weighted [[Dicke State Preparation via Inequality Testing|Dicke states]] $\sum_{k=0}^{\ell} w_k |D_{m,k}\rangle$ in the Fourier domain, apply appropriate phases encoding the instance, compute the syndrome, decode to uncompute garbage, and apply the quantum Fourier transform. The result is:

$$|P(f)\rangle = \sum_{\mathbf{x}} P(f(\mathbf{x})) |\mathbf{x}\rangle$$

where $P$ is a degree-$\ell$ polynomial determined by the weights $w_0, \ldots, w_\ell$. Measuring in the computational basis samples $\mathbf{x}$ with probability $\propto P(f(\mathbf{x}))^2$.

The polynomial degree $\ell$ controls the bias strength: higher degree means stronger preference for near-optimal solutions, but requires decoding more errors.

The optimal weights $w_k$ that maximize the expected satisfied constraints are determined by the largest eigenvalue of a specific $(m, \ell, d)$-dependent matrix and can be found classically in $\text{poly}(m)$ time.

## When to reach for it

- You have an optimization problem whose objective has a sparse Fourier expansion (e.g., linear constraints yield $m$-sparse Fourier transforms).
- You want to bias quantum sampling toward high-value solutions without iterative methods like QAOA or adiabatic evolution.
- You can afford a single-shot state preparation + measurement protocol (no variational loop, no phase estimation).

## Complexity

$O(m^2)$ gates for Dicke state preparation + $O(T)$ gates for the decoder, where $T$ is the reversible decoder circuit size. Single measurement per state preparation. Classical polynomial-time precomputation of optimal weights.

## Caveat

The Fourier sparsity of the objective function is what makes this work. For objectives with dense Fourier spectra, the initial state preparation would require $2^n$ terms and this approach offers no advantage. Also, increasing $\ell$ doesn't help if there's no efficient decoder for that many errors — the bias strength is capped by decoding capability.

## Related notes
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]
- [[DQI Semicircle Law for Optimization-Decoding Duality]]
- [[Optimization-to-Decoding Reduction via Code Duality]]
- [[Dicke State Preparation via Inequality Testing]]
