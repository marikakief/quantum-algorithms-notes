# Zero-Divisor Density Heuristic for Medium Prime Factors

> **Source:** Cheng, arXiv:math/0301179
> **Tags:** #trick #analytic-number-theory #density-heuristic #primality-proving

## What it does

Estimates the density of integers with a prime factor in a medium interval $[b,cb]$ by counting zero-divisors modulo a primorial-like product.

## The trick

Let

$$
m=\prod_{b\le p\le cb}p.
$$

An integer $t\in\{1,\ldots,m\}$ has a prime factor in $[b,cb]$ exactly when it is a zero-divisor in $\mathbb Z/m\mathbb Z$. Therefore the density of such integers modulo $m$ is

$$
1-\frac{\varphi(m)}m
=1-\prod_{b\le p\le cb}\left(1-\frac1p\right).
$$

Using Mertens' product estimate,

$$
\prod_{p<x}\left(1-\frac1p\right)
=\frac{e^{-\gamma}}{\ln x}\left(1+O\left(\frac1{\ln x}\right)\right),
$$

the product over $[b,cb]$ is controlled by the ratio of the products below $cb$ and below $b$. For a suitable absolute constant $c$ and large $b$, Cheng derives

$$
1-\prod_{b\le p\le cb}\left(1-\frac1p\right)>\frac1{\ln b}.
$$

This gives a clean back-of-the-envelope density estimate for medium prime factors.

## When to reach for it

- You need a quick model for how often $n-1$ has a prime factor in a prescribed logarithmic window.
- A subroutine works only when a medium-sized prime factor exists, and you need to estimate expected trials.
- You want a modular counting argument before making a stronger prime-distribution heuristic.

## Complexity

The argument is analytic, not an algorithmic subroutine. If used as a search heuristic, the expected number of samples is the inverse of the estimated density, about $O(\ln b)$ in Cheng's setting.

## Caveat

This counts all integers modulo $m$, not primes in short intervals. Cheng still needs an additional conjecture that good primes have comparable density in intervals of width about $4\sqrt n$.

## Related notes

- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]
- [[Medium-Prime-Factor Targeting by One ECPP Round]]
