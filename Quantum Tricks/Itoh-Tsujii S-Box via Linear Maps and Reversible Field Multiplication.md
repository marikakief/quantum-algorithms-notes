# Itoh-Tsujii S-Box via Linear Maps and Reversible Field Multiplication

> **Source:** Grassl, Langenberg, Roetteler, Steinwandt, arXiv:1512.04965
> **Tags:** #trick #finite-fields #AES #reversible-circuits #CliffordT

## What it does

Implements an AES-style finite-field inverse by making Frobenius powers CNOT-only and spending T gates only on field multiplications.

## The trick

For the AES S-box, view a byte as
$$
\alpha \in \mathbb{F}_{256}=\mathbb{F}_2[x]/(1+x+x^3+x^4+x^8).
$$
The nonlinear step is inversion, with $0$ fixed. Use the Itoh-Tsujii exponentiation pattern
$$
\alpha^{-1}=\alpha^{254}=\left((\alpha\cdot \alpha^2)\cdot(\alpha\cdot\alpha^2)^4\cdot(\alpha\cdot\alpha^2)^{16}\cdot\alpha^{64}\right)^2.
$$

In characteristic two, Frobenius powers $\alpha\mapsto \alpha^{2^j}$ are linear maps over $\mathbb{F}_2$. In a fixed polynomial basis they are just binary matrices, so they can be synthesized using CNOT gates and wire relabelling. The only T-heavy operations are the field multiplications.

Grassl--Langenberg--Roetteler--Steinwandt use a Maslov--Mathew--Cheung--Pradhan multiplier for the AES basis:

$$
\text{one }\mathbb{F}_{256}\text{ multiplication}: 64\ \text{Toffolis} + 21\ \text{CNOTs}.
$$

After exploiting duplicate intermediate products and uncomputing scratch, their S-box inversion plus affine layer costs
$$
3584\ T + 4569\ \text{Clifford gates}
$$
using 40 qubits for the byte plus temporary workspace.

## When to reach for it

- Reversible circuits for AES, Rijndael-like ciphers, or any primitive dominated by small-field inversion.
- Resource estimates where T-count matters more than CNOT count.
- Situations where fixed linear maps are cheap and field multipliers are the main cost.
- Designing arithmetic oracles over $\mathbb{F}_{2^m}$ with repeated squaring/powering patterns.

## Complexity

For $\mathbb{F}_{2^m}$, the same pattern reduces exponentiation chains to:

- CNOT-only linear maps for all $x\mapsto x^{2^j}$ steps;
- reversible field multipliers for nonlinear products;
- uncomputation to clean intermediate registers.

For the AES case, the paper's chosen multiplier gives 64 Toffolis per multiplication, and the full S-box uses eight such multiplications in the compute/uncompute schedule:
$$
8\cdot 64\cdot 7 = 3584\ T.
$$

## Caveat

This is not a minimum-T AES S-box. The authors choose a low-qubit multiplier and a 40-qubit S-box architecture. A 9-qubit permutation-synthesis S-box is also described, but it costs up to 9,695 T gates. Later AES circuits can improve constants; the reusable point is the separation between cheap linear Frobenius maps and expensive reversible multipliers.

## Related notes

- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[Optimizing Quantum Circuits for Arithmetic (Häner-Roetteler-Svore 2018) — Paper Notes]]
- [[Halving the Cost of Quantum Addition (Gidney 2018) — Paper Notes]]
