# Trace Trick for QMA-to-PP Reduction

> **Source:** Marriott & Watrous, arXiv:cs/0506068
> **Tags:** #trick #QMA #PP #counting-complexity #trace

## What it does
Converts a "maximum eigenvalue" promise problem (QMA) into a "sum of eigenvalues" problem (trace) that counting complexity classes can handle, by making the error exponentially small relative to the witness dimension.

## The trick
The QMA acceptance operator $Q_x$ on $m$ qubits has eigenvalues in $[0,1]$, and the QMA promise says:
- $x \in L$: max eigenvalue $\geq a$
- $x \notin L$: max eigenvalue $\leq b$

Computing the maximum eigenvalue is hard (it's what QMA captures). But the **trace** — the sum of all $2^m$ eigenvalues — is much easier for counting complexity.

Apply [[Jordan's Lemma Subspace Decomposition for Alternating Measurements|strong error reduction]] to push $a$ up to $1 - 2^{-(m+2)}$ and $b$ down to $2^{-(m+2)}$ without increasing $m$. Now:

- $x \in L \Rightarrow \mathrm{tr}(Q_x) \geq 1 - 2^{-(m+2)} \geq 3/4$
- $x \notin L \Rightarrow \mathrm{tr}(Q_x) \leq 2^m \cdot 2^{-(m+2)} \leq 1/4$

The NO bound uses the fact that all $2^m$ eigenvalues are $\leq 2^{-(m+2)}$, so their sum is $\leq 2^m \cdot 2^{-(m+2)} = 1/4$.

The trace $\mathrm{tr}(Q_x)$ can be computed as a GapP function via the Fortnow-Rogers method (exponential sum of polynomial products of matrix entries), so $\mathrm{QMA} \subseteq \mathrm{PP}$.

## When to reach for it
- Proving containment of quantum complexity classes in counting classes (PP, A₀PP)
- Any situation where you need to relate a "worst-case" (max eigenvalue) promise to a "sum" (trace) that's easier to compute
- Classical simulation of quantum verification: the trace replaces the need to optimize over all possible witnesses

## Complexity
The strong error reduction step multiplies the circuit size by $O(\mathrm{poly}(m, r))$. The GapP computation of the trace involves an exponential sum but this is fine for PP (which can handle GapP).

## Caveat
This only gives $\mathrm{QMA} \subseteq \mathrm{PP}$, not a tighter containment. The trace trick is lossy — it doesn't distinguish between one large eigenvalue and many medium eigenvalues — but for PP the gap of $3/4$ vs $1/4$ is enough. For tighter containments (like $\mathrm{QMA} \subseteq \mathrm{A_0PP}$), a minor modification suffices.

## Related notes
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
- [[Jordan's Lemma Subspace Decomposition for Alternating Measurements]]
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
