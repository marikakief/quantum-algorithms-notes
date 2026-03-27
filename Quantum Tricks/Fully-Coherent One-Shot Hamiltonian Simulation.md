
> **Tags:** #trick #hamiltonian-simulation #QSP #QET #fully-coherent #one-shot #block-encoding
> **Source:** Martyn, Liu, Chin, Chuang (2023), Sec. III, Theorem 5
> **See also:** [[Affine Spectrum Compression for QSP Parity Bypass]], [[Oblivious Amplitude Amplification (Robust)]], [[Jacobi-Anger Truncation for QSP Simulation]]

## What it does

Simulates $e^{-iHt}$ to error $\epsilon$ with success probability $\ge 1-\delta$, using a single QET sequence (no amplitude amplification, no postselection), so the circuit can be embedded as-is inside a larger coherent algorithm. Query complexity:
$$\Theta\!\left(\alpha|t| + \ln(1/\epsilon) + \ln(1/\delta)\right),$$
where $\alpha \ge \|H\|$ is the block-encoding normalization.

Failure probability $\delta$ is controlled additively and independently of $\epsilon$ — you can tune $\delta = \Theta(\epsilon)$ if you want a single precision parameter.

## When to use this

- [[Hamiltonian simulation]] appears as a subroutine inside a larger coherent algorithm (e.g., you're building a larger block-encoding or an LCU decomposition of a time-dependent Hamiltonian).
- You're implementing time-dependent simulation via Dyson-series or interaction-picture methods and need to concatenate many simulation steps without postselection costs compounding.
- You want clean $\ln(1/\epsilon)$ precision scaling and independent control of failure probability.

**If standalone simulation is sufficient** (no larger coherent context), plain QSP-LCU or qubitization with ROAA is fine and may have simpler circuits.

## The recipe

### Inputs
- Block-encoding oracle $U_H$ for $H/\alpha$, with $\alpha \ge \|H\|$.
- Target time $t$, error $\epsilon$, failure tolerance $\delta$.

### Step 1: Affine pre-transformation

Choose compression parameter $\beta \in (0,1)$ (typically close to 1). Construct a block-encoding of
$$\tilde{H} = \frac{1}{2}\!\left(I + \frac{\beta H}{\alpha}\right)$$
using the [[Block-Encoding Composition Algebra|block-encoding composition lemma]] and one additional ancilla qubit. This shifts the spectrum of $H/\alpha$ from $[-1,1]$ to $[(1-\beta)/2,(1+\beta)/2] \subset (0,1)$.

Cost: $O(1)$ queries to $U_H$, plus a constant overhead.

See [[Affine Spectrum Compression for QSP Parity Bypass]] for the full circuit description.

### Step 2: Polynomial approximation

Set effective evolution parameter $\tau = \alpha t/\beta$ (so that $e^{-i\tilde{H}\tau}$ restricted to the compressed spectrum reproduces $e^{-iHt}$).

Find a polynomial $P(x)$ of degree $d = \Theta(\alpha|t| + \ln(1/\epsilon) + \ln(1/\delta))$ that approximates the **even extension of the complex exponential (EECE)**:
$$\mathrm{EECE}(x;\tau) = \cos(\tau x) - i\sin(\tau x)\,\mathrm{sign}(x)$$
to error $\min(\epsilon, \delta/2)$ on the interval $[(1-\beta)/2,(1+\beta)/2]$.

On this interval $\mathrm{sign}(x) = +1$, so $\mathrm{EECE}(x;\tau) = e^{-ix\tau}$ there. The global function $\mathrm{EECE}(x;\tau)$ is even — no parity obstruction.

Polynomial synthesis: use standard minimax or Chebyshev-based approximation restricted to the interval. The degree bound follows from the approximation theory of the complex exponential.

### Step 3: QET sequence

Apply QET with polynomial $P$ to $\tilde{H}$. This implements (up to error $\epsilon$):
$$P(\tilde{H}) \approx e^{-i\tilde{H}\tau} \Big|_{\text{spectrum of }\tilde{H}} = e^{-iHt}.$$

The QET sequence has $d$ alternating applications of the block-encoding oracle and phase gates (see [[Phased Qubitization Sequence]] for the general pattern).

### Output

The circuit implements $e^{-iHt}$ on the system register with:
- Error: $\le \epsilon$ in operator norm on the relevant subspace
- Success probability: $\ge 1 - 2\epsilon$ (from the polynomial approximation error; failure means the ancilla is not in $|0\rangle$ but this happens with probability $\le 2\epsilon$)
- If you need $\delta$ independent of $\epsilon$: set the polynomial error $\le \delta$ instead, paying $\ln(1/\delta)$ in degree

## Comparison with ROAA-based approaches

| Property | QSP-LCU + ROAA (Thm 3) | One-shot (Thm 5) |
|---|---|---|
| Amplitude amplification? | Yes (3 queries) | No |
| Failure probability | $\delta = 2\epsilon$ (tied) | $\delta$ independent |
| Precision scaling | $O(\ln(1/\epsilon)/\ln\ln(1/\epsilon))$ | $O(\ln(1/\epsilon))$ (clean) |
| $\delta$ overhead | — | Additive $+\ln(1/\delta)$ |
| Circuit complexity | LCU + OAA rounds | Single QET sequence |

One-shot is cleaner and slightly more flexible. ROAA-based methods may be preferable when an existing LCU structure is already in hand and you need $\delta = \Theta(\epsilon)$ only.

## Key lemma: block-encoding composition (Lemma 4)

If $V_A$ block-encodes $A$ (with normalization $\alpha_A$) and $V_B$ block-encodes $B$ (with normalization $\alpha_B$), then $(I_B \otimes V_A)(V_B \otimes I_A)$ block-encodes $AB$ with normalization $\alpha_A \alpha_B$.

This is used to build the affine pre-transformation from the original $H/\alpha$ block-encoding.

## Caveats

- Polynomial synthesis for $\mathrm{EECE}(x;\tau)$ may require numerical methods (Remez algorithm or Chebyshev-Bessel expansion). The degree is provably $\Theta(\alpha|t| + \ln(1/\epsilon))$ but the constant depends on $\beta$.
- If $\alpha$ is much larger than $\|H\|$, the query complexity is dominated by $\alpha|t|$, not $\|H\|t$. Tightening $\alpha$ (e.g., via better block-encoding constructions) directly reduces query count.
- The construction assumes $\beta < 1$; if $H/\alpha$ already has spectrum in $[0,1]$ (e.g., $H$ is PSD), you can skip the affine step.

## Related paper notes

- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]
