# Secret Bias Smuggling for Lower Bounds

> **Source:** Aaronson, arXiv:0910.4698 (2010), §4.2
> **Tags:** #trick #lower-bound #circuit-complexity #AC0 #Fourier-analysis

## What it does

Proves classical lower bounds against simulating quantum sampling algorithms, by embedding a hard computational problem (like Majority) into the sampling distribution in a way that is undetectable to the simulator.

## The trick

Suppose a quantum algorithm samples strings $z$ with probability proportional to $\hat{f}(z)^2$ (the squared Fourier coefficients of a Boolean function $f$). To show no PH machine can simulate this:

1. **Secret bias:** Construct a distribution $D[s]$ over Boolean functions $f$, parameterised by a secret string $s$, such that:
   - $f$ looks statistically uniform to any algorithm seeing fewer than $\tilde{\Omega}(\sqrt{N})$ values
   - $\hat{f}(s)$ is slightly biased (by $O(1/\sqrt{N})$) toward being larger than average
   - $s$ is information-theoretically hidden from any efficient distinguisher

2. **Reduction:** If a PH machine $M$ simulates the quantum sampler, it outputs $z$ values correlated with large $|\hat{f}(z)|$. The secret bias means $M$ preferentially outputs $z = s$. But $\hat{f}(s) = \frac{1}{\sqrt{N}}\sum_x (-1)^{s \cdot x} f(x)$ — computing whether $|\hat{f}(s)|$ is large requires computing a weighted sum of $N$ bits, which is equivalent to the Majority function.

3. **Circuit lower bound:** By the [[PH-to-AC0 Correspondence for Oracle Separations|PH-to-AC$^0$ correspondence]], the PH machine becomes an AC$^0$ circuit. By Håstad's theorem, AC$^0$ circuits of depth $d$ computing Majority need size $\exp(\Omega(n^{1/(d-1)}))$. Contradiction.

The "smuggling" is the key innovation: the simulator can't avoid solving Majority because it doesn't know *which* Fourier coefficient was biased. Any attempt to simulate the quantum distribution must implicitly solve Majority for a random, hidden instance.

## When to reach for it

- Proving that a quantum sampling algorithm can't be simulated by PH or AC$^0$ circuits
- Any setting where you want to reduce a decision problem (like Majority) to a sampling/search problem
- When the quantum algorithm produces a distribution rather than a single answer, making direct reduction tricky
- Template: if computing any single output of the distribution is hard, and you can secretly bias which output is "interesting", you can smuggle the hard problem in

## Complexity

The bias $O(1/\sqrt{N})$ is chosen so that the biased distribution remains $O(1/\sqrt{N})$-close to uniform on any adaptive query set of size $o(\sqrt{N})$ — matching the Fourier coefficient's natural variance.

## Caveat

The technique requires that the biased distribution is sufficiently close to uniform that the simulator can't distinguish it from a fair random function. This limits the bias to $O(1/\sqrt{N})$, which means the advantage per trial is small. The lower bound works because even this small advantage, accumulated across the PH machine's operation, would violate Håstad's AC$^0$ bound.

## Related notes
- [[BQP and the Polynomial Hierarchy (Aaronson 2010) — Paper Notes]]
- [[PH-to-AC0 Correspondence for Oracle Separations]]
- [[Forrelation as Quantum-Classical Separator]]
- [[Forrelation — A Problem That Optimally Separates Quantum from Classical Computing (Aaronson-Ambainis 2015) — Paper Notes]]
