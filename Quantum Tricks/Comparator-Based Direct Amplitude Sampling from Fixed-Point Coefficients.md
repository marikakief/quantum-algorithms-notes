
> **Tags:** #trick #state-preparation #amplitude-loading #comparators
> **Source:** arXiv:2510.08644 (Liu, Zhu, Lin, Low, Yang), Section V.2

## What it does

Implements the amplitude oracle $O_A$ — preparing a quantum state with amplitudes proportional to coefficient magnitudes $|h_j|$ — using coherent comparators against fixed-point coefficient registers, avoiding the need for bespoke controlled rotations (which are T-gate expensive). This is the "direct sampling method" underlying the $\tilde{O}(\sqrt{L})$ T-count result.

## The trick

The standard approach for amplitude loading is alias sampling (a.k.a. Grover-Rudolph): [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] amplitudes $\alpha_j = \sqrt{|h_j|/\sum |h_k|}$ by a sequence of controlled rotations. Each rotation costs several T gates and the total is $O(L)$.

The direct sampling method instead:

1. [[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]] a uniform superposition over $m_b$-bit strings: $\frac{1}{\sqrt{2^{m_b}}}\sum_x |x\rangle$ (one Hadamard per bit, $m_b = O(\log(L/\varepsilon))$).
2. Load the $m_b$-bit fixed-point representation of $|h_j|$ into a register (via QROM or the SELECT-SWAP lookup).
3. Apply a **coherent comparator**: output $|1\rangle$ if $x \leq h_j \cdot 2^{m_b}$, $|0\rangle$ otherwise.
4. The acceptance probability for index $j$ is $\Pr[x \leq h_j \cdot 2^{m_b}] = h_j / h_\text{max} + O(2^{-m_b})$ — proportional to $h_j$.

Combined with the SELECT-SWAP outer lookup, this samples from the distribution $(h_j/\sum h_k)$ via the ratio of the comparator acceptance and the selection weight.

Comparators over $m_b$-bit registers cost $O(m_b)$ T gates — logarithmic in precision, compared to $O(L)$ for alias sampling. This is what enables the $\tilde{O}(\sqrt{L})$ overall count.

## When to reach for it

- Coefficient-table-based block-encodings with many terms $L$ and fixed-point coefficients
- Any amplitude oracle where the coefficient distribution doesn't have special structure (random, dense)
- When building the amplitude oracle as part of a SELECT-SWAP grouped data lookup

## Complexity

Per-call: $O(m_b) = O(\log(L/\varepsilon))$ T gates for the comparator, plus the cost of loading $h_j$ from the table. Total with SELECT-SWAP: $\tilde{O}(\sqrt{L})$ T gates.

## Caveat

- Introduces a small bias in sampled amplitudes from fixed-point truncation: order $2^{-m_b}$. Choosing $m_b = \lceil \log(L/\varepsilon)\rceil$ makes this negligible.
- The comparator-based method samples from a distribution that is close to but not exactly proportional to $h_j$ — need to account for the rejection probability carefully in the [[Standard-Form Encoding (Prepare + Signal Oracle)|block-encoding]] normalization.
- This is a "[[Standard-Form Encoding (Prepare + Signal Oracle)|PREPARE]]" oracle approach; the "[[Standard-Form Encoding (Prepare + Signal Oracle)|SELECT]]" oracle (choosing which unitary to apply) has separate cost.

## Related Paper Notes
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]
- [[Black-Box Quantum State Preparation Without Arithmetic (Sanders-Low-Scherer-Berry 2019) — Paper Notes]] — the foundational paper introducing inequality-test amplitude transduction
- [[Inequality-Test Amplitude Transduction]] — the original trick this generalises
- [[SELECT-SWAP Grouped Data Lookup for Block-Encoding]]
