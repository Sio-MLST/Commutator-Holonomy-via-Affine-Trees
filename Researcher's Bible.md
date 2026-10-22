# Researcher's Bible

### A condensed proof you can hold in your head, a map of the surrounding country, and an honest list of where the dragons live

*For a reader who already knows what Collatz is, what 3-adic digits are, and what a commutator does, and who wants the structure of the proof without the scaffolding.*

**Status of this document.** This is the single authoritative project document. The earlier "working edition" of the Bible is retired: what was worth keeping from it (the failed approaches with their failure conditions, the frozen-height formulation, the working method) is in §2.7, §10 and §11, reconciled against the current Lean files. Where an older statement disagreed with the Lean files, the Lean files won. **Notation follows the formalization-faithful account (*Commutator Holonomy via Affine Trees*): its Appendix is the canonical name table, and this document has been reconciled to it; every change made in that reconciliation is logged in §12.** The former standalone log of unaddressed points (30 items) has been folded in throughout this edition; its disposition is itemised in §13.

**Edition note.** This edition is proofread for consistency of conventions (ρ = class map `rnext`; μ = multiplier `mult`; spectral second eigenvalue = λ₂; classes valid and bounded), cross-references, tag accuracy against the Lean statements, a single impersonal voice, and removal of revision-history narration from the body. Superseded claims are collected once, in §12.1, rather than scattered through the text. The formal account remains the winner on every conflict.

**Tags.** Two axes, and both may appear on one item.

*Evidence — where does it come from, how much is behind it:*

- **[Held]**: proved (the formal Lean 4 development, or a short derivation from proved facts).
- **[Observed]**: exact computation on specific inputs; evidence, never a proof of a universal statement.
- **[Derived]**: worked out by hand, unchecked.
- **[Belief]**: judgment.
- **[Memory]**: literature recalled from memory; verify before citing.
- **[Dragon]**: expected hard resistance (a difficulty flag, not an evidence level).

*Relevance — does it belong in the Bible at all, and at what distance from the main road:*

- **[Core]**: load-bearing; the proof or the working method needs it.
- *(untagged)*: belongs; ordinary content.
- **[Tangent]**: plausibly relevant but off the main road; check it is doing work before building on it.
- **[Parked]**: deliberately shelved; do not spend time here without a new reason.
- **[Irrelevant]**: kept only as a record so it is never relitigated; no re-test value. A tombstone, not a lead.

**The admission rule.** Three things earn a place: what was tested and succeeded, what was tested and failed, and what was untested but is a lead (so nothing gets tested twice). The relevance flag then records *doubt about belonging*, which is independent of whether the thing is true: a [Held] fact can be [Tangent], and a [Belief] can be [Core].

---

## 1. The problem in one page

**The mismatch.** Halving is invisible to 3-adic information, because 2 is a unit in ℤ₃. The step 3n+1 is invisible to 2-adic information. The conjecture lives where the two collide.

**The backward tree.** From a number x you can go back by *doubling* (an invertible reshuffle of base-3 digits, nothing lost) or by *undoing a tripling*, which means dividing by 3. Division by 3 chops a trailing 0 off a base-3 expansion, so the window of known digits slides and one digit is lost. So the ancestor tree is a branching process on the last base-3 digits: one lossless road, one road that forgets.

**The question CHAT answers.** Is the ancestor tree of w blind to some residue class modulo some prime p? That would be a local obstruction, and it would have to be ruled out before the conjecture (the ancestor tree of 1 is everything) could be true.

**The theorem [Held, Core].**

> **The Tree Has No Blind Spots.** For every prime p, every positive integer w with 3 ∤ w, every residue r mod p and every bound B, some n > B reaches w and has n ≡ r (mod p).
>
> **Full Dials** (p ≥ 5). Inside the two-branch skeleton D(v) of the tree, every good 3-adic ending of every length occurs together with every residue mod p.

**What it is not.** It is not the conjecture. It proves nothing about natural density, nothing about covering all integers, and no size bounds. Reading "every ending with every residue" as "most numbers" is the error to avoid. This is taken up again in §4 and §5 (C1, C6).

**Sanity example.** Take w = 1 and p = 7. Pure doubling reaches only the residues 1, 2, 4. The theorem says the tripling-undoing branches fill in the other four. Concretely, 1024604, 1024724, 1024256, 1024544, 1024454, 1024805, 1024841 all reach 1 and cover the seven residues 0 to 6 respectively [Held as instances of the theorem, found by search]. Independently recomputed: each of the seven reaches 1 in exactly 51 steps (standard step count), and their residues mod 7 are 0 to 6 in the order listed. The pattern for arbitrary B is the theorem's content, not this finite check's.

**The hypothesis 3 ∤ w is sharp [Held, elementary].** An odd step lands on 1 mod 3, so a multiple of 3 can only be reached by halving. The ancestors of a multiple of 3 are exactly its doublings.

**A converse worth recording [Derived, Tangent].** A true Collatz conjecture would give the theorem for w = 1 for free (every integer is an ancestor of 1, so every class is covered); the theorem gives no route back from w = 1 to w ≠ 1. The asymmetry is obvious but worth one sentence in any exposition, because it pre-empts the misreading "CHAT is most of the way to Collatz".

---

## 2. The proof, condensed

### 2.1 The skeleton [Core]

