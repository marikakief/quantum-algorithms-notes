# Postselection Removal via Approximate Success-Amplitude Amplification

> **Tags:** #trick #amplitude-amplification #postselection #OAA #fully-coherent #QSP
> **Source:** Martyn, Liu, Chin, Chuang (2023), Sec. II.D–E, Theorems 2–3
> **See also:** [[Oblivious Amplitude Amplification (Robust)]], [[Fully-Coherent One-Shot Hamiltonian Simulation]], [[Linear Combination of Unitaries (LCU)]]

## The problem

A QSP-LCU simulation circuit implements $e^{-iHt}$ via postselection on ancilla qubits. The success probability is approximately $1/4$ (or some known but imprecise value close to $1/2$ in amplitude). When this circuit is embedded inside a larger coherent algorithm, you cannot discard on failure — you need guaranteed (or high-probability) success without postselection.

Two strategies exist: standard amplitude amplification and robust oblivious amplitude amplification (ROAA). They differ in overhead and in how tightly $\delta$ is coupled to $\epsilon$.

## Strategy 1: Conventional amplitude amplification (Theorem 2)

Apply standard amplitude amplification. If the success amplitude is $\sin\theta \approx 1/2$ (success probability $\approx 1/4$), then $\ell = 1$ round of AA boosts to near-certainty. For general failure tolerance $\delta$, you need $\ell = O(\ln(1/\delta))$ rounds.

**Query complexity:**
$$\Theta\!\left(\ln(1/\delta)\left[\alpha|t| + \frac{\ln(1/\epsilon)}{\ln\!\bigl(e + \ln(1/\epsilon)/(\alpha|t|)\bigr)}\right]\right).$$

The $\ln(1/\delta)$ is multiplicative. If you want $\delta = 10^{-6}$, you pay roughly a 14× overhead on top of the simulation cost. Acceptable if $\delta$ is modest; painful if you're embedding the subroutine many times in a larger algorithm.

**When to use:** $\delta$ is moderate (say $\delta \ge \epsilon$), you already have an LCU circuit, and extending to ROAA would be more work than the overhead is worth.

## Strategy 2: Robust oblivious amplitude amplification (Theorem 3)

Use ROAA: the construction $A = -WRW^\dagger RW$ from [[Oblivious Amplitude Amplification (Robust)|Berry-Childs-Cleve-Kothari-Somma (2014)]]. ROAA is designed for settings where the success amplitude $\sin\theta$ is only **approximately known** — exactly the situation here (QSP approximation gives a polynomial close to but not exactly equal to $e^{-iHt}$, so the amplitude is approximately $1/2$).

In ROAA, the failure probability is controlled by the polynomial approximation error rather than requiring explicit boosting rounds. Consequently, $\delta = 2\epsilon$ automatically — the two precision parameters are linked.

**Query complexity:**
$$\Theta\!\left(\alpha|t| + \frac{\ln(1/\epsilon)}{\ln\!\bigl(e + \ln(1/\epsilon)/(\alpha|t|)\bigr)}\right).$$

No multiplicative $\ln(1/\delta)$ factor. Three queries to the simulation circuit instead of $O(\ln(1/\delta))$.

**When to use:** The natural setting for Theorem 3 — you're building a fully-coherent simulation subroutine, you're happy with $\delta = \Theta(\epsilon)$, and you don't need to control $\delta$ separately from $\epsilon$. The residual log-log correction on the precision may or may not matter depending on the regime.

## Head-to-head comparison

| | Conventional AA | Robust OAA | One-shot (Thm 5) |
|---|---|---|---|
| $\delta$ control | Independent, multiplicative overhead | Tied to $\epsilon$ ($\delta = 2\epsilon$) | Independent, additive $\ln(1/\delta)$ |
| Precision scaling | $O(\ln(1/\epsilon)/\ln\ln(1/\epsilon))$ | $O(\ln(1/\epsilon)/\ln\ln(1/\epsilon))$ | $O(\ln(1/\epsilon))$ |
| Overhead for small $\delta$ | $O(\ln(1/\delta))\times$ | None | $+\ln(1/\delta)$ additive |
| AA rounds | $O(\ln(1/\delta))$ | 3 (single OAA step) | 0 |
| Existing LCU reuse | ✓ | ✓ | ✗ (different construction) |

If you have an existing LCU circuit and need coherence, ROAA is almost always better than conventional AA. If you're building from scratch, the one-shot approach (see [[Fully-Coherent One-Shot Hamiltonian Simulation]]) is better than both — it avoids amplitude amplification entirely and gets cleaner precision scaling.

## The 1/4 success probability issue

In the QSP-LCU construction, the LCU circuit combines $\cos(Ht)$ and $-i\sin(Ht)$ using a two-ancilla register. In the ideal (exact polynomial) case, the success amplitude in each branch is $1/2$, so overall success probability is $1/4$. (This is the $1/4$ sweet spot where a single OAA round is exact, per the [[Oblivious Amplitude Amplification (Robust)|OAA construction]].)

However, the QSP approximation is not exact — the polynomial approximates $\cos$ and $\sin$ to error $\epsilon$. The actual success amplitude is therefore only approximately $1/2$. Standard AA (which requires knowing the amplitude exactly) becomes unreliable; ROAA is built precisely for this approximately-known amplitude regime.

## Robust OAA construction

Given approximately-unitary $\tilde{W}$ with $\|\tilde{W} - W_r\| \le \delta$ where $W_r$ is the ideal LCU circuit, define:
$$A = -\tilde{W} R \tilde{W}^\dagger R \tilde{W},$$
where $R = 2\Pi_0 - I$ is the reflection about the $|0\rangle$ ancilla subspace.

Then $\Pi_0 A |0\rangle|\psi\rangle \approx |0\rangle e^{-iHt}|\psi\rangle$ with error $O(\delta)$ (linear in the approximation error). Three queries to $\tilde{W}$ (or its inverse) suffice.

## Related paper notes

- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]]
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — clearest proof of robust OAA in simulation context
- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — where ROAA was introduced
