
> **Tags:** #trick #QSP #QET #block-encoding #parity #spectrum #affine-transform
> **Source:** Martyn, Liu, Chin, Chuang (2023), Sec. III.A.1, Lemma 4, Figs. 4–5
> **See also:** [[Block-Encoding Composition Algebra]], [[Fully-Coherent One-Shot Hamiltonian Simulation]], [[QSVT Meta-Template]]

## The problem it solves

QSP/QET requires the target polynomial to have **definite parity** (all-even or all-odd). The complex exponential $e^{-ix\tau} = \cos(x\tau) - i\sin(x\tau)$ on $x \in [-1,1]$ mixes even and odd parts and cannot be directly approximated by a QSP sequence — this is the parity obstruction.

You cannot simply approximate $e^{-ix\tau}$ globally on $[-1,1]$ with a QSP polynomial. But you can sidestep this if the spectrum of interest lives only on a **positive subinterval** away from zero.

## The trick

Given a block-encoding of $H/\alpha$ with spectrum in $[-1,1]$, construct a block-encoding of the affinely transformed operator:
$$\tilde{H} = \frac{1}{2}\!\left(I + \frac{\beta H}{\alpha}\right), \quad \beta \in (0,1),$$
which has spectrum in $\left[\frac{1-\beta}{2},\, \frac{1+\beta}{2}\right] \subset (0, 1)$.

This interval is **bounded away from zero** (and from $-1$). On it, $\mathrm{sign}(x) = +1$ everywhere.

## Why this helps

On the compressed interval, the function
$$\mathrm{EECE}(x;\tau) = \cos(\tau x) - i\sin(\tau x)\,\mathrm{sign}(x)$$
equals $e^{-ix\tau}$ exactly. And $\mathrm{EECE}(x;\tau)$ is an **even function** (both $\cos(\tau x)$ and $\sin(\tau x)\,\mathrm{sign}(x)$ are even in $x$), so it has definite parity — QSP can approximate it without constraint.

Result: approximate $\mathrm{EECE}(x;\tau)$ by a polynomial on $[(1-\beta)/2,(1+\beta)/2]$, apply QET, and you get $e^{-iHt}$ acting on the system register. No parity obstruction. No amplitude amplification.

## Circuit construction

**Step 1 — Block-encoding of $\beta H/\alpha$:**

Use the [[Block-Encoding Composition Algebra|block-encoding composition lemma]] (Lemma 4 in the paper):
$$\text{if } V_A \text{ block-encodes } A,\ V_B \text{ block-encodes } B,\ \text{then } (I_B \otimes V_A)(V_B \otimes I_A) \text{ block-encodes } AB.$$
Scale the block-encoding of $H/\alpha$ by $\beta$ by composing with an appropriate diagonal (or using amplitude-scaled oracles). This gives a circuit for Fig. 4 in the paper.

**Step 2 — Shift by $I/2$:**

Block-encode $I/2$ via an ancilla rotation ($|0\rangle \to |+\rangle$ gives the identity with normalization $1$; combined with an extra ancilla qubit, $\frac{1}{2}I$ is encodable). This adds the constant shift.

**Step 3 — Combine via LCU:**

Use one ancilla qubit and LCU to sum $\beta H/\alpha$ and $I$ with equal weight, producing $\tilde{H} = \frac{1}{2}(I + \beta H/\alpha)$ (Fig. 5 in the paper).

Total overhead: a constant number of ancilla qubits and queries to $H/\alpha$.

## Parameter choices

- $\beta$ controls the compression: smaller $\beta$ → narrower interval → polynomial approximation on a smaller domain → potentially lower degree needed for a given approximation accuracy. But too small $\beta$ means the time parameter $\tau$ must be rescaled and the effective query complexity may not improve.
- For [[Hamiltonian simulation]] $e^{-iHt}$, set $\tau = \alpha t / \beta$ (so that $e^{-i\tilde{H}\tau}$ recovers $e^{-iHt}$ on the compressed spectrum). This keeps the query count $\Theta(\alpha|t|)$.
- Optimal $\beta$ is typically close to 1.

## Approximation degree

Approximating $e^{-ix\tau}$ on the compressed interval $[(1-\beta)/2,(1+\beta)/2]$ to error $\epsilon$ requires polynomial degree $\Theta(\alpha|t| + \ln(1/\epsilon))$, matching the query complexity in Theorem 5.

## When to use

- Any time you want to apply QET to a function that is parity-incompatible on $[-1,1]$ but parity-compatible on a restricted subinterval.
- Building fully-coherent simulation subroutines that will be concatenated inside larger algorithms.
- Avoiding amplitude amplification entirely when the sign problem on $[-1,1]$ would otherwise prevent QSP from working.

## Caveat

The shift adds a constant-overhead circuit. The compression factor $\beta$ should be chosen to match the actual spectral spread of $H$ if known — if $\|H\| \ll \alpha$ (the block-encoding normalization is loose), you can compress more aggressively and potentially reduce the effective $\tau$.

## Related paper notes

- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]]
- [[Grand Unification of Quantum Algorithms (Martyn-Rossi-Tan-Chuang 2021) — Paper Notes]] — QET framework
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — block-encoding framework
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSP parity constraints discussed in detail
