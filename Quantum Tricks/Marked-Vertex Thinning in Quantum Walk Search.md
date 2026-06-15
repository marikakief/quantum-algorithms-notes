# Marked-Vertex Thinning in Quantum Walk Search

> **Source:** Chailloux--Loyer, arXiv:2105.05608
> **Tags:** #trick #quantum-walks #algorithm-design #lattice-sieving

## What it does

Reduces quantum-walk setup and update costs by marking only a controlled fraction of the available solutions.

## The trick

In a quantum walk search problem, it is tempting to mark every vertex that contains a solution. Chailloux--Loyer instead shrink the auxiliary structure used to witness markedness.

In their sieve walk, a full inner code size
$$
|C_2|=N^{\rho_0+c_1-c_2}
$$
makes a close residual pair collide in some inner filter with constant probability. Replace this by a smaller code
$$
|C_2|=N^{\rho+c_1-c_2},\qquad 0<\rho\leq\rho_0.
$$
Now only a fraction
$$
N^{\rho-\rho_0}
$$
of otherwise-good pairs are witnessed as marked. This decreases the marked fraction $\varepsilon$, but it also lowers the setup cost from $N^{c_1+\rho_0}$ to $N^{c_1+\rho}$ and lowers update costs.

The win appears when the list has enough solutions that thinning still leaves many marked events. In Chailloux--Loyer's notation, if each outer bucket has $N^\zeta$ reducing pairs, the safe regime is
$$
\zeta+\rho-\rho_0>0.
$$
Then one can repeat over fresh inner codes and still extract $\Theta(N^\zeta)$ solutions while paying the smaller per-walk cost.

## When to reach for it

Use it when:

- marking every solution is expensive;
- solutions are numerous, not isolated;
- a random witness structure can expose a tunable fraction of them;
- the saved setup/update cost outweighs the $1/\sqrt\varepsilon$ penalty.

This is a good mental check for quantum-walk designs: sometimes the right marked set is not the largest one.

## Complexity

For Chailloux--Loyer's final SVP sieve optimum,
$$
\rho\to0,
\qquad c\approx0.3696,
\qquad c_1\approx0.2384,
\qquad c_2=0,
$$
which gives
$$
T=N^{1.2384+o(1)}=2^{0.2570d+o(d)}.
$$
Without thinning, their best walk construction gives $2^{0.2605d+o(d)}$.

## Caveat

Thinning is dangerous when solutions are sparse. If $\zeta+\rho-\rho_0\leq0$, the inner code may expose too few marked vertices, and the repeated-walk strategy loses the intended advantage.

## Related notes

- [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]]
- [[Local-Filter Johnson Walk for Bucket Pair Finding]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Generalised Grover Lemma for Quantum Walks]]
