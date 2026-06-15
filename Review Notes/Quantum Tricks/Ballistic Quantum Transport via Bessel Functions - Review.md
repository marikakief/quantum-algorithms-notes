# Review: Ballistic Quantum Transport via Bessel Functions

Source note: [[Ballistic Quantum Transport via Bessel Functions]]

## Primary Sources Checked

- Childs, Farhi, Gutmann, arXiv:quant-ph/0103020: https://arxiv.org/abs/quant-ph/0103020

## Verdict

Minor issues. The card is useful, but it should distinguish ordinary stationary-phase behavior from the Bessel/Airy transition at the ballistic front.

## Line-Anchored Comments

- Lines 12-16: propagator formula is convention-dependent but essentially right.

  The global phase and sign depend on whether the hopping entries are written with plus or minus sign. The probability distribution and ballistic speed are unchanged.

- Lines 20-23: near-front amplitude is oversimplified.

  For a continuous-time walk on the line, the leading-edge Bessel asymptotics are not simply the same as ordinary `t^{-1/2}` stationary phase. Away from the caustic, `t^{-1/2}` is the right scale; at the turning point, Airy-type asymptotics appear. Avoid using one exponent for all "near-front" behavior.

- Line 25: finite-line approximation is good.

  Add the caveat that boundary reflections and the center defect affect detailed amplitudes; the `O(n)` transport conclusion survives but the infinite-line formula is not exact on the finite glued-trees reduction.

- Line 35: "O(n) traversal time" should specify probability.

  The wavepacket reaches the opposite side in linear time in the propagation sense. The algorithmic 2003 result uses time-averaging and repetition to obtain the exit vertex with high probability.

## Missing or Suggested Additions

- Add a one-sentence comparison with classical continuous-time random walk on a line: variance grows linearly in time, so distance grows diffusively as `sqrt(t)`.
