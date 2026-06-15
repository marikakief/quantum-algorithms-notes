# Positive-Coefficient Quadratic Forms for Constant-Spaced Infrastructure Sampling

> **Source:** Arthur Schmidt, arXiv:0912.4807
> **Tags:** #trick #number-theory #period-finding #quadratic-forms

## What it does

Reduces register size in real-quadratic infrastructure algorithms by sampling only reduced principal forms with positive first coefficient $a$, where adjacent sampled forms have constant lower-bounded distance.

## The trick

Indefinite binary quadratic forms of discriminant $\Delta$ have the shape
$$
(a,b,c),\qquad \Delta=b^2-4ac.
$$
For reduced forms, the reduction operator $\rho$ walks around the principal cycle. If all reduced forms are allowed, neighbouring forms can be separated by only $1/\sqrt{\Delta}$ in infrastructure distance. That forces fine discretisation and extra distance information.

Schmidt restricts to
$$
R^+=\{(a,b,c): (a,b,c)\text{ reduced, principal, and }a>0\}.
$$
The sign of $a$ alternates under the form version of the reduction walk, so moving from one positive-$a$ form to the next corresponds to two reduction steps. The distance gap then satisfies
$$
\delta(\rho^2(g))-\delta(g)>\ln 2.
$$
This spacing is independent of $\Delta$. Because each stored form in $R^+$ has $0<a,b\le \sqrt{\Delta}$ and $c$ is determined by $a,b,\Delta$, the function register can store just $a,b$ using about $\log\Delta+2$ qubits.

## When to reach for it

- Real-quadratic regulator and principal-ideal algorithms.
- Infrastructure computations where the full reduced-form cycle has tiny gaps, but a parity or sign-restricted subcycle has larger spacing.
- Resource estimates where the function-value register dominates the qubit count.

## Complexity

The restriction does not change polynomial-time computability. Giant steps still use form composition plus reduction; for positions $x<\Delta^2$, Schmidt bounds the number of square/multiply/reduction operations by $c\log_2\Delta$ for a constant $c<10$.

The payoff is a qubit saving of at least $2\log\Delta$ relative to Hallgren's form-plus-distance representation.

## Caveat

This is tailored to real quadratic infrastructure. The paper leaves open whether an analogous constant-spaced sampled subset exists for real-quadratic class groups or higher-degree number fields.

## Related notes

- [[Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009) — Paper Notes]]
- [[Infrastructure of Real Quadratic Fields as Period Domain]]
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]]
