# Injective Group-Action Hidden Shift from Principal Homogeneous Spaces

> **Source:** Childs--Jao--Soukharev, arXiv:1012.4019
> **Tags:** #trick #hidden-shift #group-actions #isogenies

## What it does

Turns a free transitive group action and two orbit elements into an injective hidden shift instance.

## The trick

Let a finite abelian group $G$ act freely and transitively on a set $X$. Given $x_0,x_1\in X$, suppose there is a unique $s\in G$ with
$$
s*x_0=x_1.
$$
Define
$$
f_0(g)=g*x_0,
\qquad
f_1(g)=g*x_1.
$$
Then
$$
f_1(g)=g*(s*x_0)=(gs)*x_0=f_0(gs),
$$
so $f_0,f_1$ hide the shift $s$.

The free action gives injectivity: if $f_0(g)=f_0(g')$, then $g*x_0=g'*x_0$, hence $g=g'$.

In [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]], the group is $\operatorname{Cl}(\mathcal O_\Delta)$ and the set is $\operatorname{Ell}_{q,n}(\mathcal O_\Delta)$ with the isogeny star action.

## When to reach for it

- Class-group actions, isogeny graphs, or other principal homogeneous spaces.
- Any cryptanalytic setting where public keys are two orbit points and the secret is the group element mapping one to the other.
- Reductions to [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes|Kuperberg-style hidden shift]] where injectivity is needed for subexponential query complexity.

## Complexity

The reduction itself is query-preserving. The true cost is evaluating $g*x$ coherently for arbitrary $g\in G$ and $x\in X$.

For ordinary elliptic-curve isogenies, Childs--Jao--Soukharev evaluate the action in
$$
L_q\!\left(\frac12,\frac{\sqrt3}{2}\right)
$$
time under GRH.

## Caveat

Injectivity depends on freeness. If the action has stabilisers, $f_0$ need not be injective and the hidden shift problem can become much harder. Also, this reduction does not help unless arbitrary group-action evaluation is faster than brute-force orbit search.

## Related notes

- [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]]
- [[GRH Expander Smoothing for Class-Group Actions]]
- [[Polynomial-Space Abelian Hidden Shift Sieve by Component Isolation]]
- [[Quantum Sieve for Labelled Qubits]]
- [[A Subexponential-Time Quantum Algorithm for the Dihedral Hidden Subgroup Problem (Kuperberg 2005) — Paper Notes]]
