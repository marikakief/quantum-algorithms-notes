# Q1-Q2 Adversary Split for Quantum Cryptanalysis

> **Source:** Kaplan, Leurent, Leverrier, Naya-Plasencia, arXiv:1510.05836
> **Tags:** #trick #quantum-cryptanalysis #symmetric-key #oracle-models

## What it does

Separates attacks that only use a quantum computer after classical data collection from attacks that require superposition access to the cryptographic primitive.

## The trick

When quantising a classical cryptanalytic attack, split the cost into:

1. **data collection:** calls to the keyed primitive $E_{\kappa^\ast}$;
2. **analysis:** post-processing, partial-key filtering, collision search, or final key search.

Then evaluate two models:

| Model | Query type | What can speed up |
|---|---|---|
| Q1 | Classical queries $x\mapsto E(x)$, then quantum post-processing | Only the analysis stage; the data term remains classical. |
| Q2 | Coherent queries $\sum_x\alpha_x|x\rangle\mapsto\sum_x\alpha_x|x,E(x)\rangle$ | Data collection and analysis can both be made quantum. |

This prevents a common mistake: replacing every classical loop by Grover search even when the loop ranges over observed classical data that had to be acquired first.

For a differential attack with classical data term $2^{h_S+1}$, Q1 keeps that term:

$$
D_{Q1}=2^{h_S+1},
$$

while Q2 may reduce it to

$$
D_{Q2}=2^{h_S/2+1}.
$$

## When to reach for it

- Any quantum cryptanalysis claim involving a real keyed primitive or hash function.
- Comparing black-box Grover key search with structural attacks.
- Deciding whether a result is a practical threat model statement or a stronger mathematical oracle-model statement.

## Complexity

No algorithmic overhead by itself. It is an accounting discipline: duplicate the attack analysis with Q1 and Q2 data terms separated.

## Caveat

Q1 may understate attacks if a real implementation accidentally permits coherent access. Q2 may overstate attacks if the primitive is only exposed through measurements or protocol layers that destroy coherence.

## Related notes

- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[A Note on Quantum Related-Key Attacks (Roetteler-Steinwandt 2013) — Paper Notes]]
- [[Applying Grover's Algorithm to AES Quantum Resource Estimates (Grassl-Langenberg-Roetteler-Steinwandt 2015) — Paper Notes]]
- [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]
