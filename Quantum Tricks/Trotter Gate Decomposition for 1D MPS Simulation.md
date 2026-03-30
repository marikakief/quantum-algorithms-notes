# Trotter Gate Decomposition for 1D MPS Simulation

> **Source:** Vidal, arXiv:quant-ph/0310089
> **Tags:** #trick #tensor-networks #MPS #TEBD #Trotter #classical-simulation

## What it does
Converts continuous-time evolution under a nearest-neighbour 1D Hamiltonian into a sequence of strictly local two-body gates, each applicable via [[Local Gate Update on MPS via Schmidt Redecomposition|MPS update]] with no SWAP overhead.

## The trick
Given a nearest-neighbour Hamiltonian $H_n = \sum_l K_1^{[l]} + \sum_l K_2^{[l,l+1]}$, split into two layers:

$$F = \sum_{\text{even } l} F^{[l]}, \qquad G = \sum_{\text{odd } l} G^{[l]}$$

where $F^{[l]}$ contains the single-body and two-body terms centred on bond $l$. All terms within each layer commute (they act on disjoint pairs). So each layer exponentiates into a product of independent two-body gates:

$$e^{-iF\delta} = \prod_{\text{even } l} e^{-iF^{[l]}\delta}, \qquad e^{-iG\delta} = \prod_{\text{odd } l} e^{-iG^{[l]}\delta}$$

A second-order Trotter step is $e^{-iF\delta/2} e^{-iG\delta} e^{-iF\delta/2}$, consisting of $O(n)$ two-body gates, each acting on adjacent sites. Each gate updates only the two affected MPS tensors and the bond between them — cost $O(d^3 \chi^3)$ per gate.

The total cost per Trotter step is $O(n d^3 \chi^3)$, and for $T/\delta$ steps the total is $O(n d^3 \chi^3 T/\delta)$.

## When to reach for it
- Any time you're simulating a 1D system with nearest-neighbour interactions using MPS (i.e., TEBD, tDMRG, or variants)
- Extends to imaginary time evolution ($\delta \to -i\delta$) for ground-state preparation
- The same even/odd layer decomposition is used on quantum computers for Trotter simulation of 1D Hamiltonians (see [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes|Lloyd (1996)]])

## Complexity
$O(nd^3\chi^3)$ per Trotter step. Order-$p$ Trotter with total error $\epsilon$ requires $O(T^{1+1/p}/\epsilon^{1/(2p)})$ steps.

## Caveat
Only works directly for nearest-neighbour interactions. Longer-range interactions require either SWAP-based routing (adding $O(n)$ overhead per gate) or more sophisticated decompositions (e.g., MPO-based methods). Also, the Trotter error is controlled but accumulates — for long-time simulations you need small $\delta$ or higher-order formulas.

## Related notes
- [[Efficient Simulation of One-Dimensional Quantum Many-Body Systems (Vidal 2004) — Paper Notes]]
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[Local Gate Update on MPS via Schmidt Redecomposition]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
