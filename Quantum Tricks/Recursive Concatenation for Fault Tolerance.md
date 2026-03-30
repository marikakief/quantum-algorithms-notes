# Recursive Concatenation for Fault Tolerance

> **Source:** Aharonov and Ben-Or, arXiv:quant-ph/9906129
> **Tags:** #trick #fault-tolerance #concatenation #threshold #error-correction

## What it does
Reduces the effective error rate of a quantum computation superexponentially by repeatedly encoding the circuit at multiple levels, where each level uses a constant-size error-correcting code to reduce the error rate from the level below.

## The trick
Start with the original circuit $M_0$ and a quantum error-correcting code $C$ of block size $m$ that corrects $d$ errors. Build $M_1$: encode each qubit of $M_0$ in a block of $m$ qubits, replace each gate with a fault-tolerant procedure, and add error correction after each procedure.

If the physical error rate $\eta$ is below a threshold $\eta_c$ (determined by the code and procedures), then $M_1$ has a lower effective error rate $\eta_1 < \eta$.

Now iterate: encode $M_1$ to get $M_2$, and so on. After $r$ levels, the effective error rate satisfies:

$$\eta_r \approx \eta_c \left(\frac{\eta}{\eta_c}\right)^{2^r}$$

(for distance-3 codes; the exponent is $(d+1)^r$ for distance-$(2d+1)$ codes).

To achieve target error $\varepsilon$ in a circuit with volume $v$, you need $\eta_r \lesssim \varepsilon / v$, which requires:

$$r = O\left(\log \log \frac{v}{\varepsilon}\right)$$

levels. Each level multiplies the circuit size by a constant factor (the block size $m$ and procedure size), so $r$ levels give a $\text{polylog}(v/\varepsilon)$ blowup in total.

The resulting circuit $M_r$ has a hierarchical structure: large procedures at the top level correspond to original gates, smaller sub-procedures handle error correction at each level, with lower-level corrections applied more frequently.

## When to reach for it
- Whenever you need fault-tolerant quantum computation and the error rate is below threshold
- When you need arbitrarily low logical error rates from noisy physical gates
- When analysing the overhead of fault-tolerant quantum computation (resource estimation)

## Complexity
$O(n \cdot m^r)$ physical qubits per logical qubit, where $m$ is the block size and $r \sim \log\log(v/\varepsilon)$. Total gate count: $O(v \cdot \text{polylog}(v/\varepsilon))$. The polylogarithmic overhead is what makes fault tolerance *efficient*.

## Caveat
- The threshold $\eta_c$ depends sensitively on the code, the fault-tolerant procedures, the error model, and architectural constraints (connectivity, parallelism). Published values range from $\sim 10^{-6}$ (Aharonov-Ben-Or, not optimised) to $\sim 3\%$ ([[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes|Knill 2005]], with postselection).
- The constant factors in the polylog overhead can be large, especially at high concatenation levels. Practical fault tolerance often uses surface codes (with a fixed encoding, not recursive concatenation) for this reason.
- Requires the ability to input fresh qubits during computation to discard accumulated entropy. Without this, the overhead becomes exponential.
- Parallelism is required — sequential quantum computation (QTMs) cannot be made fault-tolerant this way.

## Related notes
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]]
- [[Error-Detecting Codes with Concatenation for Fault Tolerance]]
- [[Polynomial Codes from Secret Sharing]]
- [[Sparse Fault Path Analysis]]
