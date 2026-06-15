# Difference-Set Correlation for Hidden Shifts

## Pattern

If a known Boolean function is the characteristic function of a difference set, its Fourier magnitudes are controlled. A hidden shift of that function becomes a linear phase in Fourier space:

$$\widehat{f_s}(\chi)=\chi(s)\widehat f(\chi).$$

If the phases of $\widehat f(\chi)$ can be corrected efficiently, an inverse Fourier transform concentrates the state on $s$.

## How to use it

1. Start with a known subset $D\subseteq A$ whose characteristic function has flat or two-level Fourier power spectrum.
2. Query the shifted function $1_D(x-s)$.
3. Apply the abelian QFT.
4. Apply the known diagonal correction from the unshifted spectrum.
5. Invert the QFT and measure the shift.

## Why it matters

This gives efficient hidden-shift algorithms for structured, non-injective functions. It is not a worst-case dihedral-HSP method; it is a way to build easy white-box instances with strong algebraic design.

## Source paper

- [[Quantum Algorithms for Abelian Difference Sets and Applications to Dihedral Hidden Subgroups (Roetteler 2016) — Paper Notes|Roetteler (2016)]].
