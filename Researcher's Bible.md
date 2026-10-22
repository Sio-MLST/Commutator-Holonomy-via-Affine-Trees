# Researcher's Bible
### A condensed proof you can hold in your head, a map of the surrounding country, and an honest list of where the dragons live

*For a reader who already knows what Collatz is, what 3-adic digits are, and what a commutator does, and who wants the structure of the proof without the scaffolding.*

**Status of this document.** This is the single authoritative project document. The earlier "working edition" of the Bible is retired: what was worth keeping from it (the failed approaches with their failure conditions, the frozen-height formulation, the working method) is in §2.7, §10 and §11, reconciled against the current Lean files. Where an older statement disagreed with the Lean files, the Lean files won. **Notation follows the formalization-faithful account (*Inverse Collatz Trees and Simultaneous Density*): its Appendix is the canonical name table, and this document has been reconciled to it; every change made in that reconciliation is logged in §12.** The former standalone CHAT-unaddressed-points log (30 items) has been folded in throughout this edition; its disposition is itemised in §13.

**Proofread edition note.** Light proofreading for consistency of conventions (ρ = class map `rnext`; μ = multiplier `mult`; spectral second eigenvalue = λ₂; classes valid + bounded), tag accuracy against the Lean statements, and removal of residual phrasing friction. No mathematical content has been altered. The formal account remains the winner on every conflict.

**Tags.** Two axes, and both may appear on one item.

*Evidence — where does it come from, how much is behind it:*
- **[Held]**: proved (the formal Lean 4 development, or a short derivation from proved facts).
- **[Observed]**: exact computation on specific inputs; evidence, never a proof of a universal statement.
- **[Derived]**: worked out by hand, unchecked.
- **[Belief]**: judgment.
- **[Memory]**: literature recalled from memory, to verify before citing.
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

**The question CHAT answers.** Is the ancestor tree of w blind to some residue class modulo some prime p? That would be a local obstruction, and it is the first thing that would have to be ruled out before the conjecture (the ancestor tree of 1 is everything) could be true.

**The theorem [Held, Core].**

> **The Tree Has No Blind Spots.** For every prime p, every positive integer w with 3 ∤ w, every residue r mod p and every bound B, some n > B reaches w and has n ≡ r (mod p).
>
> **Full Dials** (p ≥ 5). Inside the two-branch skeleton D(v) of the tree, every good 3-adic ending of every length occurs together with every residue mod p.

**What it is not.** It is not the conjecture. It proves nothing about natural density, nothing about covering all integers, and no size bounds. Reading "every ending with every residue" as "most numbers" is the error to avoid. We return to this in §4.

**Sanity example.** Take w = 1 and p = 7. Pure doubling reaches only the residues 1, 2, 4. The theorem says the tripling-undoing branches fill in the other four. Concretely, 1024604, 1024724, 1024256, 1024544, 1024454, 1024805, 1024841 all reach 1 and cover the seven residues 0 to 6 respectively [Held as instances of the theorem, found by search]. Independently recomputed afterwards: each of the seven reaches 1 in exactly 51 steps (standard step count) and leaves the residues 0 to 6 in the order listed. The pattern for arbitrary B is the theorem's content, not this finite check's.

**The hypothesis 3 ∤ w is sharp [Held, elementary].** An odd step lands on 1 mod 3, so a multiple of 3 can only be reached by halving. The ancestors of a multiple of 3 are exactly its doublings.

**A converse worth recording [Derived, Tangent].** A true Collatz conjecture would give the theorem for w = 1 for free (every integer is an ancestor of 1, so every class is covered); the theorem gives no route back from w = 1 to w ≠ 1. The asymmetry is obvious but worth one sentence in any exposition, because it pre-empts the misreading "CHAT is 1/Collatz of the way there". *(Formerly unaddressed-points item 16.)*

---

## 2. The proof, condensed

### 2.1 The skeleton [Core]

