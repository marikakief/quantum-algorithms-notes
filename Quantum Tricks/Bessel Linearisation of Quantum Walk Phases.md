
> **Source:** Berry, Childs, Kothari. arXiv:1501.01715 (FOCS 2015)
> **Tags:** #trick #hamiltonian-simulation #quantum-walk #bessel-functions #LCU

## What it does

Converts the nonlinear eigenphases $\pm e^{\pm i\arcsin(\nu)}$ of a Szegedy quantum walk into the desired linear phases $e^{i\nu z}$ for [[Hamiltonian simulation]], using a [[Linear Combination of Unitaries (LCU)|linear combination]] of walk steps with Bessel function coefficients.

## The trick

The Szegedy walk operator $U$ for a $d$-sparse Hamiltonian has eigenvalues $\mu_\pm = \pm e^{\pm i\arcsin(\nu)}$ where $\nu = \lambda/Xd$ and $\lambda$ is an eigenvalue of $H$.

The Bessel generating function identity:

$$\sum_{m=-\infty}^{\infty} J_m(z)\,\mu_\pm^m = \exp\!\left[\frac{z}{2}\left(\mu_\pm - \frac{1}{\mu_\pm}\right)\right] = e^{i\nu z}$$

gives exactly the desired Hamiltonian evolution phase, and — this is the key point — is the **same** for both $\mu_+$ and $\mu_-$. No need to distinguish eigenspaces.

Apply:

$$V_k = \sum_{m=-k}^{k} a_m U^m, \qquad a_m = \frac{J_m(z)}{\sum_j J_j(z)}$$

via [[Linear Combination of Unitaries (LCU)|LCU]] (PREPARE the normalised Bessel coefficients, SELECT via controlled-$U^m$). Boost with [[Oblivious Amplitude Amplification (Robust)|OAA]].

## When to reach for it

- [[Hamiltonian simulation]] from a walk operator when you want $\log(1/\varepsilon)$ precision *and* linear-in-$d$ sparsity scaling
- Any situation where you have a unitary with eigenvalues $e^{\pm i\arcsin(\nu)}$ and want to implement $e^{i\nu z}$
- When phase estimation is too expensive (gives $O(1/\sqrt{\varepsilon})$ error) and self-inverse decomposition is too wasteful (costs an extra factor of $d$)

## Complexity

Take $z = O(1)$: each segment uses $k = O(\log(\tau/\varepsilon)/\log\log(\tau/\varepsilon))$ walk steps. Number of segments: $O(\tau)$. Total: $O(\tau k)$.

Bessel functions decay as $|J_m(z)| \sim |z/2|^m / m!$ for $m \gg |z|$ — super-exponential, matching the Taylor series decay that drives the $\log(1/\varepsilon)$ precision scaling.

## Caveat

The Bessel coefficients must be computed classically to $O(\log(\tau/\varepsilon))$ bits of precision. Efficient via Miller's backward recurrence, but adds classical preprocessing cost. Also, the walk construction itself requires $O(n)$ gates per step, so the gate complexity is $O(\tau n k)$.

Superseded in practice by [[QSVT Meta-Template|QSP/QSVT]], which achieves optimal polynomial transformations of walk eigenvalues without the Bessel detour.

## Related notes

- [[Hamiltonian Simulation with Nearly Optimal Dependence on All Parameters (Berry-Childs-Kothari 2015) — Paper Notes]] — the source paper
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]] — the walk construction this linearises
- [[Linear Combination of Unitaries (LCU)]] — the implementation framework
- [[Oblivious Amplitude Amplification (Robust)]] — used to make each segment deterministic
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]] — reformulated via QSP