Call a number **good** if it is at least 8 and ends in 02 or 22 in base 3 (equivalently, it is 2 or 8 mod 9; the formal proof's set is G = {x ≥ 8 : x ≡ 2 or 8 mod 9}). Define two maps on good numbers:

- **R(x)** = 4x if x ≡ 2 mod 9, else 16x. Both are the *fewest doublings that land back on good ground*: from a number ending 02, ×4; from one ending 22, ×16, since ×2, ×4, ×8 all fail.
- **L(x)** undoes a tripling. Following the formal proof, write

  **L(x) = μ(x)·(2x − 1)/6**,

  where the **multiplier** μ(x) depends only on x mod 27: x mod 27 ∈ {2, 8, 11, 17, 20, 26} gives μ = 4, 8, 16, 2, 4, 2. Writing μ = 2·2^i, this says L(x) = 2^i·m with m = (2x − 1)/3 an odd integer (when x ≡ 2 mod 3) satisfying 3m + 1 = 2x, and i the fewest doublings landing on a good residue. That μ depends only on x mod 27 is because the last two base-3 digits of m are decided by the last three of x; in terms of the padding, x mod 27 ∈ {2, 8, 11, 17, 20, 26} gives i = 1, 2, 3, 0, 1, 0.

Example: L(8) = μ(8)·15/6 = 8·15/6 = 20, with forward trip 20 → 10 → 5 → 16 → 8.

**Why halving terminates: the 2-adic valuation law [Held].** For every good x, v₂(L(x)) = log₂ μ(x) − 1 ∈ {0, 1, 2, 3}. Because x ≡ 2 mod 3, the numerator 2x − 1 factors as 3q with q odd, so v₂(q) = 0; hence L(x) = (μ(x)/2)·q and its 2-adic valuation is decided by μ(x) alone, with no memory of v₂(x). This is why the left branch resets the halving count at every step: the tripling is undone and the subsequent doublings are exactly the fewest that land back on good ground (§2.1 above).

Every number in the skeleton D(v) (everything obtained from a good seed v by R and L) really reaches v **[Held]**. And every positive w with 3 ∤ w has a good ancestor v obtained by multiplying by a power of 2 between 2⁶ and 2⁹ **[Held]**. So the theorem reduces to a statement about this thin two-branch tree.

### 2.2 The 3-adic side: a tower of rings with doors [Core]

Keep only the last K+1 base-3 digits: a **shadow at depth K** (the formal proof's *class at depth K*: a valid c, i.e. c ≡ 2 or 8 mod 9, with c < 3^(K+1); K ≥ 1). The good shadows are **stations**; there are 2·3^(K−1) of them.

**The ring.** The ring map is **ρ(c) = R(c) mod 3^(K+1)** (Lean `rnext`), with iterates ρ^n (Lean `rIter`) and the **accumulated exponent** E(n, c) (Lean `rExp`). These are defined through R(c) = c·4^step(c), where step(c) = 1 if c ≡ 2 mod 9 and 2 otherwise (Lean `rStep`). ρ acts on stations as an invertible map and is transitive **[Held]**, hence a single cycle of length 2·3^(K−1) **[Derived from Held facts; confirmed by direct computation to K = 6]**. At depth 2 it reads 022 → 202 → 222 → 102 → 122 → 002 → back.

**The gearbox [Held].** Name each station by the exponent E with station = 2·4^E (E ranging over 0, 1, 3, 4, 6, 7, … below 3^K; equivalently E ≡ 0 or 1 mod 3, with E ≡ 0 exactly on stations ≡ 2 mod 9). A ring step adds 1 or 2 to E. The dictionary between winding and digits is lifting-the-exponent: v₃(4^m − 1) = 1 + v₃(m). Equivalently, 4^m ≡ 1 mod 3^(K+1) exactly when 3^K | m. A full lap from station 8 winds by exactly 3^K, which is why a lap is a stretch by 4^(3^K).

**The doors [Held].** L reads one more digit than it keeps. If x is known mod 3^(K+2), then L(x) is known mod 3^(K+1): at least K+1 digits survive. So a left turn is a **door** selected by one digit beyond the station's own. Each station has three doors, and each door opens onto a station of the same depth. The ring at depth K+1 is a triple cover of the ring at depth K, and doors run from the upper floor to the lower one. That stacking is the *tower*.

### 2.3 The mod-p side: an affine cocycle, fibres, holonomy [Core]

Put a clock with p positions on every station of the depth-K graph Γ_K. Every road carries an affine rule on the clock:

- right edge: t ↦ 4t or 16t;
- left edge: t ↦ (μ/3)·t − μ/6, where μ ∈ {2, 4, 8, 16} is the multiplier μ(x) of the left branch, as in §2.1 (μ = 2·2^i; Lean `mult`).

This needs 2, 3, 6 invertible, so p ≥ 5; the primes 2 and 3 are handled separately ("The small primes" at the end of §2.5). The rule depends only on the road, never on which number travels it. Check with p = 5: the loop 8 → L → 20 → R → 80 acts as t ↦ 4t + 3, and 8, 35, 62 (all ≡ 8 mod 27) map to residues 0, 3, 1 from 3, 0, 2 respectively.

A **fibre** F_K(c) is the set of residues mod p realised by skeleton members with shadow c at depth K. **Holonomy** Hol(K) is the group of rules obtained by composing the labels of closed walks at station 8, forward or backward. It is a subgroup of the affine group of the clock.

**A structural fact that is easy to miss [Derived].** Every slope in Hol(K) lies in the multiplicative group ⟨2, 3⟩ ⊆ (ℤ/p)*. Right labels have slope a power of 4; left labels have slope μ/3 with μ a power of 2. So Hol(K) ⊆ (translations) ⋊ ⟨2, 3⟩, a proper subgroup of the full affine group whenever ⟨2, 3⟩ is proper (p = 23 is the first such prime; up to 200 they are 23, 47, 71, 73, 97, 167, 191, 193 [Observed by direct computation]). **The theorem itself does not need ⟨2, 3⟩ to be large [Derived; the separation matters]: CHAT needs only one zoom λ ≠ 1 and one translation; only a quantitative spectral gap would need |⟨2, 3⟩| ≥ p^δ (see §9.8).** The ceiling matters a great deal for anything quantitative (§5).

### 2.4 The engine: zoom, twist, commutator [Core]

Two elements of Hol(K), then one commutator.

**The twist [Held].** Leave station 8 through its door at 8 mod 27, land at some station, ride the ring back to 8. The label is x ↦ αx + β with α ≠ 0 and β = −4^E·(8/6) ≠ 0. For p = 5 this is the loop above: t ↦ 4t + 3.

**The zoom [Held].** A pure stretch λ ∉ {0, 1}. There are three cases, exhaustive:

1. **The lap is visible.** If 4^(3^K) ≠ 1 mod p, a full lap around the ring is a pure stretch by it. (p = 5, K = 1: 4³ ≡ 4.)
2. **The lap is invisible, and ord(4) = 3^a with a ≥ 2.** The dial cannot feel ring distances divisible by 3^a. Take two doors on the same wall whose endings agree to a+1 digits (8 and 8 + 3^(a+1)); both have the same multiplier, so their twists cancel, and the ring distance between *their sources* is invisible to the dial. But each door loses a digit, so their *landing* stations agree only to a digits. The ring distance between the landings is not a multiple of 3^a, and the dial feels it. Walk out through one door, back through the other, and what survives is a pure stretch.

   *Example, p = 19, depth 2:* the doors with endings 8 and 35 are the same station; they land on 20 and 92 ≡ 11 mod 27; riding from 20 to 11 winds by 3 and 4³ = 64 ≡ 7 ≠ 1 mod 19. Leaving by the door at 35 and returning by the door at 8 rides backward from 11 to 20, so the loop is t ↦ 7⁻¹t = 11t (the opposite loop is t ↦ 7t).

   **This is the conceptual centre of the proof: the digit a door forgets is exactly what makes the dial feel something.**
3. **The single exceptional prime.** If ord(4) = 3 then p | 63, so p = 7. Both ring distances are invisible, but the doors with endings 2 and 11 have different multipliers (4 versus 16), giving a stretch of 16/4 = 4.

Because the depth at which the ratchet below settles cannot be controlled, all three cases must be handled at *every* depth K, and they are: as K grows, a prime can pass from case 1 to case 2 **[Held]**.

**The commutator [Held].** Undo the twist, undo the zoom, redo the twist, redo the zoom: (α,β)⁻¹ then (λ,0)⁻¹ then (α,β) then (λ,0). The stretch α cancels (stretches commute) and what remains is a pure slide by (λ − 1)β ≠ 0. Run on t it reads (t − β)/α → (t − β)/(αλ) → (t − β)/λ + β → t + (λ − 1)β. For p = 5, with twist 4t + 3 and zoom 4t, it gives t ↦ t + 4; for p = 7, a slide by 5.

**A caution about inverses [Derived].** This walk as written traverses edges backward and so uses inverses of labels. "No inverses needed" would be wrong as a description of this argument; it becomes true only if forward-only generation is proved, and that is [Observed] (§9.2), not proved.

### 2.5 The ratchet and the unsquint [Core]

Now convert one slide into every residue everywhere.

1. **Sizes are constant around a ring [Held].** A right edge acts on fibres injectively (its slope is a unit), so fibre size never decreases along the ring; around a cycle, that forces equality. All stations at one depth have fibres of one size.
2. **Sizes can only shrink with depth [Held].** A deeper shadow is finer, so its fibre sits inside the fibre below it. Sizes are integers ≥ 1 (the seed itself is a witness). So they stop falling: there are K₀ and σ with every fibre at depth ≥ K₀ of size σ. This is a pure existence argument; **K₀ is not controlled**.
3. **After the ratchet, doors cost nothing [Held].** A door's source fibre (upper floor) is contained in its station's fibre (lower floor), and both have size σ, so they are equal. Hence every edge acts as a *bijection* between fibres, and so does every walk, forward or backward. This is the step that the claim "a slide fills the clock" depends on: the slide must permute the fibre, not merely move it.
4. **A slide on a prime clock reaches every hour [Held].** The slide by d ≠ 0 maps F_K(8) onto itself. Starting from any residue and repeatedly adding d, you stay in the fibre, and since p is prime d is invertible, so you reach every residue. So σ = p and every fibre at depth ≥ K₀ is the whole clock.
5. **Unsquint [Held].** A fibre at a shallower depth contains the finer fibre above it, so every fibre at *every* depth is full. That is Full Dials.
6. **Largeness [Held].** Any number with ending c is at least c. Take c = 3^(B+2) − 1, all 2s in base 3, which is good. Every skeleton member with that ending exceeds B.

**What the ratchet is not [Derived; a recurring over-reading].** The ratchet is a pigeonhole: it produces *some* full fibre at *some* depth, with no rate. It does not replace spectral expansion, and it yields no mixing statement of any kind. Quantitative claims must come from elsewhere (§9.3, B3, B8).

**The small primes [Held].** For p = 2, 3 the clock arithmetic breaks (6 is zero), but explicit constructions suffice: doublings for even targets, and numbers of the form (2^k·w − 1)/3 with k chosen so that 2^k·w ≡ 1 mod 3 (or mod 9 for p = 3, remainder 0).

### 2.6 Why it works, in two sentences [Core]

The 3-adic digits and the mod-p clock are two coordinates of one object, and the roads twist the clock as you move on the digits. The twist is non-commutative enough, thanks to the lost digit, that loops produce a slide, and primality turns one slide into the whole clock.

**Where each hypothesis bites.** *p prime*: a slide must generate the clock. *p ≥ 5 in the machine*: labels must be invertible. *3 ∤ w*: otherwise the tree is a single doubling corridor. *The lost digit*: without it the loop group would be too commutative to produce a slide in case 2. *Stabilisation*: existential, which is the reason there are no effective bounds. **No "Artin primitive-root wall" is in the proof [Derived]: the only requirements on p are λ ≠ 1 and p ≥ 5; the one special prime is 7 (case 3).**

---

### 2.7 The same engine, seen as "no frozen height" [Core, but the equivalence itself is Derived]

There is a second way to say what §2.4 achieves. A **frozen height** on the depth-K graph is an assignment s(c) of a residue mod p to every station c such that every road respects it: s(next station) = (4 or 16)·s(c) along right edges, and s(end of a door) = (that door's label)(s(start)) along left edges. Such a landscape would lock the residue mod p of every skeleton member to its 3-adic ending.

**Equivalence [Derived, argued by hand; not in Lean].** A frozen height exists iff Hol(K) has a common fixed residue iff Hol(K) contains no nonzero translation. (If Hol(K) fixes t₀ at station 8, define s(c) as the image of t₀ under any walk from 8 to c; this is well defined because closed walks fix t₀, and it exists because the ring makes the graph strongly connected. Conversely a frozen height makes s(8) a common fixed point. For the last equivalence: slopes commute, so every commutator in a subgroup of Aff(p) is a translation; if there is no nonzero translation the subgroup is abelian, and an abelian subgroup of the affine group of a prime field has a common fixed point.) So the translation of §2.4 is exactly the statement that no frozen height exists.

**The contradiction, case by case [Derived].** If 4^(3^K) ≠ 1, going once around the ring forces s(8) = 0, hence s ≡ 0 everywhere, but any door sends 0 to −μ/6 ≠ 0. If ord(4) = 3^a with a ≥ 2, the doors with endings 8 and 8 + 3^(a+1) share a label and (heights depending only on the exponent mod 3^a) have equal source heights, so equal target heights; but their targets differ at the 3^a scale, so the heights differ. For p = 7 the doors 2 and 11 force s = 2 and s = 6 at the same station-type. This route avoids the bookkeeping of return paths, but it has only been argued by hand, whereas the formal proof uses the zoom-twist-commutator route of §2.4.

**A mistake that is easy to make.** The equalities above are between *values of the hypothetical height function*. They are not claims that the two integers 8 and 8 + 3^(a+1), or their images, are congruent mod p. They are not (they generally differ mod p). Reading the argument as an equality of residues makes it appear to prove a false statement.

**Computational check [Observed, §9.9].** Setting up the frozen-height conditions as a linear system over 𝔽_p and solving: no solution exists for any of the 64 pairs (p, K) tried.

## 3. A finite model of the forgetting [Held, as mathematics; **Tangent** — a heuristic, not used in the proof]

Take the six depth-1 endings 2, 8, 11, 17, 20, 26 and draw an arrow from each to every ending in the class where its door lands (the lost digit unknown). Concretely, using the multiplier table of §2.1: L(2) = 2, L(8) = 20, L(11) = 56, L(17) = 11 all land ≡ 2 mod 9, while L(20) = 26 and L(26) = 17 land ≡ 8 mod 9 — matching the formal proof's μ table exactly. So A has rows (1,0,1,0,1,0) for the first four endings and (0,1,0,1,0,1) for the last two. It has rank 2, A² has rank 1 with every row (2,1,2,1,2,1), A³ = 3A², the characteristic polynomial is X⁵(X − 3), and the number of closed routes of length n is exactly 3^n. Since every row sums to 3, A/3 is a genuine stochastic matrix that **mixes in exactly two steps**, with stationary weights 2/9 on each of 2, 11, 20 and 1/9 on each of 8, 17, 26.

This is the exact shadow of the heuristic "after two left turns the start is forgotten". It is an illustration and plays no part in the proof. The modelling assumption, that the lost digit is free, is the one thing nothing here proves about the real map, and the leak is real: a small Lean statement of it is on the to-do list (§6, item 5), `¬ ∃ f, ∀ x, InG x → L x % 27 = f (x % 27)`, with witnesses 8 and 35.

**Link to §9.3.** This six-state matrix is the base of the 6p-state chain in §9.3: the same transitions on the endings, with the clock position updated by the affine label of each door. The observed spectral gap there is evidence that the forgetting survives being carried across the clock fibres. It is evidence, not a proof of an effective theorem. What supports the model is §9.4's agreement with real trajectories, not the model's own elegance.

**Independent test of the invariant measure (Auxiliary Report E2) [Observed].** Over 400 random 300-digit ternary starts and 150 steps each (60,000 steps), cell frequencies along genuine L-orbits are 0.224, 0.109, 0.224, 0.113, 0.221, 0.109 for cells 2, 8, 11, 17, 20, 26, against the predicted (2, 1, 2, 1, 2, 1)/9. The mean of (ln L¹⁵⁰(v) − ln v)/150 is 0.5197, against the predicted (7/3)·ln 2 − ln 3 = 0.5187. The gap is within sampling noise. The matrix of §3 is depth-1 only; E2 follows the real integer orbit, which is the intended target, so agreement here is the load-bearing evidence that the six-state chain's stationary law is correct in practice.

---

## 4. The surrounding landscape [Tangent throughout — the map, not the road; all literature here is recalled from memory and must be checked before citing]

**Local-global.** CHAT is a local statement (no obstruction at 3 and at one prime). The conjecture is global (everything is an ancestor of 1). Local statements alone cannot reach the global one, and that gap is the archimedean place.

**Known results of the neighbouring kind.**

- Almost all numbers have finite stopping time (Terras; Everett), and Tao (2019) shows almost all orbits attain almost bounded values, in the sense of logarithmic density. These are *forward* statements and rest on different randomness (see C3 below).
- The number of n ≤ X that reach 1 is at least a power of X; as recalled, Krasikov and Lagarias give an exponent near 0.84 [Memory].
- Cycle results are archimedean: the known obstacles are bounds on how close 2^a and 3^b can be (transcendence-type input), together with large computer verification of trajectories.

**Structural viewpoints.**

- *Conjugacy to the shift on ℤ₂* (Bernstein, Lagarias): Collatz as a 2-adic shift. The backward tree is the 3-adic counterpart, and CHAT lives there.
- *The ×2 ×3 philosophy* (Furstenberg): rigidity under joint multiplication by 2 and 3. CHAT looks like a finite-field, 3-adic shadow of such rigidity. This is an analogy and not a theorem.
- *Random walks on affine groups over 𝔽_p with slopes in ⟨2, 3⟩*: this puts the quantitative questions in the territory of sum-product estimates and expansion in affine groups (Bourgain–Gamburd line of work). Precise statements are not given here.
- *Skew products*: the tower with affine labels is a skew product over a 3-adic automaton with an affine cocycle. CHAT says the cocycle is not cohomologous to a trivial one. Ergodicity is the natural language for the limit.
- *Adelic Lyapunov balance [Held]*: the sum of the Lyapunov exponents of L across all places of ℚ vanishes identically, by the Artin product formula Σ_v ln|y|_v = 0 applied to y = μ(x)/3 against the stationary measure. Concretely, λ_∞ = (7/3)ln 2 − ln 3, λ₂ = −(7/3)ln 2, λ₃ = ln 3, λ_p = 0 for p ≥ 5, and the sum is 0. The archimedean growth rate (7/3)ln 2 − ln 3 ≈ 0.5187 that appears in §5 A1's side note is exactly this λ_∞. The multiplier 2^0.75 ≈ 1.68 quoted there is a rounded form of the per-left-turn size multiplier e^(λ_∞) ≈ 1.680, and λ_∞ equals minus the sum of the two non-archimedean terms. Reading the growth rate this way is the first step toward any quantitative transfer-operator statement (B7, B8).

**What is genuinely new here, as far as the author can tell [Belief].** The explicit holonomy mechanism, especially the *blur as engine*. No instance of the same combination is known to the author, but no literature search has been made; a literature check must come before any priority claim.

**Possible external homes for the mechanism [Belief; Tangent — orientation only].** Every attribution here is unchecked. One supplied link (Lindenstrauss–Varjú) turned out to concern Euclidean isometries and self-similar measures, not 𝔽_p, and must not be reused without re-verification. Stripped of the Syracuse setting, CHAT proves a local-global surjectivity statement for one non-invertible, congruence-guarded affine system. Four fields it might connect to, each named from memory:

1. *Strong approximation for guarded semigroups.* Classical strong approximation (Nori, Weisfeiler, as recalled) concerns Zariski-dense groups. A system where inversion is unavailable and steps are guarded by residues is outside it. Whether CHAT's ring-loops-plus-ratchet argument extends to a class of such systems is an open question, not a result.
2. *Automata on rooted trees.* D(v) can be read as a transducer on the 3-adic tree coupled to an affine clock. Whether Hol(K) fills the ceiling at all depths is a level-transitivity question of the kind studied by Grigorchuk and Nekrashevych's school.
3. *Non-Lipschitz p-adic dynamics.* As recalled, Anashin's ergodicity theory treats 1-Lipschitz maps; a map that divides by p is outside it. Whether the blur mechanism gives ergodicity criteria for such maps is untested.
4. *Affine expanders.* Random walks on Aff(𝔽_p) with linear parts in ⟨2, 3⟩ sit in sum-product territory (Bourgain–Gamburd, as recalled; the Lindenstrauss–Varjú citation is suspect, see above). The gap in §9.3 and §9.8 is consistent with an expander family, and it is not a proof of one.

Do not claim more than "worth checking" for any of these until the literature has been read.

## 5. The frontier

### Tier A: low-hanging fruit

**A1. Left-only equidistribution for typical seeds. Start here.** [Derived] [Core for the quantitative programme]

A chain of k left turns from a seed v depends only on v mod 3^(k+2), and each turn consumes one fresh base-3 digit of v. For seeds up to 3^M the pair (ending mod 27, residue mod p), over the first M steps, is up to a CRT error of about p/3^M exactly a Markov chain on 6p states, and the six-state matrix of §3 is its base. Mixing reduces to the transitivity of the loop group on the clock, which is CHAT's content, and quantitatively to the spectral gap of an explicit 6p × 6p matrix. The payoff would be an *effective* theorem: for all but a small fraction of seeds up to X, the residue after k left turns is within ε of uniform once C·log p ≤ k ≤ (horizon), with explicit rates, where CHAT's stabilisation depth is only existential. Note the distinction: *visiting every residue* is a coupon-collector event and needs about p·log p steps, not O(log p); equidistribution of the residue at a given step is the fast statement (§9.5).

*Needs proving:* that left-only loops contain a nonzero translation (expected, unproved; the commutator argument above uses right-edge rides), and irreducibility with aperiodicity of the 6p-state chain.

*Catch:* a seed has only about log₃ v digits, so runs are capped at logarithmic length; past that the dynamics are deterministic. The theorem would be "typical seed, orbit of logarithmic length", not an infinite-orbit statement. For 3-adic seeds the full ergodic theorem is available, but integers are a measure-zero set. [Dragon-lite] A deterministic exception worth naming [Derived]: seeds of the form x_k = 2 + 3^k·u with u ≢ 0 mod 3 remain in cell 2 for exactly k − 2 left turns, because x_k ≡ 2 mod 3^j for j ≤ k and the fixed point of L on cell 2 is 2 itself; until those digits are consumed, the residue mod p stays in the one-dimensional coset 2 + ⟨4/3⟩·(x_k − 2), so the CRT horizon for such a seed is k − 2 turns shorter than for a generic seed of the same size. This is a logarithmic delay, not a failure, and §9.4's agreement past the digit horizon shows it is invisible to the chain on average.

*Side note [Derived, quick arithmetic]:* under the chain's stationary weights a left turn multiplies size by about 2^0.75 ≈ 1.7 on average, while consuming log₂3 ≈ 1.58 bits of the seed's digit supply. That comparison is the first thing to examine when asking how far the horizon can be pushed.

**A2. Pin down Hol(K) exactly.** [Done for tested primes: see §9.1] [Core for the quantitative programme]

§2.3 gives the ceiling: translations extended by ⟨2, 3⟩. Computed for 11 primes, including four proper-subgroup primes (23, 47, 71, 73), Hol(K) *equals* the ceiling at K = 1 and 2. So the right conjecture is **Hol(K) = ℤ/p ⋊ ⟨2, 3⟩** [Observed]. ("The full affine group" is the wrong guess: it holds at small primes only because ⟨2, 3⟩ happens to be everything there; see §12.1, item 8.)

The question "is ⟨2, 3⟩ everything?" is a two-generator cousin of Artin's primitive-root problem, and the proper-subgroup primes in §2.3 are its failures. [Belief; reasoning only, no literature check.] The ceiling's size p·|⟨2, 3⟩| therefore fluctuates with p, and any uniform spectral-gap statement has to pass through the primes where it is unusually small (see §9.8). **The theorem itself does not need ⟨2, 3⟩ large — only the gap does (§2.3).**

**A3. Several primes, prime powers, composite moduli. [Derived; tested in §9.6] [Core: a cheap, real extension]** No joint zoom is needed (§12.1, item 9). Let H be the loop group over the clock ℤ/m, with gcd(m, 6) = 1. Slopes commute, so every commutator in H is a pure translation. The map H → H_p (reduce mod each prime p | m) is onto, so the commutator subgroup maps onto the single-prime commutator subgroup, which is all of ℤ/p by CHAT. A subgroup of the cyclic group ℤ/m that maps onto ℤ/p for every p | m is all of ℤ/m. So H contains *every* translation of ℤ/m, and the ratchet runs unchanged over the clock ℤ/m. This covers two or more distinct primes, prime powers p^k, and every modulus coprime to 6. The 2-adic part and powers of 3 beyond the ending are not covered.

**A4. Prime powers.** Same argument as A3; see above. The commutator identity (1, (λ−1)β) holds in any commutative ring, and (λ−1)β is a unit mod p^k as soon as it is a unit mod p.

**A5. Effective size bounds. [Belief]** The ratchet is why none exist. A direct construction may be possible: use the explicit translation loop's length to produce, for each residue, a skeleton member of explicit size, giving an exponential-in-p upper bound on the smallest ancestor with a given residue. That makes CHAT quotable as a computable statement. **The real obstruction that such a construction must clear [Derived]: a loop can only be applied to a number whose deeper digits match the door's requirement, and the ratchet works on sets, not single numbers. Any explicit construction must either supply the matching digits or argue past them; a naive "apply the translation loop k times" does not.**

**A6. Housekeeping.** Rename `exact_one_step_mixing` (it is a two-step statement). Add references. Check p = 23 before anyone cites "full group". Keep the Lean-faithful paper and the exposition citing each other. **Audit the paper's Lean-name appendix against the current file (it was copied from the written account and only partly re-checked).** Add a bibliography to the LaTeX paper (Lagarias's survey, the lifting-the-exponent lemma, Mathlib) and replace the placeholder author line.

**A7. Reference audit. [Core discipline for anything citeable]** Every [Memory] item in §4 and in the external-homes paragraph (Terras, Everett, Tao 2019, Krasikov–Lagarias with its exponent near 0.84, Bernstein–Lagarias, Furstenberg, Nori, Weisfeiler, Nekrashevych, Anashin, Lindenstrauss–Varjú) must be checked against the literature before it appears in any document meant for citation. An unchecked attribution is worse than a missing one, because a reader will assume it was checked. **The Lindenstrauss–Varjú instance has a known mismatch (isometries, not 𝔽_p); treat it as wrong until a correct affine-walks-over-finite-fields citation is found.**

### Tier B: real mathematics, uncertain payoff

*Orientation: B8 (the spectral gap) and B9 (growth rates) are the load-bearing quantitative items. B8 is the Dragon and the engine for A1, and §9.3 calls it the sharpest open target in the document. B2–B7 are possible homes and orientation; do not build on them before A7's literature audit.*

**B1. A general CHAT lemma. [Core for the generalisation programme]** Extract: a finite-state automaton with an affine cocycle over a prime field; when does the loop group contain a translation? Test on 5x+1 and 7x+1, where the mismatch is between 2 and a different prime. If it transfers, the structure is the mismatch and not an accident of 3. If it fails, that is informative too. **Evidence so far (§9.10): raw coverage shows no exception for any tested q — so the test must probe the *method*, not the conclusion.** Conway's undecidability result applies to generalised periodic-rule Collatz functions as a class, not to each individual qx+1; it is not a reason to avoid 5x+1 [Derived].

**B2. The skew-product reading.** Restate CHAT as "the cocycle is not cohomologous to a trivial one" and look for existing ergodic-theoretic results on skew products with affine-group cocycles.

**B3. Sum-product and expansion.** With Hol equal to translations extended by a multiplicative group generated by 2 and 3, quantitative questions about its action on the clock are in sum-product territory. This is where A1's spectral gap could meet existing technology.

**B4. Counting.** Combine the effective chain with a tree count to get lower bounds on ancestors in each residue class up to X. The skinny skeleton is too thin for this alone; it needs the full tree.

**B5. An abstract guarded-semigroup theorem [Belief; Tangent until the hypotheses are written].** State coordinate-free hypotheses on a transition automaton with q-adic valuation drops under which orbits surject onto ℤ/m for all m coprime to q. This only becomes a theorem once the hypotheses are written down and the three CHAT mechanisms (blur, commutator, ratchet) are shown to need nothing more.

**B6. Level monodromy via automata theory [Belief].** Replace the prime-by-prime checks of §9.1 and §9.2 by a uniform argument. Whether self-similar-group machinery proves that Hol(K) reaches the ceiling for all p ≥ 5 and all K is exactly the open conjecture, and no sketch exists.

**B7. A transfer-operator explanation of the horizon overrun [Belief; Tangent — heuristic until a computation says so].** Model L as an expanding branch map on ℤ₃ with an affine cocycle and study the spectrum of its transfer operator. It might explain why §9.4's chain keeps matching real trajectories past the digit supply.

**B8. The spectral gap.** [Dragon] Prove a uniform lower bound on the gap of the 6p-state chain (or of the averaged left-turn operator on 𝔽_p). This is the quantitative engine for A1. The observed λ₂ stays between roughly 0.51 and 0.70 with no visible decay in p; at primes where ⟨2, 3⟩ is small the gap does not collapse either. A proof would almost certainly need sum-product or expander input.

**B9. Growth rates and the horizon.** Under the stationary weights of the six-state matrix a left turn multiplies size by about 1.7 on average while consuming log₂ 3 ≈ 1.58 bits. Whether this comparison can be turned into a rigorous statement about how far a typical seed can be driven before its digit supply is exhausted is open.

### Tier C: dragons [Core — these are guardrails, not distractions]

**C1. The archimedean place.** Profinite density means every finite pattern appears at some size. It says nothing about natural density or about covering all integers. The tree grows more slowly than the integers, and the gap between "dense in the profinite sense" and "everything" is the conjecture itself. More of this machinery will not cross it. Quantitatively [Derived; a guardrail]: the Haar mass of a shadow at depth K together with a residue mod N is (6N)⁻¹·3^(−K), but the count of integers in [1, X] lying in that class incurs a boundary truncation of order 3^K·N/X, so the Haar measure cannot be read as an integer density unless X ≫ 3^K·N. Since the depth needed grows with the number of left turns, the boundary error dominates at every finite X, and only an explicit spectral gap on the depth-K transfer operator can turn profinite density into asymptotic density. This is the quantitative face of the archimedean gap, and it is why C1 is a wall, not a nuisance.

**C2. Cycles and divergence.** [Derived] CHAT is evidence that *no congruence-based argument can rule out a nontrivial cycle*: a cycle through c is a finite set of ancestors of c, and the theorem says c's ancestors have no blind spots. Ruling cycles out is archimedean, uses sizes, and belongs to a different world of tools. No time should be spent here without a reason that avoids this point.

**C3. Backward versus forward randomness.** The backward tree consumes fresh 3-adic digits; the forward map consumes 2-adic ones. These are different sources of randomness, and nothing in CHAT is a statement about forward trajectories.

**C4. Quantifier slippage.** "Every seed", "almost every seed" and "the seed 1" are three different theorems. Each sentence of each draft should say which one it is.

**C5. Reading existence as structure.** The ratchet is a pigeonhole. Before building on it, ask whether the mechanism is visible in the explicit loops. (It is, in the commutator and the blur, which is why those are the parts worth generalising.)

**C6. Density and the forward results.** CHAT says nothing about natural density: the set of n that reach a fixed w and lie in a fixed residue class mod p is infinite, but could still have density zero. The almost-all results (Terras, Everett, Tao 2019) are forward statements about typical orbits; CHAT is a backward local-obstruction statement. They live in different regimes.

---

## 6. Suggested order of work

1. ~~Compute Hol(K) for p = 23 and others (A2).~~ Done, §9: Hol(K) = ℤ/p ⋊ ⟨2, 3⟩.
2. ~~Check left-only holonomy per prime.~~ Done to p = 97, §9. What remains is a *uniform proof*.
3. Prove the spectral gap (uniform in p, observed to sit between about 0.5 and 0.7) and write A1 as a theorem, with the horizon stated in the abstract. A proof sketch via sum-product and affine-expansion tools (Bourgain–Gamburd line) is still owed; none has been started.
4. Formalise the composite-modulus argument (A3, A4): replace the field `ZMod p` by `ZMod m` where field properties were used, keeping the single-prime case analysis prime by prime.
5. Formalise the leak in Lean (cheap, unclaimed): `¬ ∃ f, ∀ x, InG x → L x % 27 = f (x % 27)`, witnesses 8 and 35. It machine-checks why the naive 6p-state proof fails (§10.1) and keeps §3's model honest.
6. Run the 5x+1 and 7x+1 transfer test (B1); build the general-q machinery (good cells mod q², the fewest-doublings rule, the ring, the holonomy) and test the loop group for q = 5, then q = 7 and 17.
7. Test the 6p-state gap at much larger p, with the real state-dependent chain (not just the averaged operator), and compute the 5x+1 gap, which has never been run.
8. Add references to the LaTeX paper and audit its Lean-name appendix (A6, A7).
9. Only then effective bounds (A5) or counting (B4).

**Do not:** chase the natural density of the ancestors of 1 directly; try to exclude cycles by congruences; or describe CHAT as "progress on Collatz" without saying which gap it leaves.

---

## 7. How to talk about the work [Core]

The accurate sentence: *the inverse Collatz tree of every positive w not divisible by 3 has no local obstruction at the 3-adic place together with any single prime, and the proof exhibits an explicit affine holonomy mechanism, in which the digit lost by a left turn is what makes the clock turn, that may transfer to other maps.* It does not say Collatz is true and it does not say the tree is large. The paper should be known for the mechanism (the commutator, the ratchet, the blur as engine), not oversold on the gap it leaves.

**Two ways to pitch it, depending on the audience.**

- *To Collatz readers:* the sentence above, plus: no natural density, no bounds on orbit sizes, no exclusion of cycles.
- *To dynamics and algebra readers:* "For a specific non-invertible affine system on the integers, governed by a 3-adic finite-state automaton, we prove that orbits meet every pair of a good 3-adic ending and a residue modulo any prime p ≥ 5 (and, by a short extension, modulo any modulus coprime to 6). The proof isolates a mechanism, in which the digit lost by one branch forces nontrivial holonomy, that we conjecture transfers to other systems." Do not call this a general theorem, and keep numerical spectral-gap observations out of the abstract.

## 8. Companion documents and their state [Core housekeeping]

*CHAT: Every Remainder, Everywhere* is the intuitive exposition with worked examples. *Inverse Collatz Trees and Simultaneous Density* is the LaTeX paper, faithful to the Lean development; **its Appendix is the canonical name table, and where any notation below disagrees with it, the paper wins**. The Lean files hold the formal statements: the main development and the separate six-state amnesia file. In content they are subordinate to this document and the Lean files; on notation, the paper's Appendix wins.

**Known staleness [Housekeeping].** The exposition still says it did not recompile Lean, and does not yet mention the −336 key (§9.7), the composite-moduli result (A3/§9.6), or the frozen-height reformulation (§2.7); it is subordinate to this document but out of date in those respects. The LaTeX paper's remark after Proposition 4.3 (single cycle) is marked as not formalised; if it is ever formalised, the paper must be updated. The paper has no bibliography and carries the placeholder author line "The Authors" (A6, A7).

## 9. Fruit picked: computational results

All of these are **[Observed]** unless tagged otherwise: exact computation on specific primes, not proofs.

**9.1 The ceiling is the answer (A2).** For p = 5, 7, 11, 13, 23, 31, 47, 71, 73, 79, 89, with depths K = 1 and 2, the loop group Hol(K), built from forward and backward closed walks at station 8, is exactly (all translations) ⋊ ⟨2, 3⟩. Its order is p·|⟨2, 3⟩|, for instance 253 = 23·11 at p = 23. The slope subgroup produced by loops equals ⟨2, 3⟩ in every case, including where it is a proper subgroup. Conjecture: **Hol(K) = ℤ/p ⋊ ⟨2, 3⟩ for all p ≥ 5 and all K.** Nothing above the ceiling is possible, and in every computed case nothing below it occurs. Independently confirmed for the cell graph at all 44 primes 5 ≤ p ≤ 200 by semigroup closure from (2, id) (Auxiliary Report E6); the cell-graph holonomy equals the tower holonomy at K = 1 (§9.6), so this extends §9.1's range.

**9.2 Left turns alone already generate it (A1, finite check).** Restrict to *forward* closed walks using only left edges. The set of labels again equals the full ceiling group, containing all p translations, for every prime from 5 to 97 at depth 1 and 2, and at depth 3 for primes below 41. So a left-only translation exists in every tested case, and the commutator argument's use of right-edge rides is not essential. What is missing is a uniform proof. Independently, for the pure-left orbit from admissible starts v < 6000, all pairs (cell mod 27, residue mod p) are covered within 3000 steps at p = 5 (median 138, max 704) and p = 7 (median 216, max 699), with no failures (Auxiliary Report E11). The medians are of order 9p·ln(6p), consistent with §9.5's coupon-collector scale p·ln p up to constants; the constant is governed by the rarest cell-residue pair, probability 1/(9p). Small starts do not probe the exceptional integers 2 + 3^k·u at large k.

**9.3 The six-state model extends to a 6p-state chain with a clean spectrum. [Observed; Tangent — heuristic support for A1, not part of the proof]** States are (ending mod 27, residue mod p); from (e, t) the chain moves to (f, ll(e)(t)) with f uniform among the three endings in e's target class. For every prime tested (5 to 37, then 47, 71, 97, 131):

- the chain is **primitive** (some power has all entries positive, within at most 9 steps), so it is irreducible and aperiodic;
- its stationary law is **exactly uniform in the residue**, times the weights (2, 1, 2, 1, 2, 1)/9 on the six endings, as in §3;
- the second-largest eigenvalue modulus stays between about **0.51 and 0.70** across the whole range, with no visible decay as p grows (p = 131 gives 0.577).

A gap that does not close as p grows is what an expander-type phenomenon looks like, and it would put the quantitative side squarely in the sum-product territory of B3. No proof is known; this is the sharpest open target in the document.

**Closed words up to length 8 (Auxiliary Report E4) [Observed].** Cyclic cell words of length ℓ ≤ 8 in the cell graph number 3, 9, 27, 81, 243, 729, 2187, 6561 = 3^ℓ, and the adjacency matrix has Tr A^n = 3^n for n = 1, …, 8 with eigenvalues 3 and five zeros (matching §3's characteristic polynomial X⁵(X − 3)). Solving each composite affine map for its fixed point and verifying by iterating L, the only positive integer periodic point is 2. This extends the earlier ℓ ≤ 5 check to ℓ ≤ 8 and adds no information on the conjecture; it reproduces §3's closed-route count.

**9.4 The chain predicts real integers, and keeps predicting past the digit horizon. [Observed; Tangent — but the load-bearing evidence for the §3 model]** A sample of 6,000 random good seeds in [3^22, 3^23) was iterated under the genuine map L and compared with a simulation of the chain. For p = 7 and p = 23, the fraction of seeds that have visited every residue by step k matched the chain to within sampling noise (p = 7, step 20: 0.623 real versus 0.628 chain; step 44: 0.941 versus 0.947), and the residue distribution at each step was at the sampling noise floor from uniform. This held out to 44 steps, *twice* the digit horizon of about 22. So the horizon in A1 is a limit of what a proof of this shape can reach, not an observed change in behaviour. [Observed, one choice of horizon]

**9.5 Coverage versus equidistribution.** Coverage of all p residues is a coupon-collector event. With the residue near-uniform at each step, covering needs about p·log p steps. For p = 23 the observed coverage by step 44 was about 3.5%, in line with the chain. An honest effective theorem therefore has two parts: fast equidistribution at a given step (about log p steps to mix), and coverage only when the horizon exceeds roughly p·log p, that is for p up to about log X / log log X.

**What to do with this.** The conjecture in 9.1, the left-only generation in 9.2 and the uniform gap in 9.3 are three clean statements, in increasing order of difficulty, each of which a computer cannot settle but now strongly suggests. If the gap can be proved, the effective A1 theorem follows by standard Markov-chain bounds, and the CRT cutoff gives the stated horizon.

**9.6 Composite moduli and prime powers (A3, A4).** The loop group Hol_m(K) was computed for m = 25, 49, 121, 125, 35, 55, 65, 77, 91, 143 and 385 = 5·7·11, at K = 1 (and K = 2 except for 385), and independently for N ∈ {5, 7, 11, 13, 17, 19, 23, 25, 29, 31, 35, 49, 55, 77, 125} at K = 1, 2, 3, for N = 307 at K = 1, 2, and for K = 4 with N ∈ {5, 7, 13, 23, 35} (Auxiliary Report E7). In every case the translations are *all* of ℤ/N and |Hol| equals N·|⟨2, 3⟩ mod N| exactly. Selected orders: N = 5 → 20; N = 7 → 42; N = 13 → 156; N = 23 → 253; N = 35 → 840; N = 125 → 12500; N = 307 → 31314 (= 307·102). The holonomy is already full at K = 1. This agrees with the commutator-subgroup argument in A3, which has only been derived by hand.

**9.7 An explicit search-free key.** At depth 1, the twist is (32/3)t − 16/3 and the zoom is 64t, and their commutator is the pure slide by **−336 = −2⁴·3·7**. Independently reproduced (Auxiliary Report E1): the three loops W₁, W₄, W₅ (as defined there) have fixed points 2, −101/17, −283/115; the commutators [W₁,W₄] and [W₁,W₅] are the pure translations 5/9 and 19/27; and −5·(5/9) + 4·(19/27) = 1/27 recovers the same depth-1 key in different coordinates. The slope-exponent vectors (2,−1), (6,−4), (7,−5) have pairwise determinants −2, −3, −2. The values −135/256 and −513/512 for the W₄, W₅ commutators, found in older notes, are wrong: they come from an extra factor 1/(αμ) in the composition convention (§12.1, item 6; §13, item 28). It is nonzero mod every prime p ≥ 5 except p = 7, which uses doors 2 and 11 instead (slide −16 ≡ 5). Being a rational number, it reduces mod any modulus coprime to 6, and is a unit mod m exactly when gcd(336, m) = 1. **Open [Derived as a question, unresolved]: whether this key generalises — a closed form for general K needs a discrete logarithm mod 3^(K+1), and the qx+1 analogue is unknown (§13, item 27).**

**9.8 Spectral gap: more evidence, and a worry.** The averaged left-turn affine operator on 𝔽_p, with the true weights (2,1,2,1,2,1)/9, has second eigenvalue (written **λ₂**; ρ is reserved for the class map, §12.3) 0.617 (p = 101), 0.577 (499), 0.549 (1009). At primes where ⟨2, 3⟩ is unusually small, the gap does not visibly close up to 2600: p = 431 (|⟨2,3⟩| = 43) gives 0.692 and p = 601 (75) gives 0.559. **Caution on reading these as asymptotics [Derived]: they are single-prime values. The equal-weight values retired in §10.8 ran from 0.93 down to 0.73 across primes, which is scatter, not an asymptotic.** A uniform gap would need |⟨2, 3⟩| ≥ p^δ for all p; it is not known that this holds, and Bourgain-type theorems need it as a hypothesis. **The theorem needs none of this (§2.3); only the quantitative programme does.**

**9.9 Reconciliation checks [Observed].** Five items: single-step fidelity, frozen heights, a truncated-tree check, a caution about small primes, and the tower translations.

- *Fidelity of the graph to the real tree.* For 153,531 genuine integer steps (random good integers, and the actual skeleton nodes below 8 and 20 to depth 12), at depths K = 1, 2, 3 and p = 7, 19, 23, the predicted class (right edge, and left edge from the finer ending) and the predicted residue mod p (the road's label) matched the real R(y) and L(y) every time: zero mismatches. This tests the definitions and single steps, not the stabilisation claim.
- *Frozen heights.* The linear system of §2.7 has no solution for every prime 5 ≤ p < 110 at K = 1, 2 and every p ≤ 37 at K = 3 (64 cases). The vanishing of individual tower commutators at isolated primes (Auxiliary Report E9; last item of this section) is not in tension: a frozen height requires *every* closed walk at station 8 to fix one residue, whereas E9 tests one commutator pair at a time. At p = 11 the pair (W₁, W₄) vanishes, but (W₁, W₅) does not, so the loop group still contains a nonzero translation, as the frozen-height solver's "no solution" confirms.
- *Truncated-tree fibre check [Observed; Tangent — consistent but not confirmatory, see §10.6].* In the skeleton tree from seed 128 to depth 20, every station's fibre was the full clock for p = 5 and p = 7 at depths 1 to 3, as the theorem predicts. (Recorded so it is not re-run.)
- *A caution about small primes.* Hol(K) is all of Aff(p) for p = 5, 7, 13, 19 and K ≤ 3. That is consistent with §9.1 (for those primes ⟨2, 3⟩ is everything) but must not be generalised; p = 23 is the first counterexample.
- *Tower translations nonzero, and the base-vertex gap (Auxiliary Report E8, E9) [Observed].* The three loops W₁, W₄, W₅ lift to closed walks in Γ_K at base vertices 2, 56, 56 (K = 3) separating to 2, 380, 137 (K = 5) and 2, 380, 1595 (K = 6), computed exactly from their 3-adic fixed points. At level 2 all three close at vertex 2 with no drift, so the base-vertex-gap reduction is complete if K₀ ≤ 2 (§2.5 leaves K₀ uncontrolled). Drift residues of 47 and 26 mod 81, found in older notes, are false; the correct values are 56 and 56 (§12.1, item 7). Conjugating each loop to base vertex 8 and forming commutators in Aff(𝔽_p), no prime p < 400 kills both pairs (W₁,W₄), (W₁,W₅), (W₄,W₅) at any tested level K = 2,…,7, and no prime kills all three pairs. Individual commutators do vanish at isolated primes — (W₁,W₄) at p = 5 and p = 11, (W₁,W₅) at p = 19 and p = 59 — consistent with the derived vanishing condition 34·4^c + 101 ≡ 0 mod p, where c is the exponent difference of the connecting R-paths. This is evidence about the unit property, not a proof for all K and p.

### 9.10 qx+1: probes, exceptions, and what transfers [Observed/Derived; Core for B1 — the map data for the generalisation test]

*Folded in from the former unaddressed-points log, items 7–14: the largest block of results previously recorded only in working notes.*

**Raw backward-tree probes [Observed].** For q = 3, 5, 7, 11, 13, 17 (about 2.5–3 million nodes each, from w = 1; 5x+1 also from w = 3 and 7), no prime below 60 (below 80 for 5x+1) had any missing residue. Raw coverage is therefore not where any exception shows up — these probes test the *conclusion*, not the *method*. Any B1 test must probe the loop-group machinery, not just coverage.

**Two different kinds of exception [Observed as finite computations; Derived as the taxonomy].** Do not conflate them:

- **Exceptional p** (for fixed q): primes where the multiplier 2^M (M = ord_q(2)) has order exactly q. These form finite sets per q: **q = 3: {7}; q = 5: {11, 31, 41}; q = 7: {127, 337}; q = 11: {23, 89, 683, …}**. These are the analogue of CHAT's case 3, and they are handled per q by the exceptional-prime door pair (as p = 7 is for q = 3).
- **Exceptional q**: primes q where the gearbox hypothesis v_q(2^M − 1) = 1 fails — exactly the **Wieferich primes** (1093 and 3511 below 5000). These break the *structure* of the gearbox, not just one case. **Wieferich primes are not ruled out [Derived].** Only the congruence for primes below 100 has been tested, which says nothing about 1093 or 3511 (§12.1, item 10).

**Zsigmondy: the a ≥ 2 exceptions are infinite [Derived].** The set of primes whose multiplier has order a *power* of q is **infinite** (for q = 3: 7; then 19, 73; then 87211, 262657; and so on). Only the a = 1 case is finite. The a ≥ 2 primes are exactly what the blur trick (case 2) handles, which is why case 2 is not a corner case but the generic one at depth.

**The blur trick should generalise to any q [Derived, unverified in a full construction].** If two doors have the same multiplier and their sources differ by exactly q^(a+1), the difference of their images is μ·(difference)/q, which has q-adic valuation exactly a. The multipliers agree automatically once a ≥ 2. A short derivation; never checked inside a complete general-q construction, which is the purpose of the general-q build in §6, item 6.

**Why 7x+1 and 17x+1 may behave differently [Derived].** 2 is not a primitive root mod 7 or mod 17 (they are the first two primes where that fails), so the ring may split and the "good ground" must be restricted. Concretely, for q = 7 no padding exists for half the residues, since 2 has order 3 mod 7. Raw coverage showed no exceptions there, so any difference would be in the *method*, not the conclusion.

**Primes where 2 is not a primitive root, below 100 [Observed]:** 7, 17, 23, 31, 41, 43, 47, 71, 73, 79, 89, 97 (all ≡ ±1 mod 8 except 43). That the first two are 7 = 2³ − 1 and 17 = 2⁴ + 1 is a coincidence: the next, 23, is neither.

**Order-of-4 corrections [Observed; Auxiliary Report E5].** The multiplicative order of 4 is 3 at p = 7, 9 at p = 19 and p = 73, and 81 at p = 163 and p = 2593; the claim that 2593 and 717121 have order 27 is wrong. Since Φ₂₇(4) = 3·262657·87211, the primes of order 27 are exactly 262657 and 87211 (matching the Zsigmondy list above). The order of 4 at 717121 is 264, not a power of 3. For all primes 5 ≤ p < 6000, the index [𝔽_p* : ⟨2, 3⟩] is even iff p ≡ ±1 mod 24, and divisible by 3 iff p ≡ 1 mod 3 and both 2 and 3 are cubes; the first primes with index divisible by 3 are 307, 439, 499, 643, 727, 919 (223, an earlier candidate, fails: 3⁷⁴ ≡ 183 mod 223). At p = 307 the slopes satisfy 3¹⁰² ≡ 2¹⁰² ≡ 1 mod 307, and |⟨2, 3⟩| = 102 (see §9.6).

**Exceptional-p data for 5x+1 [Observed; Tangent — kept so it is not recomputed].** The order-of-a-power-of-5 primes below 100 are 31 (ord 2 = 5) and 11 (ord 4 = 5); the primes with a proper ⟨2, 5⟩ are 31, 41, 71, 79, 89. Trivial exceptions at p = 2, 3, 5.

**An open question with no answer yet [Parked; see §13, item 26].** An unidentified class of primes was once said to yield "master keys" for 5x+1. No definition survives. It is not the Mersenne primes and not the Wieferich primes as tested, and it may be an order-based class. Parked until the general-q machinery (§6, item 6) produces candidates, so that the class can be defined from computed data rather than from recollection.

**Single-cycle on U_K, independently (Auxiliary Report E3) [Observed].** Iterating c ↦ R(c) mod 3^(K+1) from 8 returns to 8 after exactly 2·3^(K−1) steps for K = 1, …, 6 (lengths 2, 6, 18, 54, 162, 486), the number of valid classes. This confirms §2.2's single-cycle statement independently of the formal proof; it does not change the "not formalised" status recorded in §11.2 (transitivity is formal, the single-cycle corollary is [Derived from Held facts]).

---

## 10. Failed and superseded approaches [Core — tested and failed, which is exactly why it stays]

Each entry records the exact inference that broke, whether the failure is fundamental, and what survived. An attempt is listed only if it shows an obstacle that could be hit again.

**10.1 The naive finite-state proof.** *Idea:* follow a number through (ending mod 27, residue mod p), a state space of exactly 6p states, take left turns by default, and inject right turns to escape cycles. *Why it looked fine:* the Chinese remainder theorem makes the state space a product, and the offsets keep 0 from being absorbing. *Where it breaks: three separate inferences.* (a) The state is not an autonomous factor of the tree: 8 and 35 both end in 8 mod 27, with the same multiplier, but L(8) = 20 and L(35) = 92 ≡ 11 mod 27; the next ending needs x mod 81. This is fundamental to the quotient by 27. (b) "The affine maps act transitively on 𝔽_p" is the very claim to be proved. (c) Finiteness bounds when a *state* repeats, not when all residues have been visited. *What survived:* the tower of precisions, which is the cost of each left turn made explicit; the discipline that the model is a **relation**, sound from real to model (every real step is a model step) and not from model to real (a model path need not come from any integer), which the ratchet then repairs on fibres; and the fact g(0) = −μ/6 ≠ 0 used in §2.7. **Warning:** the six-state matrix of §3 and the 6p-state chain of §9.3 are this model again, kept honestly as a *heuristic* whose assumption (the lost digit is free) is not proved. What supports it is §9.4, not the model. A small Lean statement of the failure, `¬ ∃ f, ∀ x, InG x → L x % 27 = f (x % 27)`, with witnesses 8 and 35, was proposed and is not in the current file [proposed, uncompiled; now §6, item 5].

**10.2 Return-path proofs of the translation.** *Idea:* build explicit closed walks (right path to a station, a door, a right path back) and compare labels. *Fragile points:* the bookkeeping must use the **accumulated exponent** (1 or 2 per right edge), not the number of edges; at K = a the shortest path from 8 to the second door's source is empty (increment 0, not 3^a), so a claim "N steps, increment 3^a" holds only if the second source is *defined* as an iterate of R from 8; and the number of case splits. *What survived:* every case still stands as an independent route and the commutator lemma is the formal proof's engine. §2.4 and §2.7 are the cleaner account.

**10.3 Edge-level fixed points.** Individual labels have no common fixed residue (4t = t forces t = 0; every door sends 0 to −μ/6), but this says nothing about Hol(K) and is useless: a common fixed point of all edge labels would give one for Hol(K), while the converse fails. Only closed walks at 8 map a single set to itself.

**10.4 Overdetermination from several lifts of one station.** At K = 1 a station has three doors with different labels, and at p = 7 demanding a common height gives a contradiction (κ = 2 and κ = 6). *Where it fails as a method:* for K > a the lifts of a station no longer disagree in a way that forces anything. A related attempt through Weil bounds and overlap estimates was reported as overstated; its text is lost. *What survived:* the useful pair of doors is one that is **height-equal but target-distinct**, not any two doors.

**10.5 Never-Seven (predecessor route) [Parked — retired from the critical path, retained as an optional Diophantine conjecture].** The predecessor route reached the translation through a Diophantine condition: for a ≥ 2 let j_a be the unique j below 3^a with 4^j ≡ 7 mod 3^(a+1), call a prime q *relevant* if the order of 2 mod q is exactly 2·3^a (equivalently, ord of 4 is 3^a), and conjecture 4^(j_a) ≢ 7 mod q for every relevant q (equivalently gcd(4^j − 7, 2^(2·3^(a−1)) − 2^(3^(a−1)) + 1) = 3; the equivalence has not been checked). A counterexample would only show that *one particular pair of walks* shares a fixed point; it would not make Hol(K) have one. The current proof never mentions 7 or 4^j. Caution about a tempting misreading: 4⁸ − 7 = 65529 = 3⁴·809 looks like a collision for a = 2, but 809 is not relevant (reportedly ord₈₀₉(2) = 404, unchecked here), and the relevant prime for a = 2 is 19, where 4⁸ ≡ 5. Any formal use must state the prime is ≥ 5, because the definition degenerates at 2 and 3.

**10.6 Testing stabilisation on truncated trees.** A depth-truncated tree violates "equal fibre sizes" and "stable sizes" for boring reasons (not enough descendants yet), so it can neither confirm nor refute them. The meaningful test is fidelity of single steps on real integers (§9.9). *(The seed-128 depth-20 check of §9.9 lands here: consistent, but confirmatory of nothing.)*

**10.7 Two misstatements to avoid.** (a) "The case κ = 0 is killed by the ring-lap case." Wrong: when the lap multiplier is 1 that case does not apply; κ = 0 is killed directly by the door offset −μ/6 ≠ 0. (b) "The two-door contradiction at p = 7 is an accident of K = 1." It holds for every K ≥ 1 in height form, because the two doors, at different stations when K ≥ 2, both sit on stations ≡ 2 mod 9 and carry the same height.

**10.8 Two more critiqued routes, recorded so their refutations are not re-derived [Derived critiques; Tangent].**

- *The "Lead 3" averaged-operator run:* its per-prime values varied from 0.93 down to 0.73 across primes, so it was never asymptotic, and it used equal weights rather than the true (2,1,2,1,2,1)/9. Only the rerun numbers belong in §9.8, and they are there under the name λ₂.
- *The "Lead 4" second-difference route:* the padding depends only on x mod ℓ, so a "second difference mod ℓ²" is arbitrary, and the slide's nonvanishing does not come from padding being non-affine. The route as stated proves nothing; do not resurrect it without repairing these two points.

**10.9 The 1/2 coordinate shift [Core — tested and failed].** *Idea:* diagonalise L by translating the origin by 1/2, since L(y + 1/2) = (μ/3)·y. *Why it looked fine:* this makes the left branch a pure dilation (up to the origin of the output), which would kill the additive part of the label and might simplify the holonomy calculation. *Where it breaks:* the right branch does not preserve the same origin shift. R(y + 1/2) − 1/2 = 4^s·y + (4^s − 1)/2, which is not 4^s·y for any s ≥ 1, so the shift injects an additive translation into R and destroys the pure-dilation cyclic structure of §2.2's R-cycle. *What survived:* the R-cycle on U_K and its accumulated exponent E (Lean `rExp`), which is what the proof actually uses; the shift is not needed and would be fatal.

**10.10 The naive characteristic growth equation [Core — tested and failed].** *Idea:* model the growth rate of the descendant tree by the Moran-type equation Σ b_i^−δ = 1 over the branch scaling ratios (1/4, 1/16, μ/3). *Why it looked fine:* it is the standard equation for a self-similar set, and the branch ratios are explicit. *Where it breaks:* L expands in the archimedean metric while R contracts, and the two branches act across incompatible local completions (ℚ₂ for the doubling, ℚ₃ for the division by 3). A one-variable real equation cannot encode both places simultaneously; the correct object is the adelic sum in §4, which vanishes identically rather than selecting a δ. *What survived:* the adelic Lyapunov balance in §4, which is the right formulation, and the per-place exponents that give §5 A1's side note.

**10.11 Prime enrichment in D(v) [Core — tested and failed].** *Idea:* an observed statistical excess of primes among the descendants in D(v), compared with random integers of the same size. *Why it looked fine:* the excess was large and consistent over many seeds. *Where it breaks:* the baseline was wrong. Descendants in D(v) are odd and coprime to 3 by construction, so the correct comparison class is (ℤ/6ℤ)*, not all integers. Against that baseline the excess vanishes. *What survived:* nothing. The observation was a comparison artefact, recorded here so it is not re-derived.

---

## 11. Verification status, conventions and working method [Core]

**11.1 What the Lean files establish** (compiled by the authors; the main file contains no `sorry`; the statements were read against this document and not recompiled here):

- the Main Theorem for every prime (`main_theorem_infinite`) and Full Dials for p ≥ 5 (`main_theorem_core`);
- the bridge, as three proved statements and not an assumption: stabilisation (`stabilization`), the no-loss-after-stabilisation equality (`fibre_succ_eq`), and bijective action of edges and walks (`edge_image`, `walk_image`);
- the translation (`translation_exists`) through the dilation cases (`dilation_exists`), the twist (`ref_walk`) and the commutator identity (`comm_label`);
- ring transitivity and the lap at 8 (`R_transitive`, `R_full_loop`), lifting-the-exponent (`lte_four`), soundness of the skeleton (`Desc_reaches`), and the small primes;
- separately, the six-state matrix facts (characteristic polynomial, A³ = 3A², trace of Aⁿ).

**11.2 What is not formalised** (argued by hand, derived, or observed only): the frozen-height equivalence and its case proof (§2.7); that the right-edge map is a single cycle (follows from transitivity plus injectivity); the composite-moduli and prime-power extension (A3, A4); the explicit key −336 (§9.7, a short computation); left-only generation of translations and Hol(K) = ℤ/p ⋊ ⟨2, 3⟩ (§9); the spectral gap; everything about qx+1.

**11.3 Names: older notation ↔ current.** Stations U_K, base vertex b = 8 ↔ good shadows, station 8. Right edge c ↦ ρ(c) — the class map ρ(c) = R(c) mod 3^(K+1), Lean `rnext`, with multiplier 4^step(c) (Lean `rStep`) and clock label `rightLabel`. Left edge indexed by a finer ending e ↔ a door, `Edge.left`, `lnext`, `leftLabel`. Fibre F_K(c) ↔ `fibre`. Hol(K) ↔ `Hol` (the set of labels of closed walks at 8). Precision lemma ↔ `L_cong`. Closure ↔ `R_mem`, `L_mem`, `Desc_mem`. Single-cycle lemma ↔ `R_transitive` with `R_full_loop`. Theorem A ↔ `main_theorem_core`; Theorem B ↔ `translation_exists`. The multiplier is `mult`, and exactness of the division is the lemma `L_spec`. **Caution:** earlier editions of this table used ρ for the per-edge multiplier 4 or 16; the formal proof's ρ is the class map, and that convention is used throughout this document. The full classical-to-Lean table lives in the paper's Appendix and is canonical.

**11.4 Lean pitfalls worth remembering.** Natural-number division rounds down and subtraction truncates at zero, so "L is exact on good numbers" and "2x − 1 does not truncate" are lemmas, not properties of the definition. The prime p is an explicit argument of `Walk`, `Hol` and `fibre`. Division in `ZMod p` is multiplication by an inverse and is meaningful only for p ≥ 5 here. No translation between the original odd-number coordinates and the skeleton's coordinates is needed anywhere: `Desc_reaches` states reachability in the original Collatz map directly.

**11.5 Working method (kept short).** Three questions are different and none implies the next: is the code well-formed, is the statement true, and is it faithful to what the prose claims. A flawless proof of a different statement is a failure. Computation tests definitions and single steps, never a universal statement. A sufficient condition is not a necessary one, and a property of fibres is not a property of the graph. Whenever something fails, find the exact inference and record what survived. Never confuse an equality of values of a hypothetical height function with an equality of integers modulo p. **Project documents contain no flattery or atmospheric filler (for example "put your feet up" or name-dropping paragraphs); pasted material containing it is summarised, not quoted** [Irrelevant as mathematics; retained as a one-line policy so the judgement is not re-argued].

---

## 12. Convention alignment to the formalization account [Core]

*This section logs the reconciliation of this document against Inverse Collatz Trees and Simultaneous Density, whose conventions and Appendix name table are canonical.*

### 12.1 Fixes applied and claims retired

Items 1–7 reconcile earlier text with the formal conventions. Items 8–15 are earlier claims that were wrong or overstated and are retired; the body states only the corrected versions.

1. **ρ clash (§11.3, formerly "Right edge c ↦ ρ(c)·c").** The old table used ρ for the per-edge multiplier (4 or 16). The formal proof's ρ is the **class map** `ρ(c) = R(c) mod 3^(K+1)` (`rnext`); the multiplier is `4^{step(c)}` (`rStep`). §11.3 now states this and carries a caution; ρ means the class map everywhere in this document.

2. **L's definition (§2.1).** The old text led with the padding form L(x) = 2^i·m. The formal proof's canonical object is the **multiplier**: L(x) = μ(x)(2x−1)/6 with μ(x) ∈ {2,4,8,16} depending only on x mod 27 (μ = 2·2^i). §2.1 now leads with μ; the padding form follows as a corollary.

3. **"Padding factor" renamed (§2.3).** μ is now called the **multiplier** (Lean `mult`), matching the formal proof, not "the door's padding factor".

4. **w > 0 restored (§1, §2.1, §7).** The formal Theorem 1 hypothesises w > 0; the statements here now say "positive integer w".

5. **Ring notation introduced (§2.2).** ρ, ρ^n, step(c) and E(n,c) are now named explicitly (`rnext`, `rIter`, `rStep`, `rExp`), since §2.4's zoom bookkeeping implicitly used the accumulated exponent.

6. **Composition convention for affine maps.** The formal account composes "first f, then g" as f·g = (g₁f₁, g₁f₂ + g₂), with commutator f⁻¹g⁻¹fg in this product. The earlier "W₄, W₅" values −135/256, −513/512 (§13 item 28) came from an extra factor 1/(αμ) under a different convention; the correct values are 5/9 and 19/27, and §9.7 records the reconciliation. No other use of the composition convention in this document is affected.

7. **Drift residues at level 3.** Older notes gave the residues x₄ and x₅ (for the loops W₄ and W₅) as 47 and 26 mod 81. These are false: x₄ ≡ x₅ ≡ 56 mod 81. The correct values, and the level-5 and level-6 separations, are recorded in §9.9.

8. **"Hol(K) is the full affine group" (A2, §9.9).** Retired. It held only at small primes where ⟨2, 3⟩ is everything (5, 7, 13, 19); the correct conjecture is Hol(K) = ℤ/p ⋊ ⟨2, 3⟩, and p = 23 is the first counterexample.

9. **"Composite moduli need a joint zoom" (A3).** Retired. Slopes commute, so commutators are pure translations and the single-prime argument lifts.

10. **Wieferich primes "ruled out" (§9.10).** Retired. Only the congruence for primes below 100 had been tested.

11. **"Finitely many exceptional primes per q" (§9.10).** Retired as stated. Only the order-exactly-q case is finite; primes whose multiplier has order a higher power of q form an infinite set (Zsigmondy).

12. **"Every residue is visited in O(log p) steps" (A1).** Retired. Visiting every residue is a coupon-collector event needing about p·log p steps; only equidistribution at a given step is fast.

13. **Per-prime "ρ" values 0.93 to 0.73 (§9.8, §10.8).** Retired: equal weights were used instead of the true (2, 1, 2, 1, 2, 1)/9, and the values were never an asymptotic. The reruns are called λ₂.

14. **"Conway undecidability is a reason to avoid 5x+1" (B1).** Retired. The result concerns generalised periodic-rule Collatz functions as a class, not each qx+1.

15. **"The commutator argument needs no inverses" (§2.4).** Retired. The walk traverses edges backward and uses inverses of labels; forward-only generation is [Observed] (§9.2), not proved.

### 12.2 Checked and found consistent (no change)

- **Multiplier table.** x mod 27 ∈ {2,8,11,17,20,26} → μ = 4,8,16,2,4,2; equivalently padding i = 1,2,3,0,1,0. The old §2.1 i-table and the formal μ-table agree term by term (μ = 2·2^i).
- **Labels.** Right (4,0)/(16,0); left (μ/3, −μ/6) — matches `leftLabel`.
- **Commutator order.** (α,β)⁻¹, (λ,0)⁻¹, (α,β), (λ,0) = (1, (λ−1)β): old prose and formal Lemma 9.1 state the same composition.
- **Depth-1 numbers.** Twist (32/3, −16/3), zoom (64,0), slide −336 (§9.7); recomputed from the formal definitions, including the p = 7 exception with doors 2 and 11, λ = μ(11)/μ(2) = 4, slide −16 ≡ 5.
- **Witness pair 8/35 (§10.1).** Both ≡ 8 mod 27, same multiplier, L(8) = 20, L(35) = 92 ≡ 11 mod 27 — agrees with the formal L and μ, and with case 2's e₂ = 8 + 3^(a+1) at a = 2.
- **Six-state matrix (§3).** Target classes recomputed from the formal μ: doors from 2, 8, 11, 17 land ≡ 2 mod 9, doors from 20, 26 land ≡ 8 mod 9, giving rows (1,0,1,0,1,0) / (0,1,0,1,0,1) and stationary weights (2,1,2,1,2,1)/9 as claimed.
- **Depth convention.** Class = valid (c ≡ 2 or 8 mod 9) and bounded (c < 3^(K+1)), K ≥ 1 — matches "shadow at depth K". Station count 2·3^(K−1), lap winding 3^K, lift 8 + 3^(a+1): all match Lemmas 4.1, 4.3 and Case 2.
- **Lean names (§11.3).** Every name re-checked against the paper's Appendix: `Desc_reaches`, `L_spec`, `L_cong`, `stabilization`, `fibre_succ_eq`, `edge_image`, `walk_image`, `translation_exists`, `dilation_exists`, `ref_walk`, `comm_label`, `R_transitive`, `R_full_loop`, `lte_four`, `main_theorem_core`, `main_theorem_infinite` — all present and correctly mapped. (A full re-audit of the paper's table against the current Lean file remains a to-do, §6 item 8.)

### 12.3 Notation policy for companion material

The former CHAT-unaddressed-points log, in its item 24, used ρ for a spectral quantity ("ρ varied from 0.93 to 0.73"). That clashes with the formal ρ (the class map). The value is now called **λ₂** ("the second eigenvalue") in §9.8, and the rename is standing policy for all future write-ups.

### 12.4 Standing rule

Where any companion document (exposition, unaddressed-points log, future drafts) disagrees with the formalization-faithful account on names, symbols, or the shape of a statement, the formal account wins, and the disagreement is logged here before it is forgotten.

---

## 13. Disposition of the former CHAT-unaddressed-points log [Core as a ledger; the log itself is retired]

The 30-item standalone log has been folded into this edition, and its inline "formerly item N" tags have been removed from the body; the "Where it went" column below is the index. Item-by-item disposition, with the evidence and relevance flags assigned on intake:

| Items | Content | Where it went | Flags on intake |
|---|---|---|---|
| 1 | Spectral-gap proof sketch (proposed) | §6, item 3 (owed, never started) | [Belief] [Core-for-A1] |
| 2 | Larger-p and 5x+1 gap tests (proposed) | §6, item 7 | lead |
| 3 | General-q machinery, q = 5, 7, 17 (proposed) | §6, item 6 | lead [Core-for-B1] |
| 4 | Formalise the leak in Lean (proposed) | §6, item 5; §10.1; §3 | lead, cheap |
| 5 | LaTeX paper references (proposed) | §6, item 8; A6, A7 | housekeeping |
| 6 | Re-audit the paper's Lean-name table (proposed) | §6, item 8; §12.2 | housekeeping |
| 7 | Raw qx+1 probes, no missing residues | §9.10 | [Observed] [Core-for-B1] |
| 8 | Two kinds of exception (exceptional p finite per q; exceptional q = Wieferich) | §9.10 | [Observed]/[Derived] [Core-for-B1] |
| 9 | Zsigmondy correction (a ≥ 2 infinite) | §9.10 (extended with order-of-4 corrections, Auxiliary Report E5) | [Derived] [Core-for-B1] |
| 10 | Blur trick generalises to any q | §9.10 | [Derived, unverified] [Core-for-B1] |
| 11 | 7x+1 / 17x+1 ring-split worry | §9.10 | [Derived] [Core-for-B1] |
| 12 | Primes < 100 where 2 is not a primitive root | §9.10 | [Observed] [Tangent — lookup data] |
| 13 | 5x+1 exceptional-p data below 100 | §9.10 | [Observed] [Tangent — kept so it is not recomputed] |
| 13a | Auxiliary Report corrections (E1, E5, E7, E8, E9) | §9.7, §9.9, §9.10, §9.6, §12.1 | [Observed] [Core — resolves item 28, extends item 9, bounds the E9 gaps] |
| 14 | Wieferich "ruled out" was only p < 100 | §9.10 (correction recorded) | [Derived, correction] |
| 15 | Truncated-tree fibre check (seed 128, depth 20) | §9.9, third bullet; §10.6 | [Observed] [Tangent — confirmatory of nothing] |
| 16 | w = 1 converse asymmetry | §1 | [Derived] [Tangent] |
| 17 | Theorem needs no large ⟨2,3⟩; only the gap does | §2.3; A2; §9.8 | [Derived] [Core — a blunt separation] |
| 18 | The ratchet ≠ spectral expansion | §2.5 ("What the ratchet is not") | [Derived] [Core — a recurring over-reading] |
| 19 | The commutator uses inverses (as written) | §2.4 (caution) | [Derived] [Core — corrects a loose sentence] |
| 20 | No Artin wall in the proof | §2.6 | [Derived] [Core] |
| 21 | Conway undecidability ≠ per-qx+1 | B1 | [Derived] [Core — unblocks B1] |
| 22 | Lindenstrauss–Varjú citation mismatch | §4 external homes; A7 | [Memory, suspect] [Tangent] |
| 23 | A5's real obstruction (digits must match doors) | A5 | [Derived] [Core for A5] |
| 24 | Lead 3 / Lead 4 critiques; ρ renamed λ₂ | §10.8; §9.8; §12.3 | [Derived, critique] [Tangent — tombstone for two dead routes] |
| 25 | Flattery/atmosphere passages | §11.5 (one-line policy) | [Irrelevant as mathematics; kept as policy] |
| 26 | The unnamed "master key" prime class | §9.10 (Parked) | [Parked] — unresolved; name it from data, not memory |
| 27 | Does the −336 key generalise? | §9.7 (open note) | open question [Tangent until attempted] |
| 28 | "W₄, W₅" shortcut (d₄ = −135/256, d₅ = −513/512) | Resolved, §9.7: artefact of an extra 1/(αμ) factor in composition; correct commutators are 5/9 and 19/27 (Auxiliary Report E1) | [Irrelevant] — tombstone, not a lead |
| 29 | Exposition staleness | §8 | housekeeping |
| 30 | Paper's Prop 4.3 remark not formalised | §8 | housekeeping |

**The retired log is superseded by this table.** Any later reference to "the unaddressed points" resolves here: every item is folded in at the listed place, parked with a reason, or tombstoned as irrelevant.

---

*Unverified throughout: the frozen-height proof (§2.7, only checked by solver), the uniform-in-p statements in §9 (observed on finitely many primes, not proved); the Goursat-style reduction in A3; any use of λ_∞ to bound a spectral gap (the adelic balance Σλ_v = 0 is proved in §4, and the exact rate is stated in §5, A1; what remains unverified is using these to bound the gap); all literature recalled from memory. Everything tagged [Derived], [Belief] or [Memory] is a lead, not a fact. The Auxiliary Report (*CHAT Auxiliary: Experimental Checks (Revised)*) is the primary source for the independently reproduced items tagged in §9.6, §9.7, §9.9, §9.10 and §12.1; its §5 lists claims not re-run, and its §6 states the limits. Where a correction from that report has been folded in, the Bible line cites it by experiment number.*
