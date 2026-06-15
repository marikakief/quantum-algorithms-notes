# Quadratic Speedup for Classical Heuristics via Amplitude Amplification

> **Source:** Brassard, Høyer & Tapp, arXiv:quant-ph/9805082
> **Tags:** #trick #amplitude-amplification #heuristics #speedup

## What it does

Turns a coherently implementable classical randomized **search heuristic** with expected runtime $T$ and a checkable success predicate into a quantum search procedure with expected runtime $O(\sqrt{T})$, with reversible implementation costs included.

## The trick

Suppose a classical heuristic $G$ for search problem $F$ works by sampling a random seed $r \in R$ and outputting $G(F, r)$, which satisfies $F(G(F,r)) = 1$ with probability $h_F/|R|$. The expected classical time to find a solution is $|R|/h_F$.

Quantum version: treat $G$ as a quantum algorithm $A$ that maps $|0\rangle \to \frac{1}{\sqrt{|R|}}\sum_r |r\rangle|G(F,r)\rangle$. The success probability is $a = h_F/|R|$. Apply [[Standard Amplitude Amplification|amplitude amplification]] to boost this, finding a solution in $O(1/\sqrt{a}) = O(\sqrt{|R|/h_F})$ applications of $A$.

For a distribution over problem instances, the expected quantum time is:

$$
\sum_F \sqrt{|R|/h_F} \cdot P_F \leq \left(\sum_F \frac{|R|}{h_F} P_F\right)^{1/2} = \sqrt{T}
$$

by Cauchy-Schwarz.

## When to reach for it

- Speeding up bounded-time, coherently seeded versions of classical randomized search algorithms such as SAT restarts, local search, or simulated annealing restarts.
- Any setting where you have a checkable-output search heuristic and want the black-box amplitude-amplification speedup under an explicit reversible cost model.
- As a theoretical tool for proving quantum speedup results for broad algorithm families.

## Complexity

$O(\sqrt{T})$ applications of the reversible version of the heuristic and its inverse.

## Caveat

The heuristic must be implementable as a reversible quantum circuit with coherent random seeds and a clean success predicate. Classical heuristics with complex control flow may need significant restructuring. Las Vegas or variable-time algorithms generally need variable-time amplitude amplification or explicit runtime bucketing. The reversible implementation overhead can eat into the $\sqrt{T}$ improvement for practical instances.

Also, the $\sqrt{T}$ bound is over the *average* over the problem distribution — worst-case speedup for a specific instance is $\sqrt{|R|/h_F}$, which may be better or worse.

## Related notes
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Standard Amplitude Amplification]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
