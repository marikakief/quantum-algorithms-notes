# Adjacency-Array Quantum Walk via Neighbour-State Amplification

> **Source:** Cade--Montanaro--Belovs, arXiv:1610.00581; Belovs electric-network quantum walks
> **Tags:** #trick #quantum-walks #adjacency-array #amplitude-amplification #space-efficient

## What it does

Implements local quantum-walk reflections in the adjacency-array model without storing neighbour lists.

## The trick

Given neighbour access
$$
f_v:[d_v]\to[n],
$$
prepare
$$
|\psi_v\rangle=\frac1{\sqrt{d_v}}\sum_{i\in[d_v]} |i\rangle|f_v(i)\rangle.
$$
Projecting the first register onto
$$
|+_v\rangle=\frac1{\sqrt{d_v}}\sum_i |i\rangle
$$
would leave the neighbour state
$$
|\phi_v\rangle=\frac1{\sqrt{d_v}}\sum_i |f_v(i)\rangle
$$
with probability $1/d_v$. Use exact amplitude amplification to make this deterministic in $O(\sqrt{d_v})$ queries.

For a superposition over vertices, run each vertex-dependent amplification routine up to the worst-case degree $d_m$ and idle after completion. This gives $O(\sqrt{d_m})$ queries per walk step.

## When to use it

- Quantum walks on graphs given by neighbour arrays.
- Space-efficient algorithms where materialising adjacency lists is impossible.
- Implementing reflections about local neighbour superpositions.

## Caveat

The price for log-space is the $\sqrt{d_m}$ factor. In dense graphs this can erase the query-efficiency advantage.

## Related notes

- [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]]
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]]
- [[Standard Amplitude Amplification]]
