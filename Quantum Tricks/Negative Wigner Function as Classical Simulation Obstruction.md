# Negative Wigner Function as Classical Simulation Obstruction

> **Source:** Feynman, Int. J. Theor. Phys. **21**(6/7):467–488, 1982
> **Tags:** #trick #classical-simulation #Wigner-function #quasi-probability #foundational

## What it does

Identifies the negativity of the Wigner function (or more generally, quasi-probability representations) as the structural reason why quantum mechanics resists efficient classical probabilistic simulation.

## The trick

Reformulate quantum states using the Wigner function $W(x, p) = \int \rho(x+y, x-y) e^{ipy} \, dy$. This function behaves almost like a probability distribution: it's real, normalised, and its marginals give the correct quantum probabilities:

$$\int W(x,p)\,dp = |\psi(x)|^2, \qquad \int W(x,p)\,dx = |\tilde{\psi}(p)|^2$$

If $W$ were everywhere non-negative, a classical probabilistic computer could sample from it directly — simulate the quantum system by sampling from $W$ just as a Monte Carlo method samples from a classical probability distribution.

But $W$ can go negative. For discrete (spin-1/2) systems, the analogous quasi-probability $f_{s_1 s_2}$ can have negative entries while all physical observables (sums of these entries) remain non-negative. A classical probabilistic computer can only sample from non-negative distributions. The negativity is precisely what blocks classical simulation.

This isn't just a mathematical inconvenience: Bell inequality violations prove that no local hidden-variable (i.e., classical probabilistic) model can reproduce quantum statistics. The negative quasi-probabilities and Bell violations are two faces of the same obstruction.

## When to reach for it

- Assessing whether a quantum computation is classically simulable (states with non-negative Wigner functions — "stabiliser states" in the discrete case — *are* classically simulable; negativity is what makes a state computationally powerful)
- Resource theories of quantum computation: Wigner negativity quantifies the "non-classicality" and can bound the classical simulation cost
- Understanding where quantum speedups come from — contextuality and Wigner negativity provide a necessary condition for quantum advantage

## Complexity

The cost of classical simulation scales with the total Wigner negativity of the computation. For stabiliser circuits (non-negative discrete Wigner function), classical simulation is efficient — this is the Gottesman-Knill theorem. Injecting magic states introduces negativity and makes simulation hard.

## Caveat

Wigner negativity is necessary but not sufficient for quantum advantage. Some states with negative Wigner functions can still be efficiently classically simulated if the negativity is structured in the right way. The full picture involves contextuality, magic, and entanglement working together.

## Related notes
- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes]]
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
- [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]
