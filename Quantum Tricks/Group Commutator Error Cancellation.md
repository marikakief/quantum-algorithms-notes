
> **Source:** Dawson & Nielsen, arXiv:quant-ph/0505030, Lemma 1
> **Tags:** #trick #error-cancellation #commutator #gate-compilation

## What it does

Shows that when computing a group commutator $VWV^\dagger W^\dagger$ from approximate unitaries $\tilde{V} \approx V$ and $\tilde{W} \approx W$, the first-order errors cancel. The commutator is approximated to accuracy $O(\Delta\delta)$ where $\Delta$ is the approximation error and $\delta$ is the distance of $V, W$ from the identity.

## The mechanism

Write $\tilde{V} = V + \Delta_V$. In the product $\tilde{V}\tilde{W}\tilde{V}^\dagger\tilde{W}^\dagger$, the first-order error terms come in pairs:

$$
\Delta_V W V^\dagger W^\dagger + V W \Delta_V^\dagger W^\dagger
$$

By unitarity of $V$ and $V + \Delta_V$: $\Delta_V V^\dagger + V\Delta_V^\dagger = -\Delta_V \Delta_V^\dagger$. So the pair sums to second order in $\Delta$.

**Precise bound (Lemma 1):** If $d(V, \tilde{V}), d(W, \tilde{W}) < \Delta$ and $d(I, V), d(I, W) < \delta$:

$$
d(VWV^\dagger W^\dagger,\; \tilde{V}\tilde{W}\tilde{V}^\dagger\tilde{W}^\dagger) < 8\Delta\delta + 4\Delta\delta^2 + 8\Delta^2 + 4\Delta^3 + \Delta^4
$$

The leading term $8\Delta\delta$ gives the $\epsilon^{3/2}$ improvement in [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev]] when $\Delta = \epsilon_{n-1}$ and $\delta = O(\sqrt{\epsilon_{n-1}})$.

## When to reach for it

- The [[Solovay-Kitaev Recursive Gate Compilation|Solovay-Kitaev algorithm]] (the original and primary use)
- Any iterative refinement scheme where you can express the correction as a group commutator
- Understanding why the SK algorithm's accuracy improves super-linearly with recursion depth

## Caveat

The cancellation only helps when $\delta < \Delta$ (i.e., the commutator factors are closer to identity than the approximation accuracy). If $V, W$ are far from identity, the commutator offers no advantage over direct approximation.

## Related notes

- [[The Solovay-Kitaev Algorithm (Dawson-Nielsen 2005) — Paper Notes]]
- [[Solovay-Kitaev Recursive Gate Compilation]]
- [[Balanced Group Commutator Decomposition]]
