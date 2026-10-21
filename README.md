# CHAT — Inverse Collatz Trees 

**Theorem.** For every prime \(p\), every positive integer \(w\) with \(3 \nmid w\), every residue \(r \pmod{p}\), and every bound \(B\), there exists an integer \(n > B\) such that \(n\) reaches \(w\) under the Collatz map and \(n \equiv r \pmod{p}\).

In particular the set of such \(n\) is infinite.  
For \(p \ge 5\) a stronger “Full Dials” statement holds inside an explicitly constructed two-branch inverse skeleton: every valid 3-adic class at every depth realises every residue class modulo \(p\).

This is a local non-obstruction theorem. It does **not** prove the Collatz conjecture, does not give natural-density statements, and does not supply effective size bounds.

## Status

| Component | Status |
|-----------|--------|
| Lean 4 / Mathlib formalization | Complete, zero `sorry` (authors’ compilation) |
| Main theorem (`main_theorem_infinite`) | Proved for every prime |
| Full Dials (`main_theorem_core`) | Proved for \(p \ge 5\) |
| Small primes \(p=2,3\) | Handled by explicit constructions |
| Frozen-height reformulation, composite moduli, left-only generation, spectral gap | Outside the formal development |
