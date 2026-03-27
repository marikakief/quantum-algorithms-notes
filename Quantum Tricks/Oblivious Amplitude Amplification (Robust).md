# Oblivious Amplitude Amplification (OAA / Robust)

> **Tags:** #trick #amplitude-amplification #LCU #fundamental #QSVT-precursor
> **Source:** Berry, Childs, Cleve, Kothari, Somma. arXiv:1312.1414 (STOC 2014); simplified in arXiv:1412.4687
> **See also:** [[Linear Combination of Unitaries (LCU)]]

## What it does
Deterministically boosts the success probability of [[Linear Combination of Unitaries (LCU)|LCU]] from $\sin^2(\theta)$ to $\sim 1$, for **any** input state simultaneously ("oblivious" to the system register).

## The trick
Given unitary $U$ on $(\mu + n)$ qubits such that for any $|\psi\rangle$:
$$U|0^\mu\rangle|\psi\rangle = \sin\theta\,|0^\mu\rangle V|\psi\rangle + \cos\theta\,|\Phi^\perp\rangle$$

Define $R = 2\Pi - \mathbb{1}$ where $\Pi = |0^\mu\rangle\langle 0^\mu| \otimes \mathbb{1}$, and $S = -URU^\dagger R$. Then:

$$S^\ell U|0^\mu\rangle|\psi\rangle = \sin((2\ell+1)\theta)|0^\mu\rangle V|\psi\rangle + \cos((2\ell+1)\theta)|\Phi^\perp\rangle$$

**Why "oblivious":** $R$ reflects about the **ancilla** being $|0^\mu\rangle$, not about the input state $|0^\mu\rangle|\psi\rangle$. Works for any $|\psi\rangle$ without knowing it.

**The $p = 1/4$ sweet spot:** In the standard application, $\sin^2\theta = 1/4$ ($\theta = \pi/6$), so a single round ($\ell = 1$) gives $\sin(3\pi/6) = 1$ — **exact** amplification with 3 queries.

## The proof: 2D Subspace Lemma

The key insight is that the amplification stays in a 2D subspace despite not reflecting about $|\psi\rangle$.

Define the operator $Q = (\langle 0^\mu| \otimes \mathbb{1}) U^\dagger \Pi U (|0^\mu\rangle \otimes \mathbb{1})$. For any $|\psi\rangle$:
$$\langle\psi|Q|\psi\rangle = \|\Pi U|0^\mu\rangle|\psi\rangle\|^2 = \sin^2\theta$$

Since this holds for **every** $|\psi\rangle$, we must have $Q = \sin^2\theta \cdot \mathbb{1}$.

This forces the "orthogonal partner" $|\Psi^\perp\rangle$ (defined by $U|\Psi^\perp\rangle = \cos\theta|0^\mu\rangle V|\psi\rangle - \sin\theta|\Phi^\perp\rangle$) to satisfy $\Pi|\Psi^\perp\rangle = 0$.

So $|0^\mu\rangle|\psi\rangle$ lives entirely in the $\Pi = 1$ subspace and $|\Psi^\perp\rangle$ lives entirely in the $\Pi = 0$ subspace. The reflection $R$ acts as $+1$ on one and $-1$ on the other — exactly the structure needed for standard amplitude amplification. The operator $S$ is a rotation by $2\theta$ in this 2D subspace.

## Connection to Chebyshev polynomials and QSVT

$S^\ell U$ implements $\sin\theta \to \sin((2\ell+1)\theta)$, which is the Chebyshev polynomial $U_{2\ell}(\cos\theta) \cdot \sin\theta$ applied to the singular values of $\langle 0^\mu|U|0^\mu\rangle$. OAA is an **early instance of [[QSVT Meta-Template|quantum singular value transformation]]** — compositions of $U$, $U^\dagger$, and the reflection $R$ implement polynomial transformations of singular values.

## Robust version (arXiv:1412.4687)

When $\tilde{U}$ is only approximately unitary ($\|\tilde{U} - U_r\| = O(\delta)$):
$$A = -WRW^\dagger RW \quad\Rightarrow\quad \|PA|0\rangle|\psi\rangle - |0\rangle U_r|\psi\rangle\| = O(\delta)$$

Error is **linear** in $\delta$. Derived by expanding $PA = 3PW/s - 4PWPW^\dagger PW/s^3$ and using $\tilde{U}\tilde{U}^\dagger \approx \mathbb{1}$.

## Engineering $p = 1/4$

If the natural success probability $p > 1/4$, introduce an extra ancilla qubit and a rotation $\Upsilon: |0\rangle \to \frac{1}{2\sqrt{p}}|0\rangle + \sqrt{1 - \frac{1}{4p}}|1\rangle$ to dilute the amplitude to exactly $1/2$. This is the "Segment Lemma" trick.

## When to reach for it
- After any [[Linear Combination of Unitaries (LCU)|LCU]] where $s \approx 2$ (choose segment length to ensure this)
- [[Hamiltonian simulation]] via truncated Taylor series
- Any [[Block-Encoding Composition Algebra|block-encoding]] construction that needs deterministic success
- When the target operation is only approximately unitary (truncation, discretisation)
- As a conceptual stepping stone to [[QSVT Meta-Template|QSVT]]

## Complexity
$(2\ell + 1) \times$ the cost of a single $U$ application. For $\ell = 1$: $3\times$ cost. General $\ell$: $O(1/\sqrt{p})$ rounds for success probability $p$.

## Caveat
For exact single-round amplification, need $\sin^2\theta = 1/4$ exactly. For general $p$, choose $\ell$ closest to $\pi/(4\theta) - 1/2$ — residual error $\cos((2\ell+1)\theta) \neq 0$ unless $\theta$ divides $\pi/2$ evenly.

## Related Paper Notes

- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — where OAA was introduced (Lemma 3.6, 2D Subspace Lemma)
- [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]] — cleanest proof of the robust version (Eq. 14)
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Efficient Fully-Coherent Quantum Signal Processing Algorithms for Real-Time Dynamics Simulation (Martyn-Liu-Chin-Chuang 2023) — Paper Notes]] — uses ROAA (Theorem 3) as one route to fully-coherent simulation; also introduces a one-shot approach that avoids AA entirely
- [[Postselection Removal via Approximate Success-Amplitude Amplification]] — direct comparison of conventional AA, ROAA, and one-shot in this context
