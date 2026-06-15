# Pairwise-Independent Edge-Flipping to Repair Modular Cycle Tests

> **Source:** Cade--Montanaro--Belovs, arXiv:1610.00581
> **Tags:** #trick #randomisation #limited-independence #cycle-detection #space-efficient

## What it does

Repairs a modular cycle test using only $O(\log n)$ random bits. Full random orientation is unnecessary.

## The trick

The mod-3 lift misses a cycle through $k$ when its orientation imbalance is $0\bmod3$. Colour vertices with a pairwise-independent hash function
$$
h:[n]\to\{0,1\}.
$$
Flip the orientation of every edge incident to $k$ whose other endpoint has colour 1.

If the cycle neighbours of $k$ are $a,b$, then pairwise independence gives
$$
\Pr[h(a)\ne h(b)] = 1/2.
$$
So with probability $1/2$, exactly one of the two incident cycle edges flips, changing the imbalance modulo 3 and making the lift detect the cycle.

## Why it matters

The randomness can be stored and evaluated in logarithmic space. This is the point: the algorithm cannot afford to store a fully random orientation of the graph.

## When to use it

- Modular obstruction repairs in log-space algorithms.
- Randomising local structure around a guessed witness vertex.
- Making a worst-case orientation argument work with small random seed.

## Related notes

- [[Mod-k Lift from Cycle Detection to st-Connectivity]]
- [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]]
