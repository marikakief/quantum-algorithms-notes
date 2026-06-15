> **Source:** Dominic W. Berry, Andrew M. Childs, Richard Cleve, Robin Kothari, and Rolando D. Somma, *Simulating Hamiltonian dynamics with a truncated Taylor series*, arXiv:1412.4687, Phys. Rev. Lett. 114, 090502 (2015)
> **Tags:** #trick #LCU #hamiltonian-simulation #taylor-series #precision-scaling #amplitude-amplification

## What it does

Simulates \(e^{-iHt}\) by expanding each short time segment in a truncated Taylor series and implementing the truncated sum as a [[Linear Combination of Unitaries (LCU)]]. The key advantage over fixed-order product formulas is precision: the required truncation order is

$$
K = \Theta\!\left(\frac{\log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right),
$$

where \(T=\lambda t\) and \(\lambda=\sum_\ell |\beta_\ell|\) is the LCU coefficient 1-norm for \(H=\sum_\ell \beta_\ell U_\ell\).

## The trick

Write the Hamiltonian as an LCU:

$$
H=\sum_{\ell=1}^L \beta_\ell U_\ell,\qquad \lambda=\sum_\ell |\beta_\ell|.
$$

For a segment with normalized time \(\tau=\lambda t/r\), expand

$$
e^{-iH t/r}\approx
\sum_{k=0}^{K}\frac{(-i H t/r)^k}{k!}.
$$

After expanding \(H^k\), each branch is a product \(U_{\ell_1}\cdots U_{\ell_k}\) times a phase from \((-i)^k\) and from the complex signs of the \(\beta_\ell\)'s. PREPARE uses square roots of nonnegative magnitudes; phases belong in SELECT.

### Segmentation

Using the whole interval at once would give coefficient sum \(s\approx e^T\), so ordinary amplification would cost \(O(e^T)\). The BCCKS Taylor algorithm avoids this by segmenting:

$$
r=\lceil T/\ln 2\rceil,\qquad \tau=T/r\le \ln 2.
$$

For each segment, the infinite coefficient sum is \(e^\tau\le 2\). Truncation and amplitude dilution/padding arrange the OAA normalization close enough to the \(s=2\) single-round sweet spot.

### PREPARE and SELECT

PREPARE creates a superposition over \((k,\ell_1,\ldots,\ell_k)\) with amplitudes proportional to

$$
\sqrt{\frac{\tau^k}{k!}\prod_{j=1}^{k}\frac{|\beta_{\ell_j}|}{\lambda}}.
$$

SELECT applies the phase and unitary product:

$$
(-i)^k\prod_{j=1}^{k}\frac{\beta_{\ell_j}}{|\beta_{\ell_j}|}\;
U_{\ell_1}\cdots U_{\ell_k}.
$$

The straightforward implementation uses \(K\) sequential controlled-SELECT calls per segment and \(O(K\log L)\) ancilla qubits for the order and term registers.

### Amplification

The LCU block has success amplitude \(1/s\), so the success probability on input \(|\psi\rangle\) is \(\|\tilde U|\psi\rangle\|^2/s^2\), not a fixed \(1/\lambda\)-type quantity. Because \(\tilde U\) is close to the true segment unitary and \(s\approx2\), [[Oblivious Amplitude Amplification (Robust)]] turns the segment into a near-deterministic implementation with one OAA round.

## Complexity

- **Segments:** \(r=\lceil T/\ln 2\rceil\)
- **Truncation order:** \(K=\Theta(\log(T/\varepsilon)/\log\log(T/\varepsilon))\)
- **SELECT calls:** \(O(rK)=O(T\log(T/\varepsilon)/\log\log(T/\varepsilon))\)
- **Ancilla qubits:** \(O(K\log L)\) for the direct encoding

Compared with QSP/qubitization, Taylor-LCU keeps the precision degree multiplied by \(T\):

$$
\text{Taylor-LCU: } O\!\left(T\frac{\log(T/\varepsilon)}{\log\log(T/\varepsilon)}\right),\qquad
\text{QSP/qubitization: } O\!\left(T+\frac{\log(1/\varepsilon)}{\log\log(1/\varepsilon)}\right).
$$

## When to reach for it

- When precision \(\varepsilon\) must be very small and product-formula polynomial precision scaling is prohibitive.
- When \(H\) already has an efficient LCU/PREPARE/SELECT representation.
- As the clearest route for understanding how LCU, segmentation, and robust OAA fit together before moving to qubitization/QSVT.

## Caveat

- The coefficient norm \(\lambda\) can be much larger than \(\|H\|\), so normalization dominates the cost.
- PREPARE can be the bottleneck when the coefficient distribution is large or unstructured.
- This is not the asymptotic endpoint for Hamiltonian simulation; qubitization/QSP improves Taylor's multiplicative time-precision dependence.

## Related notes
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
- [[Taylor Series Truncation with ln2 Segmentation]]
- [[Linear Combination of Unitaries (LCU)]]
- [[Oblivious Amplitude Amplification (Robust)]]
- [[Standard-Form Encoding (Prepare + Signal Oracle)]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
