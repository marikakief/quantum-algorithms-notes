# QMAM Reduction to Single-Coin SDP

> **Source:** Marriott & Watrous, arXiv:cs/0506068; exploited by Jain, Ji, Upadhyay & Watrous, arXiv:0907.4737
> **Tags:** #trick #QIP #complexity #SDP #Arthur-Merlin #interactive-proofs

## What it does
Reformulates any quantum interactive proof as a semidefinite program where the verifier's coin register is 2-dimensional, enabling structural simplifications (particularly a Pauli twirl bound) that make the SDP solvable in PSPACE.

## The trick
The [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes|Marriott-Watrous]] result $\mathrm{QIP} = \mathrm{QMAM}$ shows that any QIP protocol can be converted to a 3-message Arthur-Merlin game where Arthur sends a single random bit. This means the verifier's acceptance operator takes the form:

$$Q = \frac{1}{2}|0\rangle\langle 0| \otimes P_0 + \frac{1}{2}|1\rangle\langle 1| \otimes P_1$$

on $\mathcal{X} \otimes \mathcal{W} \otimes \mathcal{Y}$, where $\mathcal{X} = \mathbb{C}^2$ is the coin register and $P_0, P_1$ are the two measurement operators.

The 2-dimensionality of $\mathcal{X}$ enables a Pauli twirl identity: for any $P \in \mathrm{Pos}(\mathcal{X} \otimes \mathcal{Z})$ with $\dim(\mathcal{X}) = 2$,

$$P \leq 2 \cdot \mathbf{1}_\mathcal{X} \otimes \mathrm{Tr}_\mathcal{X}(P)$$

This follows from $\sum_{\sigma \in \{I, X, Y, Z\}} (\sigma \otimes I) P (\sigma \otimes I) = 2 \cdot \mathbf{1}_\mathcal{X} \otimes \mathrm{Tr}_\mathcal{X}(P)$. The bound lets you replace 2D block structure with a scalar trace, which is what makes the MMW convergence analysis go through.

Concretely: in the $\mathrm{QIP}(2)$ case (Jain-Upadhyay-Watrous 2009), the coin register $\mathcal{X}$ has arbitrary polynomial dimension, and the SDP is harder. By going through QMAM instead of attacking QIP(3) directly, you trade a harder SDP for an easier one.

## When to reach for it
- Any situation where you need to formulate a quantum interactive proof as an SDP and want the simplest possible structure.
- When working with matrix games where one player has a 2-dimensional "type" space — the Pauli twirl bound is the key simplification.
- As a reminder that the right reformulation of a problem can be more important than a better algorithm for the wrong formulation. The Marriott-Watrous QMAM characterisation made the $\mathrm{QIP} = \mathrm{PSPACE}$ proof clean; without it, the SDP would be much harder.

## Complexity
The reformulation itself is efficient (polynomial-time reduction). The resulting SDP has the same exponential size as the original QIP formulation, but the structural simplification (2D coin register) is what enables the [[Matrix Multiplicative Weights Update Method for SDPs|MMW method]] to solve it in PSPACE.

## Caveat
Only applies when you can reformulate the interactive proof as a single-coin Arthur-Merlin game. This works for QIP but may not generalise to multi-prover settings or other interaction patterns. The technique is also specific to quantum Arthur-Merlin games — classical Arthur-Merlin games don't have the same SDP structure.

## Related notes
- [[QIP = PSPACE (Jain-Ji-Upadhyay-Watrous 2011) — Paper Notes]]
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
- [[Matrix Multiplicative Weights Update Method for SDPs]]
- [[Jordan's Lemma Subspace Decomposition for Alternating Measurements]]
