# Quantum Counting for Linear-Cryptanalysis Bias Detection

> **Source:** Kaplan, Leurent, Leverrier, Naya-Plasencia, arXiv:1510.05836
> **Tags:** #trick #quantum-counting #linear-cryptanalysis #amplitude-estimation

## What it does

Replaces the $1/\epsilon^2$ sample count for detecting a linear-cryptanalysis bias by $1/\epsilon$ coherent queries in the Q2 model.

## The trick

A linear approximation gives masks $(\chi_P,\chi_C,\chi_K)$ and a constant $\chi_0$ such that

$$
\Pr\left[C[\chi_C]=P[\chi_P]\oplus K[\chi_K]\oplus\chi_0\right]=\frac{1+\epsilon}{2}.
$$

Classically, distinguishing a Bernoulli variable of mean $1/2$ from one of mean $1/2\pm\epsilon/2$ takes $\Theta(1/\epsilon^2)$ samples.

With Q2 access, implement the Boolean predicate

$$
F(P)=1\quad\Longleftrightarrow\quad E(P)[\chi_C]=P[\chi_P]\oplus b,
$$

where $b$ is the guessed key-dependent bit or constant term. Then apply [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes|quantum counting]] / amplitude estimation to estimate

$$
p=|F^{-1}(1)|/2^n
$$

to additive precision $\Theta(\epsilon)$. This costs $O(1/\epsilon)$ coherent queries.

For Matsui Algorithm 2, combine this with Grover search over the $2^{k_{\rm out}}$ partial last-round keys. The Q2 cost becomes

$$
T_{Q2}^{\rm Mat.2}=2^{k_{\rm out}/2}/\epsilon+2^{(k-k_{\rm out})/2}.
$$

## When to reach for it

- Linear or correlation attacks where the statistic is the bias of a Boolean predicate over plaintexts.
- Distinguishers that reduce to estimating a frequency to additive error $\epsilon$.
- Partial-key search where each candidate key is checked by bias estimation.

## Complexity

Q2 distinguishing cost: $O(1/\epsilon)$ queries. Q1 does not get this data saving; it still needs $O(1/\epsilon^2)$ classical samples before quantum post-processing.

## Caveat

The saving needs coherent oracle access to the keyed primitive. With only stored classical plaintext-ciphertext pairs, the data term remains classical unless one assumes a strong quantum memory model and still accepts the $1/\epsilon^2$ collected-data requirement.

## Related notes

- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
- [[Quantum Counting (Brassard-Høyer-Tapp 1998) — Paper Notes]]
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]]
- [[Q1-Q2 Adversary Split for Quantum Cryptanalysis]]
