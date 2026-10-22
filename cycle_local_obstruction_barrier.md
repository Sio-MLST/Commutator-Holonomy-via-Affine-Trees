#cycle_local_obstruction_barrier

## 1. Context

A hypothetical Collatz cycle with $k$ odd steps and $S$ total steps satisfies the integer equation:

$$(2^S - 3^k) x_0 = N$$

where $x_0$ is the minimum element of the cycle and $N = \sum_{i=0}^{k-1} 3^{k-1-i} 2^{a_i}$.

A natural question is whether this equation can be ruled out locally: that is, whether there exists a prime $p$ such that no integer in the backward Collatz tree can satisfy $(2^S - 3^k) x \equiv N \pmod p$.

---

## 2. Statement of the Result

In `Every-Remainder-Everywhere.lean`, the theorem `main_theorem_infinite` establishes that for any target $w$ coprime to 3, any prime $p$, any residue $r \in \mathbb{Z}/p\mathbb{Z}$, and any bound $B$:

$$\exists n > B, \quad \text{Reaches } w \, n \quad \wedge \quad n \equiv r \pmod p$$

As a direct corollary, `cycle_local_obstruction_barrier` states:

```lean
theorem cycle_local_obstruction_barrier (spec : CycleSpec)
    (h_denom : ((2 ^ spec.S - 3 ^ spec.k : ℤ) : ZMod p) ≠ 0)
    {v : ℕ} (hv : 0 < v) (hv3 : v % 3 ≠ 0) :
    ∃ (n : ℕ), Reaches v n ∧
      ((2 ^ spec.S - 3 ^ spec.k : ℤ) : ZMod p) * (n : ZMod p) = (spec.N : ZMod p)
```

### Mechanism

For $p \ge 5$, the inverse branches $R$ and $L$ act on residue classes modulo powers of 3 as affine maps $t \mapsto a t + b$. The holonomy of closed walks based at residue 8 contains:
1. A non-trivial dilation $(\lambda, 0)$ with $\lambda \not\equiv 0, 1 \pmod p$.
2. A non-trivial translation $(1, d)$ with $d \not\equiv 0 \pmod p$.

Over the field $\mathbb{Z}/p\mathbb{Z}$, these two elements generate the full affine group $\text{Aff}(\mathbb{Z}/p\mathbb{Z})$. The action on $\mathbb{Z}/p\mathbb{Z}$ is therefore transitive, forcing every valid residue fiber to be full. The cases $p = 2$ and $p = 3$ are settled directly by explicit powers of two.

---

## 3. Scope

### What is ruled out

* **Single-prime local obstructions for $p \nmid (2^S - 3^k)$:** 
  One cannot rule out a candidate cycle $(S, k, N)$ by choosing a prime $p$ coprime to the denominator and showing that elements reaching a seed $v$ fail to satisfy the cycle equation modulo $p$. In $\mathbb{Z}/p\mathbb{Z}$, $(2^S - 3^k)$ is invertible, yielding a unique target residue $r^* = (2^S - 3^k)^{-1} N$. Because the predecessor tree surjects onto $\mathbb{Z}/p\mathbb{Z}$, witnesses $n \equiv r^* \pmod p$ reaching $v$ always exist.

### What is not ruled out

* **Primes dividing the denominator ($p \mid 2^S - 3^k$):** 
  If $p \mid (2^S - 3^k)$ and $p \nmid N$, then $(2^S - 3^k)x \equiv N \pmod p$ reduces to $0 \equiv N \not\equiv 0$, which is an immediate obstruction. This is simply the condition that $(2^S - 3^k) \mid N$ in $\mathbb{Z}$.
* **Dynamical path consistency:** 
  The witness $n$ reaches $v$ and satisfies the congruence at the base point, but its forward orbit is not guaranteed to follow the specific intermediate parity sequence that defines $N$.
* **Archimedean / size bounds:** 
  The result is purely local (modular). It has no bearing on methods based on linear forms in logarithms (Baker's method), which rule out cycles using lower bounds on $|2^S - 3^k|$ over $\mathbb{R}$.
