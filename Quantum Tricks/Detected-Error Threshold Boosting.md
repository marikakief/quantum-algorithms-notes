# Detected-Error Threshold Boosting

> **Source:** E. Knill, R. Laflamme, G. J. Milburn, arXiv:quant-ph/0006088; see also E. Knill, arXiv:quant-ph/0410199
> **Tags:** #trick #fault-tolerance #erasure #error-correction #threshold #KLM

## What it does

When gate failures are always detected (the gate either succeeds or announces that it failed), the fault-tolerance threshold is dramatically higher than for undetected errors — typically by one to two orders of magnitude.

## The trick

Standard fault-tolerance theory assumes worst-case errors: an unknown unitary rotation corrupts a qubit, and you don't know it happened until a syndrome measurement reveals it. The accuracy threshold for this is $\sim 10^{-4}$ (or optimistically $\sim 10^{-2}$ with optimised codes).

But some physical implementations produce **detected failures** (erasures): the gate either works perfectly or fails in a known way (e.g., a photon is lost, or a teleportation measurement gives the "wrong" outcome). In this case:
1. You **know which qubits are affected** — no need to search for the error location
2. You only need the error-correcting code to handle erasures at known positions, not arbitrary errors at unknown positions
3. The erasure threshold is roughly the **code distance divided by the block length**, which is much higher than the general threshold

For quantum erasure codes, the threshold is estimated at $\sim 50\%$ erasure rate per gate for some code families. KLM estimates a practical detected-error threshold $T_d \lesssim 0.96$ (i.e., requiring success rate $\geq 4\%$), achieved with teleportation parameter $n \leq 25$.

[[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes|Knill (2005)]] pushed this further: using $C_4/C_6$ error-detecting codes with postselection, the effective threshold reaches $\sim 3\%$ general error rate — a two-order-of-magnitude improvement enabled by making errors detectable.

## When to reach for it

- When your hardware has a dominant failure mode that is heralded (photon loss, teleportation failure, failed distillation)
- When designing fault-tolerant architectures — converting undetected errors to detected errors (via error detection + postselection) can dramatically reduce overhead
- When estimating resource costs for LOQC, ion trap schemes with heralded entanglement, or any platform where "I don't know if it worked" can be replaced with "I know it didn't work"

## Complexity

The overhead from error correction is polynomial in $1/(T_d - p_{\text{fail}})$ where $p_{\text{fail}}$ is the actual failure rate. For detected errors close to the erasure threshold, the polynomial is low-degree.

Contrast with undetected errors: overhead is polynomial in $1/(p_{\text{thresh}} - p_{\text{error}})$ with a much smaller $p_{\text{thresh}}$.

## Caveat

Requires that failure detection is itself reliable. If the "failure flag" is wrong (false positive or false negative), the advantage degrades. In particular, photon loss in the detection apparatus can turn a detected error into an undetected one, collapsing the threshold back to the general case.

Also, converting all errors to detected errors can be expensive. KLM achieves this naturally (teleportation always reveals success/failure), but other architectures may need additional verification steps.

## Related notes
- [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes]]
- [[Quantum Computing with Realistically Noisy Devices (Knill 2005) — Paper Notes]]
- [[Fault-Tolerant Quantum Computation with Constant Error Rate (Aharonov-Ben-Or 2008) — Paper Notes]]
- [[Offline Resource State Preparation for Gate Teleportation]]
- [[Measurement-Induced Nonlinearity]]
