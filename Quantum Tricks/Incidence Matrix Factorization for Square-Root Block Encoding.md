
> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, arXiv:2303.13012
> **Tags:** #trick #block-encoding #graph-Laplacian #incidence-matrix #Hamiltonian-simulation #square-root

## What it does
Block-encodes the "square root" of a graph Laplacian $A$ without computing $\sqrt{A}$ explicitly, by factoring $A = BB^\dagger$ through the weighted incidence matrix.

## The trick

Given a force matrix $F$ (graph Laplacian of a weighted graph with spring constants $\kappa_{jk}$ and masses $m_j$), define $A = \sqrt{M}^{-1} F \sqrt{M}^{-1}$ and construct $B$ from the weighted incidence matrix:

$$\sqrt{M}\,B|j,k\rangle = \begin{cases} \sqrt{\kappa_{jj}}\,|j\rangle & j = k \\ \sqrt{\kappa_{jk}}(|j\rangle - |k\rangle) & j < k \end{cases}$$

Then $BB^\dagger = A$, and the block Hamiltonian

$$H = -\begin{pmatrix} 0 & B \\ B^\dagger & 0 \end{pmatrix}$$

satisfies $H^2|_{\text{top block}} = A$. So $e^{-itH}$ propagates the "square-root dynamics" of $A$ — evolution under $H$ is equivalent to evolution under $\sqrt{A}$ up to a basis reshuffling.

The incidence matrix entries $\sqrt{\kappa_{jk}/m_j}$ can be block-encoded using a single sparse-state preparation plus inequality testing, giving normalization $\Lambda = \sqrt{2\aleph d}$ where $\aleph = \kappa_{\max}/m_{\min}$ and $d$ is the sparsity.

## When to reach for it

- You have a PSD matrix $A$ and need to simulate dynamics under $\sqrt{A}$ (or equivalently, second-order ODEs $\ddot{\vec{y}} = -A\vec{y}$).
- $A$ has a natural graph Laplacian structure (or can be written as $BB^\dagger$ for some sparse $B$).
- Direct computation of $\sqrt{A}$ via [[Qubitization (Quantum Walk for Spectral Encoding)|phase estimation]] would be too expensive (it adds $O(\|A\|_{\max} d / \varepsilon_{\text{PE}})$ overhead).

## Complexity

Block encoding of $B$ uses $O(1)$ oracle queries plus $O(n + \log^2(m_{\max}/(m_{\min}\varepsilon')))$ two-qubit gates. The resulting block encoding of $H$ inherits this cost with $O(n)$ additional gates for the conditional structure.

## Caveat

Requires the matrix to decompose as $BB^\dagger$ with $B$ sparse. Works naturally for graph Laplacians (spring networks, discretized differential operators) but not for arbitrary PSD matrices. For the general case, the paper falls back to phase estimation (Theorem 4), which has worse $\varepsilon$ dependence.

## Related notes
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Square-Root Sparsity in Block Encoding]]
- [[Energy-Balanced Encoding for Classical-to-Quantum Reduction]]
