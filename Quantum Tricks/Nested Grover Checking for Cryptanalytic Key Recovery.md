# Nested Grover Checking for Cryptanalytic Key Recovery

> **Source:** Kaplan, Leurent, Leverrier, Naya-Plasencia, arXiv:1510.05836
> **Tags:** #trick #Grover #quantum-cryptanalysis #key-recovery

## What it does

Turns a last-round cryptanalytic attack into a Grover search whose checking routine performs partial-key generation and final key completion.

## The trick

Many last-round attacks have this shape:

1. find a data item or pair that survives a structural filter;
2. derive a list of partial last-round key candidates;
3. complete each partial key by searching the remaining key bits.

Instead of materialising the filtered list, define the Grover search space as the filtered data domain and make the checking routine quantum.

For a simple differential attack, search over

$$
X=\{x:E(x\oplus\delta_{\rm in})\oplus E(x)\in D_{\rm fin}\}.
$$

A candidate $x$ is marked if it back-propagates to the target difference after $R$ rounds and leads to the real full key. Checking consists of:

1. generate compatible $k_{\rm out}$-bit partial keys at quantum cost $C^\ast_{k_{\rm out}}$;
2. Grover-search over the remaining key bits, costing $2^{(k-h_{\rm out})/2}$ in the Kaplan--Leurent--Leverrier--Naya-Plasencia parameterisation.

The resulting Q2 simple-differential last-round cost is

$$
T_{Q2}^{\rm s.att}=2^{h_S/2+1}+2^{(h_S+\Delta_{\rm fin}-n)/2}\left(C^\ast_{k_{\rm out}}+2^{(k-h_{\rm out})/2}\right).
$$

In Q1, replace the first term by the classical data term $2^{h_S+1}$.

## When to reach for it

- Differential or linear last-round attacks with a partial-key filtering stage.
- Attacks where the classical algorithm says "build a list, filter it, then complete keys" but the list does not need to be stored coherently.
- Security comparisons against generic Grover key search $2^{k/2}$.

## Complexity

The outer Grover factor is the inverse square root of the surviving good-pair fraction. The checking cost is whatever the partial-key generator plus final key search costs. The technique saves the square root of the key-completion term, but in Q1 it does not save data collection.

## Caveat

The checking procedure must be reversible and must mark candidates coherently. If the classical attack relies on large classical tables, measurements, or adaptive pruning, the conversion may need qRAM-style assumptions or a different walk.

## Related notes

- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
