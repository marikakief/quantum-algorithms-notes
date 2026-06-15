# Gelfand-Tsetlin Sparse Hamiltonian Implementation of Lie-Group Irreps

> **Source:** Stephen P. Jordan, arXiv:0811.0562
> **Tags:** #trick #representation-theory #Lie-groups #sparse-Hamiltonian-simulation

## What it does

Implements exponentially large Lie-group irreps by simulating sparse Lie-algebra generators in a Gel'fand--Tsetlin basis.

## The trick

Choose a Gel'fand--Tsetlin basis for a highest-weight irrep. Basis states are interlacing integer patterns, and adjacent Lie-algebra generators change only one pattern entry by $\pm 1$. For $U(n)$, the key actions have the form
$$
a_{\vec m}(E_{p-1,p})M=\sum_{j=1}^{p-1}a^j_{p-1}M^{+j}_{p-1},
$$
$$
a_{\vec m}(E_{p,p-1})M=\sum_{j=1}^{p-1}b^j_{p-1}M^{-j}_{p-1},
$$
with explicitly computable product-form coefficients. Each row therefore has only $O(n)$ nonzero entries.

For a two-level group element $u_p=I_p\oplus u\oplus I_{n-p-2}$, find a logarithm $H_p$ and simulate the represented Hamiltonian $a_{\vec m}(H_p)$. If the highest-weight entries are polynomially bounded, the matrix entries and spectral norm are polynomially bounded, so sparse Hamiltonian simulation implements
$$
A_{\vec m}(u_p)=\exp(a_{\vec m}(H_p))
$$
in polynomial time. Decompose a general $n\times n$ unitary into polynomially many two-level unitaries and concatenate.

## When to reach for it

- The representation dimension is exponential, but the basis has local combinatorial moves.
- Lie-algebra generators are sparse and row-computable in that basis.
- The group element can be decomposed into simple exponentials of those generators.

## Complexity

For polynomially bounded highest weight and explicit $n\times n$ group element, Jordan obtains polynomial-time quantum circuits for matrix-element estimation in $\operatorname{poly}(n,1/\epsilon)$ time. The final $1/\epsilon^2$ factor is from sampling in the Hadamard test.

## Caveat

The highest-weight entries must be polynomially bounded, not merely polynomial-bit integers. This is the main limitation: the method is polynomial in the weight size itself. Also, the construction is only as efficient as the sparse-Hamiltonian simulation primitive used underneath.

## Related notes

- [[Fast Quantum Algorithms for Approximating Some Irreducible Representations of Groups (Jordan 2009) — Paper Notes]]
- [[Sparse Hamiltonian Simulation via Coloring Decomposition]]
- [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
