# Kaiser Window Amplitude Estimation

> **Source:** Berry, Su, Gyurik, King, Basso, Del Toro Barba, Rajput, Wiebe, Dunjko, Babbush, arXiv:2209.13581, PRX Quantum **5**, 010319 (2024)
> **Tags:** #trick #amplitude-estimation #Kaiser-window #fault-tolerant #signal-processing

## What it does

Estimates the amplitude $a$ of a marked subspace (as in Grover-style problems) to error $\epsilon$ with failure probability $\delta$, using approximately $\frac{\pi}{2\epsilon}\ln(1/\delta)$ oracle calls — roughly an order of magnitude fewer than Brassard, Høyer & Tapp (1998) at practical failure probabilities.

## The trick

Standard amplitude estimation uses QPE on the Grover iterate $G = -R_0 R_\chi$, which has eigenvalues $e^{\pm 2i\theta}$ where $\sin\theta = a$. The innovation is to replace the standard rectangular or Hanning window in the phase estimation step with a **Kaiser window** — a near-optimal window function from signal processing that minimizes sidelobe leakage for a given mainlobe width.

Given $U|0,0\rangle = a|\psi_0,0\rangle + \sqrt{1-a^2}|\psi_1,1\rangle$, the algorithm:

1. Runs a linear combination of $N$ powers of $G$ with Kaiser window weights.
2. The window function concentrates spectral weight around the true phase $\theta$, suppressing leakage from other eigenvalues.
3. Achieves $N = \frac{\pi}{\epsilon}\sqrt{1+\alpha^2}$ calls to $U$ or $U^\dagger$ for error $\epsilon$ at failure probability $\delta$, where $\alpha = \frac{1}{\pi}\ln(1/\delta) + O(\ln\ln(1/\delta))$.

**Key design choice:** Separate amplitude *estimation* (performed once) from amplitude *amplification* (performed many times). Standard approaches either use fixed-point amplification (log overhead per use) or repeat estimation every time. By estimating once and then running plain Grover amplification with the known step count, you pay the estimation overhead only once for the entire algorithm.

## When to reach for it

- Any algorithm with an amplitude amplification step where the amplified amplitude is initially unknown (e.g., searching for cliques, marked states, satisfying assignments).
- When you need amplitude estimation as a subroutine and care about constant factors — this gives ~10× improvement over Brassard, Høyer & Tapp at $\delta = 1/20$.
- In fault-tolerant settings where every oracle call costs thousands of Toffolis, so constant-factor improvements translate directly to runtime.

## Complexity

$N = \frac{\pi}{\epsilon}\sqrt{1 + \alpha^2}$ calls to $U/U^\dagger$, where $\alpha \approx \frac{1}{\pi}\ln(1/\delta)$.

For comparison, Brassard et al. achieve $\approx \pi/\epsilon$ calls but only for $1-\delta = 8/\pi^2 \approx 0.81$, requiring repetitions for smaller $\delta$. With 5 repetitions for $\delta = 1/20$, the Kaiser approach gives about an order of magnitude improvement.

## Caveat

The improvement is in constant factors, not asymptotics — both approaches are $O(1/\epsilon)$. The advantage matters most when the oracle cost is high (fault-tolerant settings). For cheap oracles or when you need only rough estimates, the overhead of implementing the window weights may not be worth it.

## Related notes
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]]
- [[Ground-State Recycling in Serial Amplitude Estimation]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