Call a number **good** if it is at least 8 and ends in 02 or 22 in base 3 (equivalently, it is 2 or 8 mod 9; the formal proof's set is G = {x ≥ 8 : x ≡ 2 or 8 mod 9}). Define two maps on good numbers:

- **R(x)** = 4x if x ≡ 2 mod 9, else 16x. Both are the *fewest doublings that land back on good ground*: from a number ending 02, ×4; from one ending 22, ×16, since ×2, ×4, ×8 all fail.
- **L(x)** undoes a tripling. Following the formal proof, write
  **L(x) = μ(x)·(2x − 1)/6**,
  where the **multiplier** μ(x) depends only on x mod 27: x mod 27 ∈ {2, 8, 11, 17, 20, 26} gives μ = 4, 8, 16, 2, 4, 2. Writing μ = 2·2^i, this says L(x) = 2^i·m with m = (2x − 1)/3 an odd integer (when x ≡ 2 mod 3) satisfying 3m + 1 = 2x, and i the fewest doublings landing on a good residue. The multiplier depends only on x mod 27, because m's last two digits are decided by x's last three: x mod 27 ∈ {2, 8, 11, 17, 20, 26} gives i = 1, 2, 3, 0, 1, 0.

Example: L(8) = μ(8)·15/6 = 8·15/6 = 20, with forward trip 20 → 10 → 5 → 16 → 8.

Every number in the skeleton D(v) (everything obtained from a good seed v by R and L) really reaches v **[Held]**. And every positive w with 3 ∤ w has a good ancestor v obtained by multiplying by a power of 2 between 2⁶ and 2⁹ **[Held]**. So the theorem reduces to a statement about this thin two-branch tree.

### 2.2 The 3-adic side: a tower of rings with doors [Core]

Keep only the last K+1 base-3 digits: a **shadow at depth K** (the formal proof's *class at depth K*: a valid c, i.e. c ≡ 2 or 8 mod 9, with c < 3^(K+1); K ≥ 1). The good shadows are **stations**; there are 2·3^(K−1) of them.

**The ring.** The ring map is **ρ(c) = R(c) mod 3^(K+1)** (Lean `rnext`), with iterates ρ^n (Lean `rIter`) and the **accumulated exponent** E(n, c) (Lean `rExp`), defined by R(c) = c·4^step(c) with step(c) = 1 if c ≡ 2 mod 9, else 2 (Lean `rStep`). ρ acts on stations as an invertible map and is transitive **[Held]**, hence a single cycle of length 2·3^(K−1) **[Derived from Held facts; confirmed by direct computation to K = 6]**. At depth 2 it reads 022 → 202 → 222 → 102 → 122 → 002 → back.

**The gearbox [Held].** Name each station by the exponent E with station = 2·4^E (E ranging over 0, 1, 3, 4, 6, 7, … below 3^K; equivalently E ≡ 0 or 1 mod 3, with E ≡ 0 exactly on stations ≡ 2 mod 9). A ring step adds 1 or 2 to E. The dictionary between winding and digits is lifting-the-exponent: v₃(4^m − 1) = 1 + v₃(m). Equivalently, 4^m ≡ 1 mod 3^(K+1) exactly when 3^K | m. A full lap from station 8 winds by exactly 3^K, which is why a lap is a stretch by 4^(3^K).

**The doors [Held].** L reads one more digit than it keeps. If x is known mod 3^(K+2), then L(x) is known mod 3^(K+1): at least K+1 digits survive. So a left turn is a **door** selected by one digit beyond the station's own. Each station has three doors, and each door opens onto a station of the same depth. The ring at depth K+1 is a triple cover of the ring at depth K, and doors run from the upper floor to the lower one. That stacking is the *tower*.

### 2.3 The mod-p side: an affine cocycle, fibres, holonomy [Core]

Put a clock with p positions on every station. Every road carries an affine rule on the clock:

- right edge: t ↦ 4t or 16t;
- left edge: t ↦ (μ/3)·t − μ/6, where μ ∈ {2, 4, 8, 16} is the multiplier μ(x) of the left branch, as in §2.1 (μ = 2·2^i; Lean `mult`).

This needs 2, 3, 6 invertible, so p ≥ 5; the primes 2 and 3 are handled separately in §2.6. The rule depends only on the road, never on which number travels it. Check with p = 5: the loop 8 → L → 20 → R → 80 acts as t ↦ 4t + 3, and 8, 35, 62 (all ≡ 8 mod 27) map to residues 0, 3, 1 from 3, 0, 2 respectively.

A **fibre** F_K(c) is the set of residues mod p realised by skeleton members with shadow c at depth K. **Holonomy** Hol(K) is the group of rules obtained by composing the labels of closed walks at station 8, forward or backward. It is a subgroup of the affine group of the clock.

**A structural fact that is easy to miss [Derived].** Every slope in Hol(K) lies in the multiplicative group ⟨2, 3⟩ ⊆ (ℤ/p)*. Right labels have slope a power of 4; left labels have slope μ/3 with μ a power of 2. So Hol(K) ⊆ (translations) ⋊ ⟨2, 3⟩, a proper subgroup of the full affine group whenever ⟨2, 3⟩ is proper (p = 23 is the first such prime; up to 200 they are 23, 47, 71, 73, 97, 167, 191, 193 [Observed by direct computation]). **The theorem itself does not need ⟨2, 3⟩ to be large [Derived; the blunt separation matters]: CHAT needs only one zoom λ ≠ 1 and one translation; only the quantitative spectral gap needs |⟨2, 3⟩| ≥ p^δ (see §9.8).** The ceiling matters a great deal for anything quantitative (§5).

### 2.4 The engine: zoom, twist, commutator [Core]

Two elements of Hol(K), then one commutator.

**The twist [Held].** Leave station 8 through its door at 8 mod 27, land at some station, ride the ring back to 8. The label is x ↦ αx + β with α ≠ 0 and β = −4^E·(8/6) ≠ 0. For p = 5 this is the loop above: t ↦ 4t + 3.

**The zoom [Held].** A pure stretch λ ∉ {0, 1}. There are three cases, exhaustive:

1. **The lap is visible.** If 4^(3^K) ≠ 1 mod p, a full lap around the ring is a pure stretch by it. (p = 5, K = 1: 4³ ≡ 4.)
2. **The lap is invisible, and ord(4) = 3^a with a ≥ 2.** The dial cannot feel ring distances divisible by 3^a. Take two doors on the same wall whose endings agree to a+1 digits (8 and 8 + 3^(a+1)); both have the same multiplier, so their twists cancel, and the ring distance between *their sources* is invisible to the dial. But each door loses a digit, so their *landing* stations agree only to a digits. The ring distance between the landings is not a multiple of 3^a, and the dial feels it. Walk out through one door, back through the other, and what survives is a pure stretch.
   *Example, p = 19, depth 2:* the doors with endings 8 and 35 are the same station; they land on 20 and 92 ≡ 11 mod 27; riding from 20 to 11 winds by 3 and 4³ = 64 ≡ 7 ≠ 1 mod 19, so the loop is t ↦ 11t.
   **This is the conceptual centre of the proof: the digit a door forgets is exactly what makes the dial feel something.**
3. **The single exceptional prime.** If ord(4) = 3 then p | 63, so p = 7. Both ring distances are invisible, but the doors with endings 2 and 11 have different multipliers (4 versus 16), giving a stretch of 16/4 = 4.

Because the depth at which the ratchet below settles cannot be controlled, all three cases must be handled at *every* depth K, and they are: as K grows, a prime can pass from case 1 to case 2 **[Held]**.

**The commutator [Held].** Undo the twist, undo the zoom, redo the twist, redo the zoom: (α,β)⁻¹ then (λ,0)⁻¹ then (α,β) then (λ,0). The stretch α cancels (stretches commute) and what remains is a pure slide by (λ − 1)β ≠ 0. Run on t it reads (t − β)/α → (t − β)/(αλ) → (t − β)/λ + β → t + (λ − 1)β. For p = 5, with twist 4t + 3 and zoom 4t, it gives t ↦ t + 4; for p = 7, a slide by 5.

**A caution about inverses [Derived; corrects an earlier loose sentence].** This walk as written traverses edges backward and so uses inverses of labels. "No inverses needed" is wrong as a description of this argument; it becomes true only if forward-only generation is proved, which is [Observed] (§9.2), not proved. *(Formerly unaddressed-points item 19.)*

### 2.5 The ratchet and the unsquint [Core]

Now convert one slide into every residue everywhere.

1. **Sizes are constant around a ring [Held].** A right edge acts on fibres injectively (its slope is a unit), so fibre size never decreases along the ring; around a cycle, that forces equality. All stations at one depth have fibres of one size.
2. **Sizes can only shrink with depth [Held].** A deeper shadow is finer, so its fibre sits inside the fibre below it. Sizes are integers ≥ 1 (the seed itself is a witness). So they stop falling: there are K₀ and σ with every fibre at depth ≥ K₀ of size σ. This is a pure existence argument; **K₀ is not controlled**.
3. **After the ratchet, doors cost nothing [Held].** A door's source fibre (upper floor) is contained in its station's fibre (lower floor), and both have size σ, so they are equal. Hence every edge acts as a *bijection* between fibres, and so does every walk, forward or backward. This is the step that the claim "a slide fills the clock" depends on: the slide must permute the fibre, not merely move it.
4. **A slide on a prime clock reaches every hour [Held].** The slide by d ≠ 0 maps F_K(8) onto itself. Starting from any residue and repeatedly adding d, you stay in the fibre, and since p is prime d is invertible, so you reach every residue. So σ = p and every fibre at depth ≥ K₀ is the whole clock.
5. **Unsquint [Held].** A fibre at a shallower depth contains the finer fibre above it, so every fibre at *every* depth is full. That is Full Dials.
6. **Largeness [Held].** Any number with ending c is at least c. Take c = 3^(B+2) − 1, all 2s in base 3, which is good. Every skeleton member with that ending exceeds B.

**What the ratchet is not [Derived; a recurring over-reading].** The ratchet is a pigeonhole: it produces *some* full fibre at *some* depth, with no rate. It does not replace spectral expansion, and it yields no mixing statement of any kind. Quantitative claims must come from elsewhere (§9.3, B3). *(Formerly unaddressed-points item 18.)*

**The small primes [Held].** For p = 2, 3 the clock arithmetic breaks (6 is zero), but explicit constructions suffice: doublings for even targets, and numbers of the form (2^k·w − 1)/3 with k chosen so that 2^k·w ≡ 1 mod 3 (or mod 9 for p = 3, remainder 0).

### 2.6 Why it works, in two sentences [Core]

The 3-adic digits and the mod-p clock are two coordinates of one object, and the roads twist the clock as you move on the digits. The twist is non-commutative enough, thanks to the lost digit, that loops produce a slide, and primality turns one slide into the whole clock.

**Where each hypothesis bites.** *p prime*: a slide must generate the clock. *p ≥ 5 in the machine*: labels must be invertible. *3 ∤ w*: otherwise the tree is a single doubling corridor. *The lost digit*: without it the loop group would be too commutative to produce a slide in case 2. *Stabilisation*: existential, which is the reason there are no effective bounds. **No "Artin primitive-root wall" is in the proof [Derived]: the only requirements on p are λ ≠ 1 and p ≥ 5; the one special prime is 7 (case 3).** *(Formerly unaddressed-points item 20.)*

---

### 2.7 The same engine, seen as "no frozen height" [Core, but the equivalence itself is Derived]

There is a second way to say what §2.4 achieves. A **frozen height** on the depth-K graph is an assignment s(c) of a residue mod p to every station c such that every road respects it: s(next station) = (4 or 16)·s(c) along right edges, and s(end of a door) = (that door's label)(s(start)) along left edges. Such a landscape would lock the residue mod p of every skeleton member to its 3-adic ending.

**Equivalence [Derived, argued by hand; not in Lean].** A frozen height exists iff Hol(K) has a common fixed residue iff Hol(K) contains no nonzero translation. (If Hol(K) fixes t₀ at station 8, define s(c) as the image of t₀ under any walk from 8 to c; this is well defined because closed walks fix t₀, and it exists because the ring makes the graph strongly connected. Conversely a frozen height makes s(8) a common fixed point. For the last equivalence: slopes commute, so every commutator in a subgroup of Aff(p) is a translation; if there is no nonzero translation the subgroup is abelian, and an abelian subgroup of the affine group of a prime field has a common fixed point.) So the translation of §2.4 is exactly the statement that no frozen height exists.

**The contradiction, case by case [Derived].** If 4^(3^K) ≠ 1, going once around the ring forces s(8) = 0, hence s ≡ 0 everywhere, but any door sends 0 to −μ/6 ≠ 0. If ord(4) = 3^a with a ≥ 2, the doors with endings 8 and 8 + 3^(a+1) share a label and (heights depending only on the exponent mod 3^a) have equal source heights, so equal target heights; but their targets differ at the 3^a scale, so the heights differ. For p = 7 the doors 2 and 11 force s = 2 and s = 6 at the same station-type. This route avoids the bookkeeping of return paths, but it has only been argued by hand, whereas the formal proof uses the zoom-twist-commutator route of §2.4.

**A mistake that is easy to make.** The equalities above are between *values of the hypothetical height function*. They are not claims that the two integers 8 and 8 + 3^(a+1), or their images, are congruent mod p. They are not (they generally differ mod p). Reading the argument as an equality of residues makes it appear to prove a false statement.

**Computational check [Observed, §9.9].** Setting up the frozen-height conditions as a linear system over 𝔽_p and solving: no solution exists for any of the 64 pairs (p, K) tried.

## 3. A finite model of the forgetting [Held, as mathematics; **Tangent** — a heuristic, not used in the proof]

Take the six depth-1 endings 2, 8, 11, 17, 20, 26 and draw an arrow from each to every ending in the class where its door lands (the lost digit unknown). Concretely, using the multiplier table of §2.1: L(2) = 2, L(8) = 20, L(11) = 56, L(17) = 11 all land ≡ 2 mod 9, while L(20) = 26 and L(26) = 17 land ≡ 8 mod 9 — matching the formal proof's μ table exactly. So A has rows (1,0,1,0,1,0) for the first four endings and (0,1,0,1,0,1) for the last two. It has rank 2, A² has rank 1 with every row (2,1,2,1,2,1), A³ = 3A², the characteristic polynomial is X⁵(X − 3), and the number of closed routes of length n is exactly 3^n. Since every row sums to 3, A/3 is a genuine stochastic matrix that **mixes in exactly two steps**, with stationary weights 2/9 on each of 2, 11, 20 and 1/9 on each of 8, 17, 26.

This is the exact shadow of the heuristic "after two left turns the start is forgotten". It is an illustration and is not used in the proof. The modelling assumption, that the lost digit is free, is the one thing nothing here proves about the real map — the leak is real, and a small Lean statement of it is on the to-do list (§6, item 4): `¬ ∃ f, ∀ x, InG x → L x % 27 = f (x % 27)`, witnesses 8 and 35.

**Link to §9.3.** This six-state matrix is the base of the 6p-state chain in §9.3: the same transitions on the endings, with the clock position updated by the affine label of each door. The observed spectral gap there is evidence that the forgetting survives being carried across the clock fibres. It is evidence, not a proof of an effective theorem. What supports the model is §9.4's agreement with real trajectories, not the model's own elegance.

---

## 4. The surrounding landscape [Tangent throughout — the map, not the road; all literature here is [Memory], check before citing]

**Local-global.** CHAT is a local statement (no obstruction at 3 and at one prime). The conjecture is global (everything is an ancestor of 1). Local statements alone cannot reach the global one, and that gap is the archimedean place.

**Known results of the neighbouring kind.**
- Almost all numbers have finite stopping time (Terras; Everett), and Tao (2019) shows almost all orbits attain almost bounded values, in the sense of logarithmic density. These are *forward* statements and rest on different randomness (see C3 below).
- The number of n ≤ X that reach 1 is at least a power of X; as far as I recall, Krasikov and Lagarias give an exponent near 0.84.
- Cycle results are archimedean: the known obstacles are bounds on how close 2^a and 3^b can be (transcendence-type input), together with large computer verification of trajectories.

**Structural viewpoints.**
- *Conjugacy to the shift on ℤ₂* (Bernstein, Lagarias): Collatz as a 2-adic shift. The backward tree is the 3-adic counterpart, and CHAT lives there.
- *The ×2 ×3 philosophy* (Furstenberg): rigidity under joint multiplication by 2 and 3. CHAT looks like a finite-field, 3-adic shadow of such rigidity. This is an analogy and not a theorem.
- *Random walks on affine groups over 𝔽_p with slopes in ⟨2, 3⟩*: this puts the quantitative questions in the territory of sum-product estimates and expansion in affine groups (Bourgain–Gamburd line of work). I can't give precise statements.
- *Skew products*: the tower with affine labels is a skew product over a 3-adic automaton with an affine cocycle. CHAT says the cocycle is not cohomologous to a trivial one. Ergodicity is the natural language for the limit.

**What is genuinely new here, as far as I can tell [Belief].** The explicit holonomy mechanism, especially the *blur as engine*. I don't know of the same combination elsewhere, but I haven't searched; a literature check should come before any priority claim.

**Possible external homes for the mechanism [Belief; Tangent — orientation only. Every attribution here is unchecked; one pasted link (Lindenstrauss–Varjú) turned out to be about Euclidean isometries and self-similar measures, not 𝔽_p, and should not be recycled without re-verification].** Stripped of the Syracuse setting, CHAT proves a local-global surjectivity statement for one non-invertible, congruence-guarded affine system. Four fields it might connect to, each named from memory:
1. *Strong approximation for guarded semigroups.* Classical strong approximation (Nori, Weisfeiler, as I recall) concerns Zariski-dense groups. A system where inversion is unavailable and steps are guarded by residues is outside it. Whether CHAT's ring-loops-plus-ratchet argument extends to a class of such systems is an open question, not a result.
2. *Automata on rooted trees.* D(v) can be read as a transducer on the 3-adic tree coupled to an affine clock. Whether Hol(K) fills the ceiling at all depths is a level-transitivity question of the kind studied by Grigorchuk and Nekrashevych's school.
3. *Non-Lipschitz p-adic dynamics.* As I recall, Anashin's ergodicity theory treats 1-Lipschitz maps; a map that divides by p is outside it. Whether the blur mechanism gives ergodicity criteria for such maps is untested.
4. *Affine expanders.* Random walks on Aff(𝔽_p) with linear parts in ⟨2, 3⟩ sit in sum-product territory (Bourgain–Gamburd, as I recall; the Lindenstrauss–Varjú citation is suspect, see above). The gap in §9.3 and §9.8 is consistent with an expander family, and it is not a proof of one.
Do not claim more than "worth checking" for any of these until the literature has been read.

## 5. The frontier

### Tier A: low-hanging fruit

**A1. Left-only equidistribution for typical seeds. Start here.** [Derived] [Core for the quantitative programme]
A chain of k left turns from a seed v depends only on v mod 3^(k+2), and each turn consumes one fresh base-3 digit of v. For seeds up to 3^M the pair (ending mod 27, residue mod p), over the first M steps, is up to a CRT error of about p/3^M exactly a Markov chain on 6p states, and the six-state matrix of §3 is its base. Mixing reduces to the transitivity of the loop group on the clock, which is CHAT's content, and quantitatively to the spectral gap of an explicit 6p × 6p matrix. The payoff would be an *effective* theorem: for all but a small fraction of seeds up to X, the residue after k left turns is within ε of uniform once C·log p ≤ k ≤ (horizon), with explicit rates, where CHAT's stabilisation depth is only existential. **Correction to my earlier wording:** *visiting every residue* is a coupon-collector event and needs about p·log p steps, not O(log p). Equidistribution of the residue at a given step is the fast statement (§9).
*Needs proving:* that left-only loops contain a nonzero translation (I expect yes, unproved; the commutator argument above uses right-edge rides), and irreducibility with aperiodicity of the 6p-state chain.
*Catch:* a seed has only about log₃ v digits, so runs are capped at logarithmic length; past that the dynamics are deterministic. The theorem would be "typical seed, orbit of logarithmic length", not an infinite-orbit statement. For 3-adic seeds the full ergodic theorem is available, but integers are a measure-zero set. [Dragon-lite]
*Side note [Derived, quick arithmetic]:* under the chain's stationary weights a left turn multiplies size by about 2^0.75 ≈ 1.7 on average, while consuming log₂3 ≈ 1.58 bits of the seed's digit supply. That comparison is the first thing to examine when asking how far the horizon can be pushed.

**A2. Pin down Hol(K) exactly.** [Picked: see §9] [Core for the quantitative programme]
§2.3 gives the ceiling: translations extended by ⟨2, 3⟩. Computed for 11 primes, including four proper-subgroup primes (23, 47, 71, 73), Hol(K) *equals* the ceiling at K = 1 and 2. So the right conjecture is **Hol(K) = ℤ/p ⋊ ⟨2, 3⟩** [Observed]. My earlier "full affine group" claim was an artefact of four small primes where ⟨2, 3⟩ happens to be everything.
The question "is ⟨2, 3⟩ everything?" is a two-generator cousin of Artin's primitive-root problem, and the proper-subgroup primes in §2.3 are its failures. [Belief; reasoning only, no literature check.] The ceiling's size p·|⟨2, 3⟩| therefore fluctuates with p, and any uniform spectral-gap statement has to pass through the primes where it is unusually small (see §9.8). **The theorem itself does not need ⟨2, 3⟩ large — only the gap does (§2.3).**

**A3. Several primes, prime powers, composite moduli. [Derived; tested in §9.6] [Core: a cheap, real extension]** Not hard after all, and my earlier claim that it needs a joint zoom was wrong. Let H be the loop group over the clock ℤ/m, with gcd(m, 6) = 1. Slopes commute, so every commutator in H is a pure translation. The map H → H_p (reduce mod each prime p | m) is onto, so the commutator subgroup maps onto the single-prime commutator subgroup, which is all of ℤ/p by CHAT. A subgroup of the cyclic group ℤ/m that maps onto ℤ/p for every p | m is all of ℤ/m. So H contains *every* translation of ℤ/m, and the ratchet runs unchanged over the clock ℤ/m. This covers two or more distinct primes, prime powers p^k, and every modulus coprime to 6. The 2-adic part and powers of 3 beyond the ending are not covered.

**A4. Prime powers.** Same argument as A3; see above. The commutator identity (1, (λ−1)β) holds in any commutative ring, and (λ−1)β is a unit mod p^k as soon as it is a unit mod p.

**A5. Effective size bounds. [Belief]** The ratchet is why none exist. I suspect a direct construction: use the explicit translation loop's length to produce, for each residue, a skeleton member of explicit size, giving an exponential-in-p upper bound on the smallest ancestor with a given residue. That makes CHAT quotable as a computable statement. **The real obstruction that such a construction must clear [Derived; formerly unaddressed-points item 23]: a loop can only be applied to a number whose deeper digits match the door's requirement, and the ratchet works on sets, not single numbers. Any explicit construction must either supply the matching digits or argue past them; a naive "apply the translation loop k times" does not.**

**A6. Housekeeping.** Rename `exact_one_step_mixing` (it is a two-step statement). Add references. Check p = 23 before anyone cites "full group". Keep the Lean-faithful paper and the exposition citing each other. **Audit the paper's Lean-name appendix against the current file (it was copied from the written account and only partially re-grepped).** Add the bibliography to the LaTeX paper (Lagarias's survey, the lifting-the-exponent lemma, Mathlib) and replace the placeholder author line.

**A7. Reference audit. [Core discipline for anything citeable]** Every [Memory] item in §4 and in the external-homes paragraph (Terras, Everett, Tao 2019, Krasikov–Lagarias with its exponent near 0.84, Bernstein–Lagarias, Furstenberg, Nori, Weisfeiler, Nekrashevych, Anashin, Lindenstrauss–Varjú) must be checked against the literature before it appears in any document meant for citation. An unchecked attribution is worse than a missing one, because a reader will assume it was checked. **The Lindenstrauss–Varjú instance has a known mismatch (isometries, not 𝔽_p); treat it as wrong until a correct affine-walks-over-finite-fields citation is found.**

### Tier B: real mathematics, uncertain payoff

**B1. A general CHAT lemma. [Core for the generalisation programme]** Extract: a finite-state automaton with an affine cocycle over a prime field; when does the loop group contain a translation? Test on 5x+1 and 7x+1, where the mismatch is between 2 and a different prime. If it transfers, the structure is the mismatch and not an accident of 3. If it fails, that is informative too. **Evidence so far (§9.10): raw coverage shows no exception for any tested q — so the test must probe the *method*, not the conclusion.** Conway's undecidability result applies to generalised periodic-rule Collatz functions as a class, not to each individual qx+1; it is not a reason to avoid 5x+1 [Derived; corrects an earlier caution]. *(Formerly unaddressed-points item 21.)*

**B2. The spectral gap.** [Dragon] Prove a uniform lower bound on the gap of the 6p-state chain (or of the averaged left-turn operator on 𝔽_p). This is the quantitative engine for A1. The observed λ₂ stays between roughly 0.51 and 0.70 with no visible decay in p; at primes where ⟨2, 3⟩ is small the gap does not collapse either. A proof would almost certainly need sum-product or expander input.

**B3. Growth rates and the horizon.** Under the stationary weights of the six-state matrix a left turn multiplies size by about 1.7 on average while consuming log₂ 3 ≈ 1.58 bits. Whether this comparison can be turned into a rigorous statement about how far a typical seed can be driven before its digit supply is exhausted is open.

### Tier C: the larger picture

**C1. Density.** CHAT says nothing about natural density. The set of n that reach a fixed w and lie in a fixed residue class mod p is infinite, but could still have density zero.

**C2. Cycles.** CHAT does not address cycles.

**C3. Relation to Tao 2019 and the almost-all results.** Those are forward statements about typical orbits; CHAT is a backward local-obstruction statement. They live in different regimes.

---

## 6. Open items and to-do list (short)

1. Formalise the leak: `¬ ∃ f, ∀ x, InG x → L x % 27 = f (x % 27)` (witnesses 8, 35).
2. Full re-grep of the paper's Appendix against the current Lean file.
3. Spectral-gap proof (owed, never started).
4. Bibliography and author line for the LaTeX paper.
5. General-q machinery (q = 5, 7, 17).
6. Larger-p and 5x+1 gap tests.
7. Rename `exact_one_step_mixing`.

---

## 7. How to talk about the work [Core]

The accurate sentence: *the inverse Collatz tree of every positive w not divisible by 3 has no local obstruction at the 3-adic place together with any single prime, and the proof exhibits an explicit affine holonomy mechanism, in which the digit lost by a left turn is what makes the clock turn, that may transfer to other maps.* It does not say Collatz is true and it does not say the tree is large. The paper should be known for the mechanism (the commutator, the ratchet, the blur as engine), not oversold on the gap it leaves.

**Two ways to pitch it, depending on the audience.**
- *To Collatz readers:* the sentence above, plus: no natural density, no bounds on orbit sizes, no exclusion of cycles.
- *To dynamics and algebra readers:* "For a specific non-invertible affine system on the integers, governed by a 3-adic finite-state automaton, we prove that orbits meet every pair of a good 3-adic ending and a residue modulo any prime p ≥ 5 (and, by a short extension, modulo any modulus coprime to 6). The proof isolates a mechanism, in which the digit lost by one branch forces nontrivial holonomy, that we conjecture transfers to other systems." Do not call this a general theorem, and keep numerical spectral-gap observations out of the abstract.

## 8. Companion documents and their state [Core housekeeping]

*CHAT: Every Remainder, Everywhere* is the intuitive exposition with worked examples. *Inverse Collatz Trees and Simultaneous Density* is the LaTeX paper, faithful to the Lean development; **its Appendix is the canonical name table, and where any notation below disagrees with it, the paper wins**. The Lean files hold the formal statements: the main development and the separate six-state amnesia file. These are derived from, and subordinate to, this document and the Lean files.

**Known staleness [Housekeeping].** The exposition still says it did not recompile Lean, and does not yet mention the −336 key (§9.7), the composite-moduli result (A3/§9.6), or the frozen-height reformulation (§2.7); it is subordinate to this document but out of date in those respects. The LaTeX paper's remark after Proposition 4.3 (single cycle) is marked as not formalised; if it is ever formalised, the paper must be updated. The paper has no bibliography and carries the placeholder author line "The Authors" (A6, A7). *(Formerly unaddressed-points items 29–30.)*

## 9. Fruit picked: computations done after the first draft of this document

All of these are **[Observed]** unless tagged otherwise: exact computation on specific primes, not proofs.

**9.1 The ceiling is the answer (A2).** For p = 5, 7, 11, 13 (as before) and now also 23, 31, 47, 71, 73, 79, 89, with depths K = 1 and 2, the loop group Hol(K), built from forward and backward closed walks at station 8, is exactly (all translations) ⋊ ⟨2, 3⟩. Its order is p·|⟨2, 3⟩|, for instance 253 = 23·11 at p = 23. The slope subgroup produced by loops equals ⟨2, 3⟩ in every case, including where it is a proper subgroup. Conjecture: **Hol(K) = ℤ/p ⋊ ⟨2, 3⟩ for all p ≥ 5 and all K.** Nothing above the ceiling is possible, and in every computed case nothing below it occurs.

**9.2 Left turns alone already generate it (A1, finite check).** Restrict to *forward* closed walks using only left edges. The set of labels again equals the full ceiling group, containing all p translations, for every prime from 5 to 97 at depth 1 and 2, and at depth 3 for primes below 41. So a left-only translation exists in every tested case, and the commutator argument's use of right-edge rides is not essential. What is missing is a uniform proof.

**9.3 The six-state model extends to a 6p-state chain with a clean spectrum. [Observed; Tangent — heuristic support for A1, not part of the proof]** States are (ending mod 27, residue mod p); from (e, t) the chain moves to (f, ll(e)(t)) with f uniform among the three endings in e's target class. For every prime tested (5 to 37, then 47, 71, 97, 131):
- the chain is **primitive** (some power has all entries positive, within at most 9 steps), so it is irreducible and aperiodic;
- its stationary law is **exactly uniform in the residue**, times the weights (2, 1, 2, 1, 2, 1)/9 on the six endings, as in §3;
- the second-largest eigenvalue modulus stays between about **0.51 and 0.70** across the whole range, with no visible decay as p grows (p = 131 gives 0.577).

A gap that does not close as p grows is what an expander-type phenomenon looks like, and it would put the quantitative side squarely in the sum-product territory of B3. I have no proof of this; it is the sharpest open target in the document.

**9.4 The chain predicts real integers, and keeps predicting past the digit horizon. [Observed; Tangent — but the load-bearing evidence for the §3 model]** I took 6,000 random good seeds in [3^22, 3^23), iterated the genuine map L, and compared with a simulation of the chain. For p = 7 and p = 23, the fraction of seeds that have visited every residue by step k matched the chain to within sampling noise (p = 7, step 20: 0.623 real versus 0.628 chain; step 44: 0.941 versus 0.947), and the residue distribution at each step was at the sampling noise floor from uniform. This held out to 44 steps, *twice* the digit horizon of about 22. So the horizon in A1 is a limit of what a proof of this shape can reach, not an observed change in behaviour. [Observed, one choice of horizon]

**9.5 A correction about "coverage".** Coverage of all p residues is a coupon-collector event. With the residue near-uniform at each step, covering needs about p·log p steps. For p = 23 the observed coverage by step 44 was about 3.5%, in line with the chain. An honest effective theorem therefore has two parts: fast equidistribution at a given step (about log p steps to mix), and coverage only when the horizon exceeds roughly p·log p, that is for p up to about log X / log log X.

**What to do with this.** The conjecture in 9.1, the left-only generation in 9.2 and the uniform gap in 9.3 are three clean statements, in increasing order of difficulty, each of which a computer cannot settle but now strongly suggests. If the gap can be proved, the effective A1 theorem follows by standard Markov-chain bounds, and the CRT cutoff gives the stated horizon.

**9.6 Composite moduli and prime powers (A3, A4).** Computed the loop group Hol_m(K) for m = 25, 49, 121, 125, 35, 55, 65, 77, 91, 143 and 385 = 5·7·11, at K = 1 (and K = 2 except for 385). In every case the translations are *all* of ℤ/m and |Hol| equals m·|⟨2, 3⟩ mod m| exactly. This agrees with the commutator-subgroup argument in A3, which I have only derived by hand.

**9.7 An explicit search-free key.** At depth 1, the twist is (32/3)t − 16/3 and the zoom is 64t, and their commutator is the pure slide by **−336 = −2⁴·3·7**. It is nonzero mod every prime p ≥ 5 except p = 7, which uses doors 2 and 11 instead (slide −16 ≡ 5). Being a rational number, it reduces mod any modulus coprime to 6, and is a unit mod m exactly when gcd(336, m) = 1. **Open [Derived as a question, unresolved]: whether this key generalises — a closed form for general K needs a discrete logarithm mod 3^(K+1), and the qx+1 analogue is unknown (§13, item 27).**

**9.8 Spectral gap: more evidence, and a worry.** The averaged left-turn affine operator on 𝔽_p, with the true weights (2,1,2,1,2,1)/9, has second eigenvalue (call it **λ₂**, not ρ — ρ is the class map, §12.3) 0.617 (p = 101), 0.577 (499), 0.549 (1009). At primes where ⟨2, 3⟩ is unusually small, the gap does not visibly close up to 2600: p = 431 (|⟨2,3⟩| = 43) gives 0.692 and p = 601 (75) gives 0.559. **Caution on reading these as asymptotics [Derived; corrects an earlier over-reading]: the earlier per-prime "ρ" values varied from 0.93 down to 0.73 across primes and were computed with equal weights — that is scatter, not an asymptotic.** A uniform gap would need |⟨2, 3⟩| ≥ p^δ for all p; I don't know that this holds, and Bourgain-type theorems need it as a hypothesis. **The theorem needs none of this (§2.3); only the quantitative programme does.**

**9.9 Reconciliation checks [Observed].** Three checks, now run.
- *Fidelity of the graph to the real tree.* For 153,531 genuine integer steps (random good integers, and the actual skeleton nodes below 8 and 20 to depth 12), at depths K = 1, 2, 3 and p = 7, 19, 23, the predicted class (right edge, and left edge from the finer ending) and the predicted residue mod p (the road's label) matched the real R(y) and L(y) every time: zero mismatches. This tests the definitions and single steps, not the stabilisation claim.
- *Frozen heights.* The linear system of §2.7 has no solution for every prime 5 ≤ p < 110 at K = 1, 2 and every p ≤ 37 at K = 3 (64 cases).
- *Truncated-tree fibre check [Observed; Tangent — consistent but not confirmatory, see §10.6].* In the skeleton tree from seed 128 to depth 20, every station's fibre was the full clock for p = 5 and p = 7 at depths 1 to 3, as the theorem predicts. *(Formerly unaddressed-points item 15; recorded here so it is not re-run.)*
- *Old reports.* The earlier Bible reported that Hol(K) is all of Aff(p) for p = 5, 7, 13, 19 and K ≤ 3. That is consistent with §9.1 (for those primes ⟨2, 3⟩ is everything) but should not be generalised; p = 23 is the first counterexample.

### 9.10 qx+1: probes, exceptions, and what transfers [Observed/Derived; Core for B1 — the map data for the generalisation test]

*Folded in from the former unaddressed-points log, items 7–14. This is the largest block of results that lived only in chat.*

**Raw backward-tree probes [Observed].** For q = 3, 5, 7, 11, 13, 17 (about 2.5–3 million nodes each, from w = 1; 5x+1 also from w = 3 and 7), no prime below 60 (below 80 for 5x+1) had any missing residue. Raw coverage is therefore not where any exception shows up — these probes test the *conclusion*, not the *method*. Any B1 test must probe the loop-group machinery, not just coverage.

**Two different kinds of exception [Observed as finite computations; Derived as the taxonomy].** Do not conflate them:
- **Exceptional p** (for fixed q): primes where the multiplier 2^M (M = ord_q(2)) has order exactly q. These form finite sets per q: **q = 3: {7}; q = 5: {11, 31, 41}; q = 7: {127, 337}; q = 11: {23, 89, 683, …}**. These are the analogue of CHAT's case 3, and they are handled per q by the exceptional-prime door pair (as p = 7 is for q = 3).
- **Exceptional q**: primes q where the gearbox hypothesis v_q(2^M − 1) = 1 fails — exactly the **Wieferich primes** (1093 and 3511 below 5000). These break the *structure* of the gearbox, not just one case. **Correction of an earlier over-claim [Derived]: I once said I had "ruled out" Wieferich primes; all I had done was test the congruence for p < 100.**

**Zsigmondy correction [Derived; corrects an earlier "finite exceptions" reading].** The set of primes whose multiplier has order a *power* of q is **infinite** (for q = 3: 7; then 19, 73; then 87211, 262657; and so on). Only the a = 1 case is finite. The a ≥ 2 primes are exactly what the blur trick (case 2) handles, which is why case 2 is not a corner case but the generic one at depth.

**The blur trick should generalise to any q [Derived, unverified in a full construction].** If two doors have the same multiplier and their sources differ by exactly q^(a+1), the difference of their images is μ·(difference)/q, which has q-adic valuation exactly a. The multipliers agree automatically once a ≥ 2. A short derivation; never checked inside a complete general-q construction — that is exactly what the §6 item 6 machinery build is for.

**Why 7x+1 and 17x+1 may behave differently [Derived].** 2 is not a primitive root mod 7 or mod 17 (they are the first two primes where that fails), so the ring may split and the "good ground" must be restricted. Concretely, for ℓ = 7 no padding exists for half the residues, since 2 has order 3 mod 7. Raw coverage showed no exceptions there, so any difference would be in the *method*, not the conclusion.

**Primes where 2 is not a primitive root, below 100 [Observed]:** 7, 17, 23, 31, 41, 43, 47, 71, 73, 79, 89, 97 (all ≡ ±1 mod 8 except 43). 7 = 2³ − 1 and 17 = 2⁴ + 1 is a coincidence of two cases, since 23 is neither.

**Exceptional-p data for 5x+1 [Observed; Tangent — kept so it is not recomputed].** The order-of-a-power-of-5 primes below 100 are 31 (ord 2 = 5) and 11 (ord 4 = 5); the primes with a proper ⟨2, 5⟩ are 31, 41, 71, 79, 89. Trivial exceptions at p = 2, 3, 5. *(Formerly unaddressed-points item 13.)*

**A closed question with no answer yet [Parked; unresolved, see §13 item 26].** A "named kind of prime" that "spits out master keys" for 5x+1 was mentioned in an early chat and never identified — not Mersenne, not Wieferich-as-tested, possibly an order-based class. Parked until the general-q machinery (§6, item 6) generates the candidates and the class can be named from data instead of memory.

---

## 10. Failed and superseded approaches [Core — tested and failed, which is exactly why it stays]

Each entry records the exact inference that broke, whether the failure is fundamental, and what survived. An attempt is listed only if it shows an obstacle that could be hit again.

**10.1 The naive finite-state proof.** *Idea:* follow a number through (ending mod 27, residue mod p), a state space of exactly 6p states, take left turns by default, and inject right turns to escape cycles. *Why it looked fine:* the Chinese remainder theorem makes the state space a product, and the offsets keep 0 from being absorbing. *Where it breaks, three separate inferences.* (a) The state is not an autonomous factor of the tree: 8 and 35 both end in 8 mod 27, with the same multiplier, but L(8) = 20 and L(35) = 92 ≡ 11 mod 27; the next ending needs x mod 81. This is fundamental to the quotient by 27. (b) "The affine maps act transitively on 𝔽_p" is the very claim to be proved. (c) Finiteness bounds when a *state* repeats, not when all residues have been visited. *What survived:* the tower of precisions, which is the cost of each left turn made explicit; the discipline that the model is a **relation**, sound from real to model (every real step is a model step) and not from model to real (a model path need not come from any integer), which the ratchet then repairs on fibres; and the fact g(0) = −μ/6 ≠ 0 used in §2.7. **Warning:** the six-state matrix of §3 and the 6p-state chain of §9.3 are this model again, kept honestly as a *heuristic* whose assumption (the lost digit is free) is not proved. What supports it is §9.4, not the model. A small Lean statement of the failure, `¬ ∃ f, ∀ x, InG x → L x % 27 = f (x % 27)`, with witnesses 8 and 35, was proposed and is not in the current file [proposed, uncompiled; now §6, item 5].

**10.2 Return-path proofs of the translation.** *Idea:* build explicit closed walks (right path to a station, a door, a right path back) and compare labels. *Fragile points:* the bookkeeping must use the **accumulated exponent** (1 or 2 per right edge), not the number of edges; at K = a the shortest path from 8 to the second door's source is empty (increment 0, not 3^a), so a claim "N steps, increment 3^a" holds only if the second source is *defined* as an iterate of R from 8; and the many case splits. *What survived:* every case still stands as an independent route and the commutator lemma is the formal proof's engine. §2.4 and §2.7 are the cleaner account.

**10.3 Edge-level fixed points.** Individual labels have no common fixed residue (4t = t forces t = 0; every door sends 0 to −μ/6), but this says nothing about Hol(K) and is useless: a common fixed point of all edge labels would give one for Hol(K), while the converse fails. Only closed walks at 8 map a single set to itself.

**10.4 Overdetermination from several lifts of one station.** At K = 1 a station has three doors with different labels, and at p = 7 demanding a common height gives a contradiction (κ = 2 and κ = 6). *Where it fails as a method:* for K > a the lifts of a station no longer disagree in a way that forces anything. A related attempt through Weil bounds and overlap estimates was reported as overstated; its text is lost. *What survived:* the useful pair of doors is one that is **height-equal but target-distinct**, not any two doors.

**10.5 Never-Seven (predecessor route) [Parked — retired from the critical path, retained as an optional Diophantine conjecture].** The predecessor route reached the translation through a Diophantine condition: for a ≥ 2 let j_a be the unique j below 3^a with 4^j ≡ 7 mod 3^(a+1), call a prime q *relevant* if the order of 2 mod q is exactly 2·3^a (equivalently, ord of 4 is 3^a), and conjecture 4^(j_a) ≢ 7 mod q for every relevant q (equivalently gcd(4^j − 7, 2^(2·3^(a−1)) − 2^(3^(a−1)) + 1) = 3; I have not checked the equivalence). A counterexample would only show that *one particular pair of walks* shares a fixed point; it would not make Hol(K) have one. The current proof never mentions 7 or 4^j. Caution about a tempting misreading: 4⁸ − 7 = 65529 = 3⁴·809 looks like a collision for a = 2, but 809 is not relevant (reportedly ord₈₀₉(2) = 404, unchecked here), and the relevant prime for a = 2 is 19, where 4⁸ ≡ 5. Any formal use must state the prime is ≥ 5, because the definition degenerates at 2 and 3.

**10.6 Testing stabilisation on truncated trees.** A depth-truncated tree violates "equal fibre sizes" and "stable sizes" for boring reasons (not enough descendants yet), so it can neither confirm nor refute them. The meaningful test is fidelity of single steps on real integers (§9.9). *(The seed-128 depth-20 check of §9.9 lands here: consistent, but confirmatory of nothing.)*

**10.7 Two misstatements to avoid.** (a) "The case κ = 0 is killed by the ring-lap case." Wrong: when the lap multiplier is 1 that case does not apply; κ = 0 is killed directly by the door offset −μ/6 ≠ 0. (b) "The two-door contradiction at p = 7 is an accident of K = 1." It holds for every K ≥ 1 in height form, because the two doors, at different stations when K ≥ 2, both sit on stations ≡ 2 mod 9 and carry the same height.

**10.8 Two more critiqued routes, recorded so their refutations are not re-derived [Derived critiques; Tangent].** *(Formerly unaddressed-points item 24.)*
- *The "Lead 3" averaged-operator run:* its per-prime values varied from 0.93 down to 0.73 across primes, so it was never asymptotic, and it used equal weights rather than the true (2,1,2,1,2,1)/9. Only the rerun numbers belong in §9.8, and they are there under the name λ₂.
- *The "Lead 4" second-difference route:* the padding depends only on x mod ℓ, so a "second difference mod ℓ²" is arbitrary, and the slide's nonvanishing does not come from padding being non-affine. The route as stated proves nothing; do not resurrect it without repairing these two points.

---

## 11. Verification status, conventions and working method [Core]

**11.1 What the Lean files establish** (compiled by the authors; the main file contains no `sorry`; I read the statements and did not recompile):
- the Main Theorem for every prime (`main_theorem_infinite`) and Full Dials for p ≥ 5 (`main_theorem_core`);
- the bridge, as three proved statements and not an assumption: stabilisation (`stabilization`), the no-loss-after-stabilisation equality (`fibre_succ_eq`), and bijective action of edges and walks (`edge_image`, `walk_image`);
- the translation (`translation_exists`) through the dilation cases (`dilation_exists`), the twist (`ref_walk`) and the commutator identity (`comm_label`);
- ring transitivity and the lap at 8 (`R_transitive`, `R_full_loop`), lifting-the-exponent (`lte_four`), soundness of the skeleton (`Desc_reaches`), and the small primes;
- separately, the six-state matrix facts (characteristic polynomial, A³ = 3A², trace of Aⁿ).

**11.2 What is not formalised** (argued by hand, derived, or observed only): the frozen-height equivalence and its case proof (§2.7); that the right-edge map is a single cycle (follows from transitivity plus injectivity); the composite-moduli and prime-power extension (A3, A4); the explicit key −336 (§9.7, a short computation); left-only generation of translations and Hol(K) = ℤ/p ⋊ ⟨2, 3⟩ (§9); the spectral gap; everything about qx+1.

**11.3 Names: older notation ↔ current.** Stations U_K, base vertex b = 8 ↔ good shadows, station 8. Right edge c ↦ ρ(c) — the class map ρ(c) = R(c) mod 3^(K+1), Lean `rnext`, with multiplier 4^step(c) (Lean `rStep`) and clock label `rightLabel`. Left edge indexed by a finer ending e ↔ a door, `Edge.left`, `lnext`, `leftLabel`. Fibre f_K(c) ↔ `fibre`. Hol_K ↔ `Hol` (the set of labels of closed walks at 8). Precision lemma ↔ `L_cong`. Closure ↔ `R_mem`, `L_mem`, `Desc_mem`. Single-cycle lemma ↔ `R_transitive` with `R_full_loop`. Theorem A ↔ `main_theorem_core`; Theorem B ↔ `translation_exists`. The multiplier is `mult`, and exactness of the division is the lemma `L_spec`. **Caution:** earlier editions of this table used ρ for the per-edge multiplier 4 or 16; the formal proof's ρ is the class map, and that convention is used throughout this document. The full classical-to-Lean table lives in the paper's Appendix and is canonical.

**11.4 Lean pitfalls worth remembering.** Natural-number division rounds down and subtraction truncates at zero, so "L is exact on good numbers" and "2x − 1 does not truncate" are lemmas, not properties of the definition. The prime p is an explicit argument of `Walk`, `Hol` and `fibre`. Division in `ZMod p` is multiplication by an inverse and is meaningful only for p ≥ 5 here. No translation between the original odd-number coordinates and the skeleton's coordinates is needed anywhere: `Desc_reaches` states reachability in the original Collatz map directly.

**11.5 Working method (kept short).** Three questions are different and none implies the next: is the code well-formed, is the statement true, and is it faithful to what the prose claims. A flawless proof of a different statement is a failure. Computation tests definitions and single steps, never a universal statement. A sufficient condition is not a necessary one, and a property of fibres is not a property of the graph. Whenever something fails, find the exact inference and record what survived. Never confuse an equality of values of a hypothetical height function with an equality of integers modulo p. **Never write flattery or atmosphere ("put your feet up", name-drop paragraphs) into a project document; pasted material containing it is summarised, not quoted** [Irrelevant as mathematics; retained as a one-line policy so the judgement is not re-argued]. *(Formerly unaddressed-points item 25.)*

---

## 12. Convention alignment to the formalization account [Core]

*This section logs the reconciliation of this document against Inverse Collatz Trees and Simultaneous Density, whose conventions and Appendix name table are canonical.*

### 12.1 Fixes applied (earlier text disagreed with the formal conventions)

1. **ρ clash (§11.3, formerly "Right edge c ↦ ρ(c)·c").** The old table used ρ for the per-edge multiplier (4 or 16). The formal proof's ρ is the **class map** `ρ(c) = R(c) mod 3^(K+1)` (`rnext`); the multiplier is `4^{step(c)}` (`rStep`). §11.3 now states this and carries a caution; ρ means the class map everywhere in this document.
2. **L's definition (§2.1).** The old text led with the padding form L(x) = 2^i·m. The formal proof's canonical object is the **multiplier**: L(x) = μ(x)(2x−1)/6 with μ(x) ∈ {2,4,8,16} depending only on x mod 27 (μ = 2·2^i). §2.1 now leads with μ; the padding form follows as a corollary.
3. **"Padding factor" renamed (§2.3).** μ is now called the **multiplier** (Lean `mult`), matching the formal proof, not "the door's padding factor".
4. **w > 0 restored (§1, §2.1, §7).** The formal Theorem 1 hypothesizes w > 0; the statements here now say "positive integer w".
5. **Ring notation introduced (§2.2).** ρ, ρ^n, step(c) and E(n,c) are now named explicitly (`rnext`, `rIter`, `rStep`, `rExp`), since §2.4's zoom bookkeeping implicitly used the accumulated exponent.

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

The 30-item standalone log has been folded into this edition. Item-by-item disposition, with the evidence and relevance flags assigned on intake:

| Items | Content | Where it went | Flags on intake |
|---|---|---|---|
| 1 | Spectral-gap proof sketch (offer) | §6, item 3 (owed, never started) | [Belief] [Core-for-A1] |
| 2 | Larger-p and 5x+1 gap tests (offer) | §6, item 7 | lead |
| 3 | General-q machinery, q = 5, 7, 17 (offer) | §6, item 6 | lead [Core-for-B1] |
| 4 | Formalise the leak in Lean (offer) | §6, item 5; §10.1; §3 | lead, cheap |
| 5 | LaTeX paper references (offer) | §6, item 8; A6, A7 | housekeeping |
| 6 | Re-audit the paper's Lean-name table (offer) | §6, item 8; §12.2 | housekeeping |
| 7 | Raw qx+1 probes, no missing residues | §9.10 | [Observed] [Core-for-B1] |
| 8 | Two kinds of exception (exceptional p finite per q; exceptional q = Wieferich) | §9.10 | [Observed]/[Derived] [Core-for-B1] |
| 9 | Zsigmondy correction (a ≥ 2 infinite) | §9.10 | [Derived] [Core-for-B1] |
| 10 | Blur trick generalises to any q | §9.10 | [Derived, unverified] [Core-for-B1] |
| 11 | 7x+1 / 17x+1 ring-split worry | §9.10 | [Derived] [Core-for-B1] |
| 12 | Primes < 100 where 2 is not a primitive root | §9.10 | [Observed] [Tangent — lookup data] |
| 13 | 5x+1 exceptional-p data below 100 | §9.10 | [Observed] [Tangent — kept so it is not recomputed] |
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
| 28 | "W₄, W₅" shortcut (d₄ = −135/256, d₅ = −513/512) | Not folded in: source never seen, values unverifiable | [Parked] — do not use without the source |
| 29 | Exposition staleness | §8 | housekeeping |
| 30 | Paper's Prop 4.3 remark not formalised | §8 | housekeeping |

**The retired log is superseded by this table.** If a future conversation recalls "the unaddressed points", the answer is here: every item is either folded in at the listed place, parked with a reason, or tombstoned as irrelevant-with-policy.

---

*Unverified throughout: the frozen-height proof (§2.7, only checked by solver), the uniform-in-p statements in §9 (observed on finitely many primes, not proved); the Goursat reduction; the growth-rate arithmetic; all literature recalled from memory. Everything tagged [Derived], [Belief] or [Memory] is a lead, not a fact.*
