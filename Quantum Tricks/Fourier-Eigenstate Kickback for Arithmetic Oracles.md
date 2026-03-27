
> **Source:** Jordan, PRL 95, 050501 (2005); same mechanism in [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes|Shor (1994)]], [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes|Kitaev (1995)]]
> **Tags:** #trick #phase-kickback #oracle #arithmetic #QFT

## What it does
Converts an oracle that computes a function via modular addition into a phase oracle, by preparing the output register in a Fourier basis eigenstate of the addition operator.

## The trick

An oracle that adds $f(x)$ modulo $N_o$ to the output register:

$$U_f |x\rangle |a\rangle = |x\rangle |a + f(x) \bmod N_o\rangle$$

The Fourier basis states $|u_k\rangle = \frac{1}{\sqrt{N_o}} \sum_{a=0}^{N_o-1} e^{i2\pi a k/N_o} |a\rangle$ are eigenstates of addition modulo $N_o$:

$$U_f |x\rangle |u_k\rangle = e^{i2\pi k f(x)/N_o} |x\rangle |u_k\rangle$$

The output register is unchanged; the function value kicks back as a **phase**. For $k=1$:

$$|x\rangle |u_1\rangle \xrightarrow{U_f} e^{i2\pi f(x)/N_o} |x\rangle |u_1\rangle$$

To prepare $|u_1\rangle$: write 1 into the output register, then apply the inverse QFT.

This generalises [[Phase Kickback from Oracle Queries|Boolean phase kickback]] ($|{-}\rangle$ is the $k=1$ Fourier eigenstate of addition mod 2) to real-valued functions encoded in fixed-point arithmetic.

## When to reach for it
- Any oracle that computes a real-valued function via modular addition (gradient estimation, function evaluation, numerical algorithms)
- Understanding why Shor's algorithm works: modular exponentiation adds to a phase register in exactly this way
- Converting "compute and store" oracles into "compute as phase" oracles for interference-based algorithms

## Complexity
Zero overhead beyond preparing $|u_1\rangle$ (one value-write + inverse QFT on the output register).

## Caveat
The phase is $2\pi f(x)/N_o$ — the function value must fit in $N_o$ levels of precision. If $f$ isn't integer-valued, the modular addition introduces rounding error. The phase precision must be high enough for the subsequent QFT to resolve the quantity of interest (gradient components, eigenvalues, etc.).

## Related notes
- [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]]
- [[Phase Kickback from Oracle Queries]] — the Boolean special case
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]] — modular exponentiation kickback
- [[Gapped Phase Estimation]] — kickback from controlled-$U$ is the same mechanism
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]] — major application: Coulomb potential evaluation via this trick, halving Zalka-Wiesner gate count
