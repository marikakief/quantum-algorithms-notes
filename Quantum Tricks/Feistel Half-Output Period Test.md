# Feistel Half-Output Period Test

> **Source:** Santoli--Schaffner, arXiv:1603.07856
> **Tags:** #trick #Simon #Feistel #quantum-cryptanalysis #oracle-distinguishing

## What it does

Turns the first half of a three-round Feistel oracle into a Simon-style period test that distinguishes Feistel structure from a random permutation.

## The trick

For a three-round Feistel network on $2n$ bits with round functions $P_1,P_2,P_3$, the first output half is

$$
W(a\|c)=c\oplus P_2(a\oplus P_1(c)).
$$

Fix two distinct right halves $\alpha,\beta\in\{0,1\}^n$ and define

$$
f(b\|a)=
\begin{cases}
W(a\|\alpha)\oplus\alpha, & b=0,\\
W(a\|\beta)\oplus\beta, & b=1.
\end{cases}
$$

The added $\alpha$ or $\beta$ cancels the visible branch value, leaving

$$
f(0\|a)=P_2(a\oplus P_1(\alpha)),\qquad
f(1\|a)=P_2(a\oplus P_1(\beta)).
$$

Therefore

$$
f(u)=f(u\oplus s),
\qquad
s=1\|\big(P_1(\alpha)\oplus P_1(\beta)\big).
$$

Run Simon sampling on $f$, solve for the candidate $s$, and test the equality $f(u)=f(u\oplus s)$ on a fresh random $u$. For a random permutation, the same construction should not hide a global XOR period, so the verification catches the false candidate except with negligible probability.

## When to reach for it

- A Feistel-like construction exposes one output half whose formula has a visible XOR branch term plus a round-function term.
- You can fix two branch constants and cancel the visible terms to align both branches through a shared internal function.
- The goal is a Q2 distinguisher rather than classical cryptanalysis.

## Complexity

One coherent evaluation of $f$ uses at most two coherent queries to the Feistel oracle $V$. The distinguisher uses $O(n)$ Simon samples, polynomial-time linear algebra over $\mathbb F_2$, and one final period check.

## Caveat

This is tailored to three rounds. More Feistel rounds bury the useful aligned expression, and the paper does not prove insecurity for a larger constant number of rounds.

## Related notes

- [[Using Simon's Algorithm to Attack Symmetric-Key Cryptographic Primitives (Santoli-Schaffner 2017) — Paper Notes]]
- [[Weak-Period Simon Sampling for Distinguishers]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]
- [[Quantum Differential and Linear Cryptanalysis (Kaplan-Leurent-Leverrier-Naya-Plasencia 2017) — Paper Notes]]
