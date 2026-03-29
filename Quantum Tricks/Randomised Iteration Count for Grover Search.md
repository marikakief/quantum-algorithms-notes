# Randomised Iteration Count for Grover Search

> **Source:** Boyer, Brassard, Høyer & Tapp, arXiv:quant-ph/9605034
> **Tags:** #trick #search #grover #unknown-solutions

## What it does

Finds a marked item with $O(\sqrt{N/t})$ queries even when the number of solutions $t$ is unknown.

## The trick

Grover's algorithm needs $\approx \frac{\pi}{4}\sqrt{N/t}$ iterations for near-certain success, but if $t$ is unknown, you can't set this. Worse, running for the wrong number of iterations can *reduce* the success probability to near zero (overshooting).

The fix: randomise the iteration count.

1. Set $m = 1$, $\lambda = 6/5$.
2. Choose $j$ uniformly at random from $\{0, 1, \ldots, m-1\}$.
3. Run $j$ Grover iterations, measure. If solution found, stop.
4. Otherwise, $m \leftarrow \min(\lambda m, \sqrt{N})$. Go to 2.

The key lemma: if $j \sim \text{Uniform}\{0, \ldots, m-1\}$, the average success probability is

$$
P_m = \frac{1}{2} - \frac{\sin(4m\theta)}{4m\sin(2\theta)} \geq \frac{1}{4} \quad \text{when } m \geq \frac{1}{\sin(2\theta)}
$$

The randomisation over $j$ averages over the oscillating $\sin^2((2j+1)\theta)$, converting the problematic non-monotone behaviour into a guaranteed constant success floor.

The geometric growth of $m$ by factor $\lambda$ ensures you hit the critical threshold $m_0 \approx \sqrt{N/t}$ after $O(\log\sqrt{N/t})$ rounds, and the total work across all rounds forms a convergent geometric series.

## When to reach for it

- Any application of [[Standard Amplitude Amplification|amplitude amplification]] where the success probability $a$ (and hence the required iteration count) is unknown.
- Quantum search subroutines inside larger algorithms where the number of marked items isn't known in advance.
- Anytime you'd want to use Grover but don't know how many iterations to run.

## Complexity

$O(\sqrt{N/t})$ expected queries — the same as the known-$t$ case, with a constant overhead factor $\leq 9/2$.

## Caveat

The constant factor is worse than the known-$t$ case. If you have even a rough estimate of $t$, use it directly rather than this generic schedule. For the case where you need the *count* rather than just finding an element, see [[Amplitude Estimation via Phase Estimation on Q|amplitude estimation]].

Any $\lambda \in (1, 4/3)$ works. The paper uses $6/5$; the choice affects the constant but not the asymptotics.

## Related notes
- [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
