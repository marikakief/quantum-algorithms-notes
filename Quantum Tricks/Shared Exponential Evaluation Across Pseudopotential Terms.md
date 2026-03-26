# Shared Exponential Evaluation Across Pseudopotential Terms

> **Source:** Berry, Rubin, Babbush et al., arXiv:2312.07654
> **Tags:** #trick #block-encoding #pseudopotential #resource-reduction #first-quantized

## What it does

Restructures the block encoding of local and nonlocal pseudopotential operators so that both use a single evaluation of the negative exponential function, despite having different functional forms and different arguments.

## The trick

The GTH pseudopotential has two pieces that both involve Gaussian-type exponentials:

**Nonlocal part** for angular momentum $l$, projectors $i, j$:
$$\tilde{F}^i_l(r^\alpha_l \|k_q\|) \cdot \tilde{F}^j_l(r^\alpha_l \|k_p\|) \propto \text{polynomial}(r^\alpha_l \|k_q\|, r^\alpha_l \|k_p\|) \cdot e^{-[(r^\alpha_l \|k_q\|)^2 + (r^\alpha_l \|k_p\|)^2]/2}$$

**Local part** (the Gaussian-screened nuclear potential):
$$u^\alpha_{\text{loc}}(k_\nu) \propto \text{polynomial}(r^\alpha_{\text{loc}} \|k_\nu\|) \cdot e^{-(r^\alpha_{\text{loc}} \|k_\nu\|)^2/2}$$

The nonlocal part depends on both $k_q$ (system momentum) and $k_p = k_{q-\nu}$ (shifted momentum). The local part depends only on $k_\nu$ (the momentum transfer).

The insight: set $q = 0$ for the local pseudopotential. Then $k_p = k_{q-\nu} = -k_\nu$, so:
- The argument to the exponential becomes $(r^\alpha_{\text{loc}} \|k_\nu\|)^2 / 2 + 0 = (r^\alpha_{\text{loc}} \|k_\nu\|)^2 / 2$ — exactly what the local part needs
- The polynomial evaluations $\tilde{F}^j_0(r^\alpha_{\text{loc}} \|k_\nu\|)$ give the correct Gaussian-polynomial products for the local terms
- The $l = 0$ Legendre polynomial is just 1, so no angular-momentum-dependent correction is needed

In the circuit, a qubit $\varsigma$ selects between local and nonlocal. Controlled on $\varsigma$, either the actual $q$ or zero is copied into the working register. The rest of the arithmetic pipeline is shared.

The $C^\alpha_1, C^\alpha_2, C^\alpha_3, C^\alpha_4$ polynomial terms of the local pseudopotential map directly to the $l = 0$, $i = 1$, $j = 1, 2, 3, 4$ terms of the nonlocal pseudopotential, because the polynomial coefficients $c_{x,lj}$ are the same.

## When to reach for it

- Whenever a block encoding requires evaluating the same expensive function (exponentials, trigonometric functions) for multiple Hamiltonian terms with related but not identical arguments
- More broadly: when two operators in an LCU share computational structure, combine them into a single arithmetic pipeline controlled by a selection qubit, rather than computing them separately
- Applicable beyond pseudopotentials — any situation with local + nonlocal potentials sharing Gaussian-type radial functions

## Complexity

Saves one full exponential evaluation (~$b^2$ to $(11/4)b^2$ Toffolis depending on interpolation order). For $b = 20$, this is 400–1,100 Toffolis saved. The overhead is one controlled copy of $\sim n$ qubits, costing $O(n)$ Toffolis — negligible.

## Caveat

Only works when the local and nonlocal potentials share the same functional form for the expensive primitive. For pseudopotentials with different radial functions (e.g., PAW pseudopotentials), the trick wouldn't apply directly.

Also, forcing $q = 0$ for the local part means the local pseudopotential must use $l = 0$ — which is the case for GTH, but not for all pseudopotential parameterisations.

## Related notes
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — The paper introducing this trick
- [[QROM Interpolation for Negative Exponential]] — The exponential evaluation being shared
- [[Arithmetic vs QROM for Pseudopotential Evaluation]] — The broader principle: use arithmetic, not data loading
