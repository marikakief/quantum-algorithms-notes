# Frequency-Domain Spectral Function via QPE Importance Sampling

> **Source:** Wecker, Hastings, Wiebe, Clark, Nayak, Troyer, arXiv:1506.05135
> **Tags:** #trick #measurement #phase-estimation #spectral-function #correlation-function

## What it does
Measures dynamic structure factors $S_A(\omega)$ directly in the frequency domain, bypassing the need to sample $C_A(t)$ at many time points and Fourier transform.

## The trick
The spectral function decomposes as:

$$S_A(\omega) = \sum_n |\langle \psi_n | A | \psi_0 \rangle|^2 \,\delta(\omega - (E_n - E_0))$$

Instead of computing $C_A(t) = \langle \psi_0 | A^\dagger(t) A(0) | \psi_0 \rangle$ for many $t$ and Fourier transforming:

1. Prepare the state $A|\psi_0\rangle$ (apply operator $A$ to the ground state)
2. Run [[Iterative Phase Estimation (Kitaev)|quantum phase estimation]] on this state
3. QPE projects onto eigenstate $|\psi_n\rangle$ with probability $|\langle \psi_n | A | \psi_0 \rangle|^2$
4. The measured energy $E_n$ directly samples from $S_A(\omega)$

A histogram of QPE outcomes is the spectral function. This is "importance sampling" — each shot automatically draws from the spectral weight distribution. Delta-function peaks in $S_A(\omega)$ are resolved by the QPE precision, not by a time-domain Fourier window.

The retarded correlation function $\chi_A(\omega)$ is then recovered via the fluctuation-dissipation theorem ($\chi_A''(\omega) = (1 - e^{-\beta\omega})S_A(\omega)$ at $T=0$) and Kramers-Kronig relations.

## When to reach for it
- Measuring spectral functions (ARPES-like observables, dynamic spin structure factor, charge excitation spectra)
- When the spectral function has sharp features (quasiparticle peaks, gap edges) that would be smeared by finite-time Fourier transforms
- When the physics question is "what are the excitation energies and their weights?" rather than "what is $C(t)$?"

## Complexity
- $O(1/\Delta E)$ Trotter steps per QPE shot, where $\Delta E$ is the desired energy resolution
- Number of shots: $O(1/p_{\min})$ to resolve a peak with spectral weight $p_{\min} = |\langle \psi_n | A | \psi_0 \rangle|^2$
- Preparing $A|\psi_0\rangle$: depends on $A$. For local operators ($c^\dagger_{k,\sigma}$, $S^z_q$), this is $O(1)$–$O(N)$ additional gates

## Caveat
Requires the ability to prepare $A|\psi_0\rangle$ coherently, which may be non-trivial for non-unitary $A$. For Hermitian $A$, one can use [[Linear Combination of Unitaries (LCU)|LCU]] to implement $A$ as a linear combination, but this adds overhead. Also, if the spectral weight is spread over many eigenstates, many QPE shots are needed to build a reliable histogram.

## Related notes
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Non-Destructive Ground-State Measurement via Recovery Map]]
