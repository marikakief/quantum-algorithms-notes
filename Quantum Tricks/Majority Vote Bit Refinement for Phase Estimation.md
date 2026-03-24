# Majority Vote Bit Refinement for Phase Estimation

> **Source:** Lanyon, Whitfield, Aspuru-Guzik, White et al., Nature Chemistry 2, 106 (2010); arXiv:0905.0887
> **Tags:** #trick #phase-estimation #error-mitigation #classical-postprocessing

## What it does
Reduces the bit error probability in iterative phase estimation exponentially, using only classical repetition and majority voting — no additional quantum resources per shot.

## The trick

The IPEA extracts the eigenphase one bit at a time. Each single-shot measurement of bit $\phi_k$ succeeds with probability $p > 1/2$ (with $p \geq 1 - 8/\pi^2 \approx 0.81$ in the worst case of intrinsic algorithmic error, higher when the binary expansion terminates exactly).

Repeat iteration $k$ independently $n$ times (each time re-preparing the input state and running the same circuit). Record all $n$ outcomes. Take the mode (most frequent value) as the estimated bit.

By a Chernoff bound, the probability that the majority vote returns the wrong bit is:

$$P_{\text{error}} \leq e^{-2n(p - 1/2)^2}$$

which decreases exponentially with $n$. For the worst-case $p \approx 0.81$, even $n = 31$ gives a bit error probability below $10^{-3}$.

The method composes across bits: if each of $m$ bits has error probability $\delta$, the probability that the full $m$-bit string is correct is $(1-\delta)^m$, which stays close to 1 for small $\delta$.

## When to reach for it

- Any iterative phase estimation implementation (IPEA, Bayesian PEA, etc.) where individual bit measurements have bounded error
- Near-term experiments where quantum error correction isn't available but shot budgets are cheap
- As a baseline comparison for more sophisticated error-mitigation strategies

## Complexity

- **Overhead:** $n$ repetitions per bit, $mn$ total for $m$-bit precision
- **Classical cost:** negligible (count and compare)
- **No quantum overhead:** each repetition is the same circuit, no ancillas or coherent error correction

## Caveat

Only works when the per-shot success probability $p > 1/2$ — otherwise the majority vote converges to the wrong answer. For large-scale circuits where gate errors accumulate across many sequential gates, $p$ can drop below 1/2, and majority voting breaks. In that regime, you need quantum error correction or fault-tolerant constructions.

Also, the method treats each bit independently. Correlated errors across bits (e.g., from systematic miscalibration) aren't addressed.

## Related notes
- [[Towards Quantum Chemistry on a Quantum Computer (Lanyon-Whitfield-Aspuru-Guzik-White 2010) — Paper Notes]]
- [[IPEA with Constant-Depth Controlled Powers]]
- [[Recursive Phase Estimation for Qubit-Efficient Readout]]
