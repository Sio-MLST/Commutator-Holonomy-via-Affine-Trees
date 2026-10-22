# Every-Remainder-Everywhere
### A Formalization-Faithful Account

**Abstract.** We prove the following theorem: for every prime $p$, every positive integer $w$ with $3 \nmid w$, every residue class $r \bmod p$, and every bound $B$, there exists $n > B$ such that $n$ reaches $w$ under the Collatz map and $n \equiv r \pmod p$. In particular, the set of such $n$ is infinite. The proof proceeds by constructing, for each seed $v$ in an explicitly defined inverse tree $D(v)$, a family of "fibres" recording the residues mod $p$ attained by descendants of $v$ in each adic class; we show these fibres stabilize in size, become simultaneously all of $\mathbb{Z}/p\mathbb{Z}$, and transfer to the genuine Collatz inverse tree. Every lemma, hypothesis, and inference below corresponds step-for-step to a Lean 4 formalization; the correspondence is documented in the Appendix.

---

## Standing assumptions and notation

Throughout the paper, $p$ denotes a **prime number**. Sections 2–9 additionally assume $p \ge 5$. Natural-number division is truncating; $\mathbb{Z}/p\mathbb{Z}$ is the finite field with $p$ elements, and $(n : \mathbb{Z}/p\mathbb{Z})$ or simply $n \bmod p$ denotes the reduction of a natural number.

**Convention (classes).** A *class at depth $K$* always means: a natural number $c$ with $c \equiv 2$ or $8 \pmod 9$ (*valid*) **and** $c < 3^{K+1}$ (*bounded*). Where both properties are used, we say so once and use them freely. The depth index $K$ is always at least $1$.

**Standing maps.** For natural $x$ define the two inverse Collatz branches:
$$R(x) = \begin{cases} 4x & x \equiv 2 \pmod 9,\\ 16x & \text{otherwise},\end{cases} \qquad L(x) = \frac{\mu(x)(2x-1)}{6},$$
where the *multiplier* $\mu$ depends only on $x \bmod 27$:
$$\mu(x) = \begin{cases} 4 & x \equiv 2,\\ 8 & x \equiv 8,\\ 16 & x \equiv 11,\\ 2 & x \equiv 17,\\ 4 & x \equiv 20,\\ 2 & x \equiv 26 \end{cases} \pmod{27}.$$
Note $\mu(x) \in \{2,4,8,16\}$ always, i.e. $\mu(x)$ is a power of $2$.

**The seed set.** $G = \{x \ge 8 : x \equiv 2 \text{ or } 8 \pmod 9\}$. For $v \in G$, the *descendant tree* $D(v)$ is the least set of natural numbers containing $v$ and closed under $R$ and $L$.

---

## 1. The Collatz layer: the theorem and its reduction

The Collatz map is $C(n) = n/2$ if $n$ is even, $C(n) = 3n+1$ if $n$ is odd. We say $n$ *reaches* $v$ if $C^{(k)}(n) = v$ for some $k \ge 0$.

**Theorem 1 (Main Theorem).** *Fix a prime $p$. Let $w > 0$ with $3 \nmid w$, let $r \in \mathbb{Z}/p\mathbb{Z}$, and let $B$ be arbitrary. Then there exists $n$ with $n > B$, $n$ reaches $w$, and $n \equiv r \pmod p$.*

The proof splits according to $p$.

- **For $p \ge 5$ (Sections 2–9).** We construct an abstract inverse tree $D(v)$ from a seed $v \in G$ reaching $w$, and prove a density statement (Theorem 2): in every valid class at every depth, the descendants of $v$ realize *every* residue mod $p$. Choosing the class deep enough that its least element exceeds $B$ yields $n > B$ with the required residue; that $n$ reaches $w$ follows because the abstract tree embeds in the genuine Collatz inverse tree (Lemma 1.2).

- **For $p = 2$ and $p = 3$ (Section 10).** Explicit constructions using only that $C$ halves even numbers.

We record the elementary Collatz facts first; each is verified by direct computation or induction.

**Lemma 1.1.** (i) $C(2a) = a$. (ii) $C^{(j)}(2^j m) = m$ for all $j, m$. (iii) $R(x)$ reaches $x$: if $x \equiv 2 \pmod 9$ then $R(x) = 2^2 x$ reaches $x$ in 2 steps; otherwise $R(x) = 2^4 x$ reaches $x$ in 4 steps. (iv) If $x \ge 1$ and $x \equiv 2 \pmod 3$ then $L(x)$ reaches $x$.

*Proof.* (i), (ii): the even branch, and induction on $j$. (iii): immediate from (ii). (iv): by Lemma 2.1 below, $6\,L(x) = \mu(x)(2x-1)$ exactly. Write $\mu(x) = 2 \cdot 2^i$ with $i \in \{0,1,2,3\}$ and $2x - 1 = 3m$ (divisible by 3 since $x \equiv 2 \pmod 3$). Then $6\,L(x) = 6 \cdot 2^i m$, so $L(x) = 2^i m$. Now $m = (2x-1)/3$ is odd, so $C(m) = 3m+1 = 2x$, and $C(2x) = x$. Hence $L(x) = 2^i m$ reaches $x$ in $(i) + 2$ steps. ∎

**Lemma 1.2 (the tree is genuine).** *If $v \in G$ and $n \in D(v)$, then $n$ reaches $v$.*

*Proof.* Induction on the derivation of $n \in D(v)$. If $n = v$, zero steps. If $n = R(m)$ with $m \in D(v)$: $R(m)$ reaches $m$ (Lemma 1.1(iii)) and $m$ reaches $v$ by hypothesis; compose. If $n = L(m)$: by Lemma 2.3 below $m \in G$, so $m \ge 8 \ge 1$ and $m \equiv 2 \pmod 3$; conclude by Lemma 1.1(iv) and composition as before. ∎

**Lemma 1.3 (seeds exist).** *If $m > 0$ and $3 \nmid m$, there exists $v \in G$ reaching $m$.*

*Proof.* $m \bmod 9 \in \{1,2,4,5,7,8\}$. In each case multiply $m$ by a power of $2$ so the product lands in $\{2,8\} \pmod 9$ and exceeds 8:

| $m \bmod 9$ | 1 | 2 | 4 | 5 | 7 | 8 |
|---|---|---|---|---|---|---|
| $v$ | $128m$ | $64m$ | $128m$ | $256m$ | $512m$ | $64m$ |
| halvings | 7 | 6 | 7 | 8 | 9 | 6 |
| $v \bmod 9$ | 2 | 2 | 8 | 2 | 2 | 8 |

(Each entry: $128 \equiv 2$, $64 \equiv 1$, $256 \equiv 4$, $512 \equiv 8 \pmod 9$; e.g. $512m \equiv 8 \cdot 7 = 56 \equiv 2$.) Then $v \in G$ and $v = 2^k m$ reaches $m$ in $k$ halvings by Lemma 1.1(ii). ∎

Finally, reachability is transitive ($C^{(k_2+k_1)}(a) = C^{(k_2)}(C^{(k_1)}(a))$), and we record $C^{(3)}(8) = 1$.

---

## 2. Basic structure of the branches

**Lemma 2.1 (exactness of $L$).** *If $x \equiv 2 \pmod 3$, then $6\,L(x) = \mu(x)(2x-1)$ exactly.*

*Proof.* $\mu(x)$ is even (a power of 2), say $\mu(x) = 2a$; and $3 \mid 2x-1$ since $2x \equiv 4 \equiv 1 \pmod 3$, say $2x - 1 = 3b$. Then $\mu(x)(2x-1) = 6ab$, a multiple of 6; since $L(x)$ is *defined* as the truncating quotient $\mu(x)(2x-1)/6$ and the dividend is a multiple of the divisor, the quotient is exact. ∎

**Lemma 2.2 (residue reduction).** *If $x \equiv 2$ or $8 \pmod 9$, then $x \bmod 27 \in \{2, 8, 11, 17, 20, 26\}$.*

*Proof.* The six residues listed are exactly the residues mod 27 that reduce to 2 or 8 mod 9; and $x \bmod 27$ determines $x \bmod 9$. Finite check. ∎

**Lemma 2.3 ($D(v)$ stays in $G$).** *(i) If $x \in G$ then $R(x) \in G$. (ii) If $x \in G$ then $L(x) \in G$ and $L(x)$ is valid (residue $\equiv 2,8 \pmod 9$) even without the size hypothesis. (iii) If $v \in G$ and $n \in D(v)$ then $n \in G$.*

*Proof.* (i) If $x \equiv 2 \pmod 9$: $R(x) = 4x \equiv 8$, and $R(x) \ge 4x \ge 32$. If $x \equiv 8$: $R(x) = 16x \equiv 16 \cdot 8 = 128 \equiv 2 \pmod 9$, and $R(x) \ge 16 \cdot 8$.
(ii) From $x \in G$, $x \equiv 2 \pmod 3$; apply Lemma 2.1 and Lemma 2.2, then split on the six possible residues of $x$ mod 27, unfolding $\mu$ in each case and evaluating $L(x) \bmod 9$ (each case yields 2 or 8) and $L(x) \ge 8$ when $x \ge 8$.
(iii) Induction over the tree, using (i) and (ii). ∎

**Lemma 2.4 (precision loss under $L$).** *Let $K \ge 1$. Suppose $x \equiv y \pmod{3^{K+2}}$ and $x \equiv y \equiv 2 \pmod 3$. Then $\mu(x) = \mu(y)$ and*
$$L(x) \equiv L(y) \pmod{3^{K+1}}.$$

*Proof.* Since $3^3 \mid 3^{K+2}$, we get $x \equiv y \pmod{27}$, hence $\mu(x) = \mu(y)$ (Lemma 2.2 plus the definition of $\mu$). For the congruence, work in $\mathbb{Z}$: write $x = (x \bmod 3^{K+2}) + 3^{K+2} \lfloor x/3^{K+2} \rfloor$ and similarly for $y$; since the residues agree, $3^{K+2} \mid x - y$, say $x - y = 3^{K+2} q$. By Lemma 2.1 (cast to $\mathbb{Z}$; truncated subtraction is genuine since $x, y \ge 1$):
$$6\,L(x) = \mu(x)(2x-1), \qquad 6\,L(y) = \mu(y)(2y-1) = \mu(x)(2y-1).$$
Subtracting and dividing by 2: $3(L(x) - L(y)) = \mu(x)(x - y) = \mu(x)\, 3^{K+2} q$. Dividing by 3: $L(x) - L(y) = 3^{K+1} \mu(x) q$. Hence $3^{K+1} \mid L(x) - L(y)$; as $L(x), L(y) \ge 0$, congruence in $\mathbb{Z}$ is equality of remainders. ∎

*Remark.* A right step ($\times 2^i$) loses no 3-adic precision; a left step forgets its argument modulo one power of 3 less. All later bookkeeping absorbs exactly this one-power loss.

---

## 3. The right-branch map on classes

Fix $K \ge 1$ for this section and the next. On classes mod $3^{K+1}$, define the right-branch map
$$\rho(c) = R(c) \bmod 3^{K+1},$$
the left-edge targets $\lambda(e) = L(e) \bmod 3^{K+1}$, the step size $\mathrm{step}(c) = 1$ if $c \equiv 2 \pmod 9$ else $2$ (so that $R(c) = c \cdot 4^{\mathrm{step}(c)}$), the iterates $\rho^n(c)$, and the cumulative exponent $E(n, c)$ defined by $E(0,c) = 0$, $E(n+1, c) = \mathrm{step}(c) + E(n, \rho(c))$.

**Lemma 3.1.** *(i) $9 \mid 3^{K+1}$; validity is preserved by reduction mod $3^{K+1}$. (ii) $8 < 3^{K+1}$. (iii) If $c$ is valid then $\rho(c)$ is a class at depth $K$. (iv) If $c$ is a class at depth $K$, so is $\rho^n(c)$ for every $n$.*

*Proof.* (i) $2 \le K+1$. (ii) $8 < 9 \le 3^{K+1}$. (iii) The bound is the general remainder bound. For validity: $\rho(c) \equiv R(c) \pmod 9$, and $R(c) \equiv 8$ or $2 \pmod 9$ exactly as in Lemma 2.3(i). (iv) Induction on $n$. ∎

**Lemma 3.2 (iterates are multiplications).** *If $c$ is a class at depth $K$, then for all $n$:*
$$\rho^n(c) = \bigl(c \cdot 4^{E(n,c)}\bigr) \bmod 3^{K+1}.$$

*Proof.* Induction on $n$. At $n=0$ both sides are $c \bmod 3^{K+1}$. For the step, apply the induction hypothesis at the class $\rho(c)$ (Lemma 3.1(iii)), note $R(c) = c \cdot 4^{\mathrm{step}(c)}$, and use that a remainder is congruent to its number and congruences multiply. The exponent accumulates by definition. Finally, since $\rho^n(c) < 3^{K+1}$ (Lemma 3.1(iv)), its remainder is itself. ∎

**Lemma 3.3 (two steps triple the exponent).** *(i) If $c$ is valid, $\mathrm{step}(c) + \mathrm{step}(\rho(c)) = 3$. (ii) If $c$ is a class at depth $K$, then $E(2k, c) = 3k$.*

*Proof.* (i) $R$ swaps the two residues: $2 \mapsto 8 \mapsto 2 \pmod 9$. (ii) Induction on $k$: unfold two steps, apply (i) and the induction hypothesis at the class $\rho^2(c)$ (valid and bounded by two applications of Lemma 3.1(iii)). ∎

**Lemma 3.4 (lifting the exponent).** *For $n \neq 0$: $v_3(4^n - 1) = 1 + v_3(n)$. Consequently, for all $K, m$:*
$$4^m \equiv 1 \pmod{3^{K+1}} \iff 3^K \mid m.$$

*Proof.* The first statement is the lifting-the-exponent lemma for the prime 3 at $(4,1)$: $v_3(4^n - 1) = v_3(4-1) + v_3(n) = 1 + v_3(n)$. For the equivalence: if $m = 0$ both sides hold trivially. If $m > 0$, then $4^m > 1$ and the congruence is $3^{K+1} \mid 4^m - 1$, i.e. $v_3(4^m - 1) \ge K+1$, i.e. $1 + v_3(m) \ge K+1$, i.e. $3^K \mid m$. Every equivalence is an iff. ∎

**Lemma 3.5 (cancellation).** *If $a \le b$ and $4^a \equiv 4^b \pmod{3^{K+1}}$, then $a \equiv b \pmod{3^K}$.*

*Proof.* $\gcd(4^a, 3^{K+1}) = 1$; cancel $4^a$ to get $4^{b-a} \equiv 1 \pmod{3^{K+1}}$, and apply Lemma 3.4. ∎

**Lemma 3.6.** $4^E \bmod 9$ is $1, 4, 7$ according as $E \equiv 0, 1, 2 \pmod 3$.

*Proof.* Strong induction on $E$, using $4^3 = 64 \equiv 1 \pmod 9$. ∎

---

## 4. Existence of exponents; transitivity of $\rho$

**Lemma 4.1 (every class is $2 \cdot 4^E$).** *If $c$ is a class at depth $K$, there exists $E < 3^K$ with $c = 2 \cdot 4^E \bmod 3^{K+1}$. Moreover $E$ is unique, and $c \equiv 2 \pmod 9$ forces $E \equiv 0 \pmod 3$; $c \equiv 8$ forces $E \equiv 1 \pmod 3$.*

*Proof.* Let $T = \{w < 3^{K+1} : w \equiv 1 \pmod 3\}$. Every $w \in T$ is $3j+1$ with $j < 3^K$, so $|T| \le 3^K$. The map $E \mapsto 4^E \bmod 3^{K+1}$ sends $\{0, \dots, 3^K - 1\}$ into $T$ (as $4 \equiv 1 \pmod 3$), and is injective there: equal values give $4^{E_1} \equiv 4^{E_2} \pmod{3^{K+1}}$, hence $E_1 \equiv E_2 \pmod{3^K}$ by Lemma 3.5, hence $E_1 = E_2$ as both are $< 3^K$. By pigeonhole it is surjective onto $T$. Now choose $w \in T$ with $2w = c$ if $c$ is even ($w = c/2 \in T$ since $c \equiv 2 \pmod 3$) or $2w = c + 3^{K+1}$ if $c$ is odd ($3^{K+1}$ being odd). By surjectivity $w = 4^E \bmod 3^{K+1}$ for some $E < 3^K$; then $2 \cdot 4^E \bmod 3^{K+1} = 2w \bmod 3^{K+1} = c$ in both cases. For the type claim: reduce mod 9 and apply Lemma 3.6 — $2 \cdot 4^E \bmod 9$ is $2, 8, 5$ according as $E \equiv 0, 1, 2 \pmod 3$, and validity excludes 5. ∎

**Proposition 4.2 (transitivity).** *If $c, d$ are classes at depth $K$, there exists $n$ with $\rho^n(c) = d$.*

*Proof.* Write $c = 2 \cdot 4^{E_c}$, $d = 2 \cdot 4^{E_d} \pmod{3^{K+1}}$ (Lemma 4.1), with $E_c, E_d \in \{0,1\} \pmod 3$ of matching types. Set
$$y := E_d + 3^K E_c - E_c,$$
which satisfies $E_c + y = E_d + 3^K E_c$ (no truncation: $E_c \le 3^K E_c$).

*Constructing a path with exponent $y$.* If $y \equiv 0 \pmod 3$, take $n = 2\lfloor y/3 \rfloor$; Lemma 3.3(ii) gives $E(n, c) = 3\lfloor y/3\rfloor = y$. If $y \equiv 1 \pmod 3$: reading $y + E_c = E_d + 3^K E_c$ mod 3 (the last term divisible by 3) forces $E_c \equiv 0$, $E_d \equiv 1 \pmod 3$; then $c \equiv 2 \pmod 9$, $\mathrm{step}(c) = 1$, and $n = 2\lfloor y/3\rfloor + 1$ gives $E(n,c) = 1 + 3\lfloor y/3 \rfloor = y$ (as $y = 3\lfloor y/3\rfloor + 1$). The case $y \equiv 2$ is symmetric: then $c \equiv 8$, $\mathrm{step}(c) = 2$, and $2 + 3\lfloor y/3\rfloor = y$.

*Verification.* By Lemma 3.2 it suffices to show $(c \cdot 4^y) \bmod 3^{K+1} = d$. Now
$$c \cdot 4^y \equiv 2 \cdot 4^{E_c + y} = 2 \cdot 4^{E_d + 3^K E_c} = 2 \cdot 4^{E_d} \cdot (4^{3^K})^{E_c} \equiv 2 \cdot 4^{E_d} \pmod{3^{K+1}},$$
the last step because $3^K \mid 3^K$ gives $4^{3^K} \equiv 1 \pmod{3^{K+1}}$ (Lemma 3.4). Since $d = 2 \cdot 4^{E_d} \bmod 3^{K+1}$, this is $d$. ∎

**Proposition 4.3 (loop at 8).** *For every $K \ge 1$ there exists $n > 0$ with $\rho^n(8) = 8$ and $E(n, 8) = 3^K$.*

*Proof.* Write $K = j+1$ and take $n = 2 \cdot 3^j$. By Lemma 3.3(ii), $E(n, 8) = 3 \cdot 3^j = 3^{j+1} = 3^K$. By Lemma 3.2, $\rho^n(8) = (8 \cdot 4^{3^{j+1}}) \bmod 3^{j+2}$. Lemma 3.4 at level $j+1$ (the exponent $3^{j+1}$ is divisible by $3^{j+1}$) gives $4^{3^{j+1}} \equiv 1 \pmod{3^{j+2}}$, so $8 \cdot 4^{3^{j+1}} \equiv 8$, and reducing (using $8 < 3^{j+2}$, Lemma 3.1(ii)) gives $\rho^n(8) = 8$. ∎

---

## 5. Exponent arithmetic

**Lemma 5.1.** *If $c$ is a class at depth $K$, $d < 3^{K+1}$, and $\rho^n(c) = d$, then $c \cdot 4^{E(n,c)} \equiv d \pmod{3^{K+1}}$.*

*Proof.* Lemma 3.2 and $d \bmod 3^{K+1} = d$. ∎

**Lemma 5.2 (congruence forces divisibility).** *If $b \le K$, $3 \nmid c$, $c \cdot 4^E \equiv c' \pmod{3^{K+1}}$, and $c' \equiv c \pmod{3^{b+1}}$, then $3^b \mid E$.*

*Proof.* Reduce the first congruence mod $3^{b+1}$, compose with the second, and cancel $c$ (coprime to $3^{b+1}$ since $3 \nmid c$): $4^E \equiv 1 \pmod{3^{b+1}}$. Apply Lemma 3.4 at level $b$. ∎

**Lemma 5.3 (divisibility forces congruence).** *If $b \le K$, $c \cdot 4^E \equiv c' \pmod{3^{K+1}}$, and $3^b \mid E$, then $c' \equiv c \pmod{3^{b+1}}$.*

*Proof.* Reduce mod $3^{b+1}$; Lemma 3.4 turns $3^b \mid E$ into $4^E \equiv 1 \pmod{3^{b+1}}$; chain the congruences. ∎

**Lemma 5.4.** *If $c \equiv d \equiv 2 \pmod 9$, $c < 3^{K+1}$, and $\rho^n(c) = d$, then $3 \mid E(n, c)$.*

*Proof.* By Lemma 5.1, $d \equiv c \cdot 4^E \pmod{3^{K+1}}$ with $E = E(n,c)$. Reduce mod 9: $2 \equiv 2 \cdot 4^E \pmod 9$. By Lemma 3.6, $2 \cdot 4^E \bmod 9$ is $2, 8, 5$ according as $E \equiv 0, 1, 2 \pmod 3$; the cases $1, 2$ contradict $d \equiv 2$. ∎

**Lemma 5.5.** *If $(4 \bmod p)^3 = 1$ and $3 \mid E$ then $(4 \bmod p)^E = 1$.* (Immediate: $E = 3q$.)

---

## 6. Affine labels and walks

Fix $p \ge 5$ from here through Section 9. An *affine label* is a pair $f = (a, b) \in (\mathbb{Z}/p\mathbb{Z})^2$, acting by $f(t) = a t + b$; composition is $f \then g = (g_1 f_1,\; g_1 f_2 + g_2)$ ("first $f$, then $g$"), and when $a \neq 0$ the inverse is $f^{-1} = (a^{-1}, -a^{-1} b)$. These satisfy the usual identities: composition is associative with unit $(1,0)$, and $(f^{-1} \circ f)(t) = (f \circ f^{-1})(t) = t$, $f \then g = g \then f$ inverting as $(f \then g)^{-1} = g^{-1} \then f^{-1}$ — each verified by clearing the invertible denominators.

**Nonvanishing.** In $\mathbb{Z}/p\mathbb{Z}$ with $p \ge 5$: $2, 3, 4, 6 \neq 0$ (the characteristic property: $p \mid 2$ would force $p \le 2$), hence $4^j \neq 0$ and $\mu(x) \neq 0$ after reduction (a power of 2).

**Edges and walks.** The *class graph* at depth $K$ has as vertices the classes mod $3^{K+1}$ and two kinds of directed edges:
- a **right edge** $c \to \rho(c)$, labeled $\mathrm{rl}(c) = (4, 0)$ if $c \equiv 2 \pmod 9$, else $(16, 0)$;
- a **left edge** $e \bmod 3^{K+1} \to \lambda(e) = L(e) \bmod 3^{K+1}$ (for $e$ valid, $e < 3^{K+2}$), labeled $\mathrm{ll}(e) = (\mu(e)/3, -\mu(e)/6)$.

A *walk* is a finite sequence of edge traversals, forward or backward, starting and ending at classes; its label is the composition of the traversed labels in order (the label of the empty walk at $c$ is $(1,0)$). A *closed walk at 8* is a walk from 8 to 8. Write
$$\mathrm{Hol}(K) = \{f : f \text{ is the label of a closed walk at } 8 \text{ at depth } K\}.$$

**Lemma 6.1.** *(i) Every edge label $f$ has $f_1 \neq 0$; every walk label too. (ii) Walks compose: a walk $u \to w$ with label $f$ followed by $w \to d$ with label $g$ is a walk $u \to d$ with label $f \then g$. (iii) The reverse of a walk $u \to w$ with label $f$ is a walk $w \to u$ with label $f^{-1}$. Consequently $\mathrm{Hol}(K)$ is closed under $\then$ and (for $p \ge 5$) under inversion.*

*Proof.* (i) Right-edge slopes are powers of 4; left-edge slopes are quotients of nonzero $\mu(e)$ by nonzero 3; walk labels are compositions, and products/inverses of nonzero elements are nonzero. (ii) Induction on the first walk, re-associating with the associativity identity. (iii) Induction on the walk: reverse each traversal and each remaining sub-walk; the label computes to the inverse via $(f \then g)^{-1} = g^{-1} \then f^{-1}$, using (i) to justify the inversions. ∎

**Lemma 6.2 (labels of right paths).** *If $c$ is a class at depth $K$, there is a walk from $c$ to $\rho^n(c)$ with label $(4^{E(n,c)}, 0)$.*

*Proof.* Induction on $n$: the empty walk at $n = 0$; at $n+1$, prepend the right edge from $c$ — its label $(4^{\mathrm{step}(c)}, 0)$ composes with the induction hypothesis label $(4^{E}, 0)$ to $(4^{\mathrm{step}(c) + E}, 0)$, matching the recursive definition of $E$. ∎

**Lemma 6.3 (right labels in arithmetic).** *(i) $R(x) = \mathrm{rl}(x)\,(x \bmod p)$: if $x \equiv 2 \pmod 9$ both sides are $4x$; otherwise $16x$. (ii) If $p \ge 5$ and $x \in G$, then $L(x) \bmod p = \mathrm{ll}(x)\,(x \bmod p)$, i.e. $L(x) \equiv (\mu(x)/3)\,x - \mu(x)/6 \pmod p$.*

*Proof.* (i) is immediate. (ii) By Lemma 2.1, $6 L(x) = \mu(x)(2x-1)$; cast to $\mathbb{Z}/p\mathbb{Z}$ (truncated subtraction is genuine) and compute $6 \cdot \mathrm{ll}(x)(x \bmod p)$ from the definitions, clearing the invertible 3 and 6: both equal $\mu(x)(2x-1) \bmod p$; cancel 6. ∎

---

## 7. Fibres

Fix a seed $v \in G$ for Sections 7–9. For $K \ge 1$ and a class $c$ at depth $K$, the **fibre**
$$F_K(c) = \{\, n \bmod p : n \in D(v),\ n \equiv c \pmod{3^{K+1}} \,\} \subseteq \mathbb{Z}/p\mathbb{Z}.$$
It records which residues mod $p$ occur among descendants of $v$ in the class $c$.

**Lemma 7.1 (class arithmetic).** *(i) $R(x) \bmod 3^{K+1} = \rho(x \bmod 3^{K+1})$. (ii) $L(x) \bmod 3^{K+1} = \lambda(x \bmod 3^{K+2})$ — note the depth shift, which is Lemma 2.4 at work. (iii) $\mathrm{rl}(x \bmod 3^{K+1}) = \mathrm{rl}(x)$; (iv) $\mathrm{ll}(x \bmod 3^{K+2}) = \mathrm{ll}(x)$ (since $27 \mid 3^{K+2}$ and $\mu$ depends only on $x \bmod 27$).*

*Proof.* (i) The case split defining $R$ depends only on $x \bmod 9$, which is unchanged by reduction mod $3^{K+1}$; then reduce before or after multiplying by 4 (resp. 16). (ii) Both $x$ and $x \bmod 3^{K+2}$ satisfy the hypotheses of Lemma 2.4 and agree mod $3^{K+2}$. (iii), (iv): the labels depend only on residues mod 9 resp. mod 27. ∎

**Lemma 7.2 (edge action on fibres).** *(i) If $t \in F_K(c)$ then $\mathrm{rl}(c)(t) \in F_K(\rho(c))$. (ii) If $p \ge 5$ and $t \in F_{K+1}(e)$ then $\mathrm{ll}(e)(t) \in F_K(\lambda(e))$.*

*Proof.* (i) Say $t = n \bmod p$ with $n \in D(v)$, $n \equiv c \pmod{3^{K+1}}$. Then $R(n) \in D(v)$; its class is $\rho(c)$ by Lemma 7.1(i); its residue is $\mathrm{rl}(n)(t) = \mathrm{rl}(c)(t)$ by Lemma 6.3(i) and 7.1(iii). (ii) Similarly with $L(n)$, Lemma 7.1(ii) for the class and Lemma 6.3(ii), 7.1(iv) for the residue. ∎

**Lemma 7.3 (projection).** *If $J \le K$ and $t \in F_K(c)$, then $t \in F_J(c \bmod 3^{J+1})$.* (Double reduction, $3^{J+1} \mid 3^{K+1}$.)

**Lemma 7.4 (nonemptiness at the root).** $F_K(v \bmod 3^{K+1}) \neq \emptyset$ (witnessed by $v$ itself). ∎

---

## 8. Stabilization of fibre sizes

Throughout, $p \ge 5$ and $v \in G$.

**Lemma 8.1.** *For every class $c$ at depth $K$: $|F_K(c)| \le |F_K(\rho(c))|$. Consequently $|F_K(c)| \le |F_K(\rho^n(c))|$ for all $n$, and $|F_K(c)| = |F_K(d)|$ for any two classes $c, d$ at depth $K$.*

*Proof.* The slope of $\mathrm{rl}(c)$ is $4^{\mathrm{step}(c)} \neq 0$, so $t \mapsto \mathrm{rl}(c)(t)$ is injective; by Lemma 7.2(i) it maps $F_K(c)$ into $F_K(\rho(c))$. An injection's image has the source's cardinality, bounded by the target's. Iterate for the chain; for constancy, apply Proposition 4.2 in both directions and combine the two inequalities. ∎

**Lemma 8.2 (monotonicity in depth).** $|F_{K+1}(8)| \le |F_K(8)|$.

*Proof.* $F_{K+1}(8) \subseteq F_K(8)$ by Lemma 7.3 with $J = K$, using $8 \bmod 3^{K+1} = 8$ (Lemma 3.1(ii)). ∎

**Proposition 8.3 (stabilization).** *There exist $K_0 \ge 1$ and $\sigma \ge 1$ such that for all $K \ge K_0$ and every class $c$ at depth $K$: $|F_K(c)| = \sigma$.*

*Proof.* Positivity: $F_K(v \bmod 3^{K+1}) \neq \emptyset$ (Lemma 7.4) is finite (a subset of $\mathbb{Z}/p\mathbb{Z}$) and has the same cardinality as $F_K(8)$ (Lemma 8.1, the class $v \bmod 3^{K+1}$ being a class at depth $K$); so $|F_K(8)| \ge 1$ for all $K \ge 1$. Let $\sigma$ be the least value attained by $|F_K(8)|$ over $K \ge 1$, and $K_0 \ge 1$ a depth attaining it. Minimality gives $|F_K(8)| \ge \sigma$ always; monotonicity (Lemma 8.2) and induction give $|F_K(8)| \le |F_{K_0}(8)| = \sigma$ for $K \ge K_0$. Constancy across classes is Lemma 8.1. (No effective bound on $K_0$ is claimed.) ∎

**Lemma 8.4 (no precision loss after stabilization).** *Under Proposition 8.3's conclusion: if $K \ge K_0$, $K \ge 1$, and $e$ is valid with $e < 3^{K+2}$, then $F_{K+1}(e) = F_K(e \bmod 3^{K+1})$.*

*Proof.* The inclusion "$\subseteq$" is Lemma 7.3. Both sets are finite of size $\sigma$ (Proposition 8.3 at depths $K+1$ and $K$ respectively — in the latter case the class $e \bmod 3^{K+1}$ is valid by reduction and bounded as a remainder). A subset of a finite set of equal cardinality is the whole set. ∎

**Proposition 8.5 (labels act on fibres).** *Under Proposition 8.3's conclusion, with $K \ge K_0$, $K \ge 1$: (i) every edge label $f$ of an edge $u \to w$ at depth $K$ satisfies $f(F_K(u)) = F_K(w)$; (ii) the same holds for every walk label.*

*Proof.* (i) $f$ is injective (Lemma 6.1(i)). For a right edge $c \to \rho(c)$: the image lies in $F_K(\rho(c))$ (Lemma 7.2(i)) and has size $|F_K(c)| = \sigma = |F_K(\rho(c))|$, so it is everything. For a left edge with source $u = e \bmod 3^{K+1}$ and target $\lambda(e)$: by Lemma 8.4, $F_K(u) = F_{K+1}(e)$, and Lemma 7.2(ii) maps it into $F_K(\lambda(e))$; both have size $\sigma$; the image is everything.
(ii) Induction on the walk: the label of a forward traversal acts as in (i) and then the rest by hypothesis (applying a composition is applying the parts in order); for a backward traversal of an edge labeled $f$ from $w'$, note $f(F_K(w')) = F_K(u)$ by (i), so $f^{-1}(F_K(u)) = F_K(w')$ (as $f^{-1} \circ f = \mathrm{id}$); then continue with the rest of the walk. ∎

---

## 9. The commutator engine and the density theorem

**Lemma 9.1 (commutator identity).** *For $\lambda, \alpha, \beta \in \mathbb{Z}/p\mathbb{Z}$ with $\lambda, \alpha \neq 0$:*
$$(\alpha,\beta)^{-1} \then (\lambda, 0)^{-1} \then (\alpha, \beta) \then (\lambda, 0) \;=\; \bigl(1,\; (\lambda - 1)\beta\bigr).$$

*Proof.* Componentwise from the definitions, clearing the invertible denominators. First component: $\lambda \alpha (\alpha^{-1} \lambda^{-1} \alpha) = \lambda \lambda^{-1} = 1$. Second: the linear combination in $\alpha, \beta, \lambda$ and their inverses simplifies to $(\lambda-1)\beta$. ∎

**Lemma 9.2 (a walk with translation part).** *For every $K \ge 1$ there exist $\alpha, \beta \neq 0$ with $(\alpha, \beta) \in \mathrm{Hol}(K)$.*

*Proof.* The class $\lambda(8) = L(8) \bmod 3^{K+1}$ is a class at depth $K$ (validity by Lemma 2.3(ii), boundedness as a remainder). By Proposition 4.2 there is $n$ with $\rho^n(\lambda(8)) = 8$; by Lemma 6.2 there is a walk $\lambda(8) \to 8$ with label $g = (4^E, 0)$, $E = E(n, \lambda(8))$. Prepend the left edge at 8 (from $8$ to $\lambda(8)$, labeled $\mathrm{ll}(8)$): the result is a closed walk at 8 with label $(\alpha, \beta) = \mathrm{ll}(8) \then g \in \mathrm{Hol}(K)$. Then $\alpha \neq 0$ by Lemma 6.1(i); and computing the second component, $\beta = 4^E \cdot (-8/6) = -4^E \cdot 8/6$, a product of nonzero elements ($4^E \neq 0$; $8/6 \neq 0$ since $\mu(8) = 8 \neq 0$ and $6 \neq 0$), hence nonzero. ∎

**Lemma 9.3 (dilations exist).** *For every $K \ge 1$ there exists $\lambda \in \mathbb{Z}/p\mathbb{Z}$, $\lambda \neq 0, 1$, with $(\lambda, 0) \in \mathrm{Hol}(K)$.*

*Proof.* Three cases.

*Case 1: $4^{3^K} \neq 1$.* By Proposition 4.3 and Lemma 6.2 there is a closed walk at 8 with label $(4^{3^K}, 0)$; take $\lambda = 4^{3^K}$.

*Case 2: $4^{3^K} = 1$ and $\mathrm{ord}(4) = 3^a$ with $2 \le a \le K$.* Write $a = j + 2$ and set $e_1 = 8$, $e_2 = 8 + 27 \cdot 3^j$. Then $e_2 \equiv 8 \pmod{27}$, so both are valid, both $< 3^{K+2}$; and $\mu(8) = \mu(e_2) = 8$, while $L(8) = 20$ and $L(e_2) = 20 + 72 \cdot 3^j$ (by Lemma 2.1: $L(e_2) = \mu(e_2)(2e_2 - 1)/6$). Let $u_i = e_i \bmod 3^{K+1}$ and $w_i = \lambda(e_i)$; all four are classes at depth $K$. By Proposition 4.2 there are $n_s, n_t$ with $\rho^{n_s}(u_1) = u_2$ and $\rho^{n_t}(w_1) = w_2$.

*Source exponent is annihilated:* by Lemma 5.1, $8 \cdot 4^{E_s} \equiv u_2 \pmod{3^{K+1}}$ where $E_s = E(n_s, u_1)$ (note $u_1 = 8$). Apply Lemma 5.2 with $b = a$: we need $u_2 \equiv 8 \pmod{3^{a+1}}$, which holds since $e_2 = 8 + 3^{j+3}$ with $j + 3 = a + 1 \le K + 1$. So $3^a \mid E_s$, and $\mathrm{ord}(4) = 3^a$ gives $4^{E_s} = 1$.

*Target exponent is not:* suppose $4^{E_t} = 1$ where $E_t = E(n_t, w_1)$. Then $3^a \mid E_t$, and Lemma 5.3 with $b = a$ gives $w_2 \equiv w_1 \pmod{3^{a+1}}$, i.e. $20 + 72 \cdot 3^j \equiv 20 \pmod{27 \cdot 3^j}$, i.e. $27 \mid 72$ — false ($72 = 2 \cdot 27 + 18$).

*Conclusion of Case 2:* the composite closed walk built from: the reverse of the left edge at $e_1$ (label $\mathrm{ll}(e_1)^{-1}$), the trivially-labeled walk $u_1 \to u_2$ (label $(4^{E_s}, 0) = (1,0)$), the forward left edge at $e_2$ (label $\mathrm{ll}(e_2)$), the reverse of the walk $w_1 \to w_2$, conjugated from 8 and back by a right walk (Lemma 6.2 and reversal, Lemma 6.1(iii)) — has label
$$\Bigl( \frac{\mu(e_2)}{\mu(e_1) \cdot 4^{E_t}},\; 0 \Bigr) \in \mathrm{Hol}(K).$$
(This is Lemma 9.4 below applied at $e_1, e_2$.) Take $\lambda = \mu(e_2)/(\mu(e_1) 4^{E_t})$: nonzero (a quotient of nonzeros), and $\neq 1$ — otherwise, cancelling the nonzero $\mu(e_1) = 8$, $4^{E_t} = 1$, contradicting the previous paragraph.

*Case 3: $4^{3^K} = 1$ and $\mathrm{ord}(4) = 3$.* Take $e_1 = 2$, $e_2 = 11$ (both valid, $< 3^{K+2}$; $\mu(2) = 4$, $\mu(11) = 16$, $L(2) = 2$, $L(11) = 56$). The four classes $u_i, w_i$ as above are all $\equiv 2 \pmod 9$. By Proposition 4.2 there are $n_s, n_t$ with $\rho^{n_s}(u_1) = u_2$, $\rho^{n_t}(w_1) = w_2$; by Lemma 5.4 (endpoints $\equiv 2 \pmod 9$) both $3 \mid E_s$ and $3 \mid E_t$; by Lemma 5.5, $4^{E_s} = 4^{E_t} = 1$. Lemma 9.4 at $e_1, e_2$ yields
$$\lambda = \frac{\mu(11)}{\mu(2) \cdot 4^{E_t}} = \frac{16}{4} = 4 \neq 0, 1 \in \mathrm{Hol}(K),$$
where $4 \neq 1$ since $3 \neq 0$ and $4 \neq 0$ in $\mathbb{Z}/p\mathbb{Z}$.

*The case analysis is exhaustive:* if $4^{3^K} \neq 1$ we are in Case 1. Otherwise $\mathrm{ord}(4) \mid 3^K$, so $\mathrm{ord}(4) = 3^a$ for some $a \le K$; $a = 0$ would mean $4 = 1$, i.e. $3 = 0 \bmod p$, forcing $p \mid 3$, contradicting $p \ge 5$; so $a \ge 1$, and we are in Case 3 (if $a = 1$) or Case 2 (if $a \ge 2$). ∎

**Lemma 9.4 (dilation from a pair).** *Let $e_1, e_2$ be valid, $< 3^{K+2}$. Suppose there are $n_s, n_t$ with $\rho^{n_s}(u_1) = u_2$ where $u_i = e_i \bmod 3^{K+1}$ and $4^{E(n_s, u_1)} = 1$, and $\rho^{n_t}(w_1) = w_2$ where $w_i = \lambda(e_i)$. Then*
$$\Bigl( \frac{\mu(e_2)}{\mu(e_1) \cdot 4^{E(n_t, w_1)}},\; 0 \Bigr) \in \mathrm{Hol}(K).$$

*Proof.* Build the closed walk: reverse-left-edge at $e_1$ from $w_1$ down to $u_1$; the walk $u_1 \to u_2$ with label $(4^{E(n_s,u_1)}, 0) = (1,0)$; the forward left edge at $e_2$ from $u_2$ up to $w_2$; the reverse of the walk $w_1 \to w_2$ with label $(4^{E(n_t,w_1)}, 0)$. This is a closed walk at $w_1$; conjugate it by a right-walk from 8 to $w_1$ and its reverse (Lemma 6.2, Lemma 6.1(iii), Lemma 6.1(ii)) to obtain a closed walk at 8. Its label is computed componentwise from the composition, clearing the invertible factors $\mu(e_1), \mu(e_2), 3, 6$ and the powers of 4: the slope reduces to $\mu(e_2)/(\mu(e_1) 4^{E(n_t,w_1)})$ and the translation part to $0$. ∎

**Proposition 9.5 (existence of a translation).** *For every $K \ge 1$ there exists $d \neq 0$ in $\mathbb{Z}/p\mathbb{Z}$ with $(1, d) \in \mathrm{Hol}(K)$.*

*Proof.* By Lemma 9.3 take $(\lambda, 0) \in \mathrm{Hol}(K)$ with $\lambda \neq 0, 1$; by Lemma 9.2 take $(\alpha, \beta) \in \mathrm{Hol}(K)$ with $\alpha, \beta \neq 0$. Since $\mathrm{Hol}(K)$ is closed under composition and inversion (Lemma 6.1), it contains the commutator, which by Lemma 9.1 is $(1, (\lambda - 1)\beta)$. And $(\lambda-1)\beta \neq 0$ since $\lambda \neq 1$ and $\beta \neq 0$ in the field $\mathbb{Z}/p\mathbb{Z}$. Take $d = (\lambda-1)\beta$. ∎

**Theorem 2 (simultaneous density).** *Assume $p \ge 5$ and $v \in G$. Let $J \ge 1$, let $c$ be a class at depth $J$, and let $r \in \mathbb{Z}/p\mathbb{Z}$ be arbitrary. Then there exists $y \in D(v)$ with $y \equiv c \pmod{3^{J+1}}$ and $y \equiv r \pmod p$.*

*Proof.* Let $K_0, \sigma$ be as in Proposition 8.3, and set $K = \max(K_0, J, 1)$.

*Step 1 (a translation preserves the fibre at 8).* By Proposition 9.5 there is $d \neq 0$ with $(1, d) \in \mathrm{Hol}(K)$: a closed walk at 8 with label $(1,d)$. By Proposition 8.5(ii) (applicable, $K \ge K_0$), $(1,d)$ maps $F_K(8)$ onto itself. The fibre is nonempty ($|F_K(8)| = \sigma \ge 1$); pick $t_0 \in F_K(8)$. Since $(1,d)(t) = t + d$, induction on $k$ gives $t_0 + k d \in F_K(8)$ for every natural $k$.

*Step 2 (the fibre at 8 is everything).* Let $r' \in \mathbb{Z}/p\mathbb{Z}$ be arbitrary; since $p$ is prime, $d$ is invertible, and taking $k = (r' - t_0)/d$ gives $t_0 + kd = r' \in F_K(8)$. So $F_K(8) = \mathbb{Z}/p\mathbb{Z}$, and consequently $\sigma = |F_K(8)| = p$.

*Step 3 (every valid fibre at depth $K$ is everything).* The requested class $c$ satisfies $c < 3^{K+1}$ (as $c < 3^{J+1} \le 3^{K+1}$), so $|F_K(c)| = \sigma = p$ by Proposition 8.3; a subset of $\mathbb{Z}/p\mathbb{Z}$ with $p$ elements is all of it.

*Step 4 (project to depth $J$).* Since $r \in F_K(c)$ and $J \le K$, Lemma 7.3 gives $r \in F_J(c \bmod 3^{J+1}) = F_J(c)$. Unpacking the definition: some $y \in D(v)$ has $y \equiv c \pmod{3^{J+1}}$ and $y \equiv r \pmod p$. ∎

---

## 10. Completion of the proof

**Proposition 10.1 (the case $p \ge 5$).** *If $p \ge 5$, $w > 0$, $3 \nmid w$, $r \in \mathbb{Z}/p\mathbb{Z}$, $B$ arbitrary: there is $n > B$ reaching $w$ with $n \equiv r \pmod p$.*

*Proof.* By Lemma 1.3 there is $v \in G$ reaching $w$. Choose $J \ge 1$ and a class $c$ at depth $J$ with $c > B$ (e.g. $J = B+1$, $c = 3^{B+2} - 1$, which is $\equiv 8 \pmod 9$ and $< 3^{J+1}$, and $B < 3^B \le c$). By Theorem 2 there is $y \in D(v)$ with $y \equiv c \pmod{3^{J+1}}$, $y \equiv r \pmod p$. Then $y \ge c > B$ (a remainder never exceeds its argument — and $y > 0$, else $y \bmod 3^{J+1} = 0 \neq c$). By Lemma 1.2, $y$ reaches $v$, and $v$ reaches $w$; by transitivity $y$ reaches $w$. Take $n = y$. ∎

**Proposition 10.2 (the case $p = 2$).** *Same conclusion for $p = 2$.*

*Proof.* Either $r = 0$ or $r = 1$.
- $r = 0$: take $n = 2^{B+1} w$. It exceeds $B$ (a power of 2 exceeds its exponent, and $w \ge 1$); it reaches $w$ in $B+1$ halvings; and it is even, so $n \equiv 0 \pmod 2$.
- $r = 1$: choose $k$ with $6B + 6 \le k \le 6B + 7$ and $2^k w \equiv 1 \pmod 3$ (possible: if $w \equiv 1 \pmod 3$ take $k = 2t$ even so $4^{k/2} \equiv 1$; if $w \equiv 2$ take $k$ odd so that $2w \equiv 4 \equiv 1$). Then $3 \mid 2^k w - 1$, so $n = (2^k w - 1)/3$ is a natural number with $3n + 1 = 2^k w$. Now $n$ is odd (as $2^k w$ is even), so $n \equiv 1 \pmod 2$; $n$ reaches $w$ (one odd step to $2^k w$, then $k$ halvings); and $n > B$ since $n \ge 2^k/3 \ge 2^{6B+6}/3 > B$.

**Proposition 10.3 (the case $p = 3$).** *Same conclusion for $p = 3$.*

*Proof.* $r \in \{0, 1, 2\}$.
- $r = 0$: choose $j < 6$ with $2^j w \equiv 1 \pmod 9$ (possible since $2$ is a primitive root mod 9: the powers $1, 2, 4, 8, 7, 5$ run through all units mod 9). Set $k = j + 6(B+1)$; then $2^k w = 64^{B+1} \cdot 2^j w \equiv 1 \pmod 9$ (as $64 \equiv 1 \pmod 9$). So $n = (2^k w - 1)/3$ satisfies $3n + 1 = 2^k w$ and $n \equiv 0 \pmod 3$ (as $2^k w \equiv 1 \pmod 9$ means the quotient by 3 is $\equiv 0 \pmod 3$); $n$ reaches $w$ and $n > B$ as above.
- $r = 1$: choose $k \ge B$ with $2^k w \equiv 1 \pmod 3$ (as in Proposition 10.2); take $n = 2^k w$: it reaches $w$ in $k$ halvings and $n \equiv 1 \pmod 3$.
- $r = 2$: with the same $k$, take $n = 2^{k+1} w$: it reaches $w$ and $n \equiv 2 \cdot 1 = 2 \pmod 3$. ∎

*Proof of Theorem 1.* If $p \ge 5$, apply Proposition 10.1. Otherwise $p$ is prime and $p < 5$, so $p = 2$ or $p = 3$; apply Proposition 10.2 or 10.3. ∎

---

## Appendix: correspondence with the Lean formalization

Every environment above corresponds to a declaration in the Lean 4 / Mathlib formalization (namespace `CHAT`, with the fixed prime supplied as a global hypothesis). The mapping:

| Classical name | Lean declaration |
|---|---|
| $R$, $L$, $\mu$, $G$, $D(v)$ | `R`, `L`, `mult`, `InG`, `Desc` |
| Lemma 1.1 | `collatz_double`, `collatz_pow_mul`, `R_reaches`, `L_reaches` |
| Lemma 1.2 | `Desc_reaches` |
| Lemma 1.3 | `exists_inG_reaches` |
| Lemma 2.1 | `L_spec` |
| Lemma 2.2 | `residue_mod27` (with `mult_mem`, `mult_pow`) |
| Lemma 2.3 | `R_mem`, `L_mem`, `L_valid`, `Desc_mem` |
| Lemma 2.4 | `mod27_eq_of_mod_pow`, `mult_congr`, `L_cong` |
| $\rho$, $\lambda$, $\mathrm{step}$, $\rho^n$, $E(n,c)$ | `rnext`, `lnext`, `rStep`, `rIter`, `rExp` |
| Lemma 3.1 | `nine_dvd_pow`, `validClass_mod`, `valid8`, `eight_lt`, `rnext_valid`, `rIter_valid` |
| Lemma 3.2 | `rIter_eq`, `rIter_eq'` |
| Lemma 3.3 | `rStep_pair`, `rExp_two_mul` |
| Lemma 3.4 | `lte_four`, `four_pow_modEq_one_iff` |
| Lemma 3.5 | `four_cancel` |
| Lemma 3.6 | `four_pow_mod9` |
| Lemma 4.1 | `exists_exp`, `exp_type` |
| Proposition 4.2 | `R_transitive` |
| Proposition 4.3 | `R_full_loop` |
| Lemmas 5.1–5.5 | `rIter_modEq`, `exp_dvd_of_congr`, `congr_of_exp_dvd`, `rExp_mod3`, `four_pow_eq_one_of_mod3` |
| affine machinery, $\mathrm{Hol}(K)$ | `Aff`, `aapp`, `athen`, `ainv`, `rightLabel`, `leftLabel`, `Edge`, `Walk`, `Hol`, `HasTranslation`; identities: `athen_app` … `ainv_athen`; nonvanishing: `two_ne_zero_zmod` … `mult_cast_ne_zero` |
| Lemma 6.1 | `Edge.label_unit`, `Walk.label_unit`, `Walk.append`, `Walk.symm`, `Hol_mul`, `Hol_inv` |
| Lemma 6.2 | `walk_of_rIter` |
| Lemma 6.3 | `R_label`, `L_label`, `rightLabel_fst/snd` |
| Lemma 7.1 | `R_class`, `L_class`, `rightLabel_mod`, `leftLabel_mod` |
| Lemma 7.2 | `fibre_right`, `fibre_left` |
| Lemmas 7.3, 7.4 | `fibre_proj`, `fibre_root_nonempty`; fibres: `M`, `fibre` |
| Lemma 8.1 | `fibre_ncard_step`, `fibre_ncard_chain`, `fibre_ncard_const` |
| Lemma 8.2 | `fibre_ncard_antitone` |
| Proposition 8.3 | `stabilization` |
| Lemma 8.4 | `fibre_succ_eq` |
| Proposition 8.5 | `edge_image`, `walk_image` |
| Lemma 9.1 | `comm_label` |
| Lemma 9.2 | `ref_walk` |
| Lemma 9.3 | `dil_case1`, `dil_case2a`, `dil_case2b`, `order_split`, `dilation_exists` (with `order_three_forces_seven` as an unused parenthetical) |
| Lemma 9.4 | `pure_dilation_of_pair` (with `rpath`) |
| Proposition 9.5 | `translation_of_dilation`, `translation_exists` |
| Theorem 2 | `main_theorem_core` |
| Propositions 10.1–10.3 | `main_theorem_p_ge_5`, `main_theorem_p_eq_2`, `main_theorem_p_eq_3` (with `exists_large_valid_class`, `cast_eq_of_mod`, `exists_pow_two_mod`, `exists_k_mod_three`, `four_pow_mod_three`, `sixtyfour_pow_mod`, `lt_pow_mul`, `odd_of_three_mul_add_one`, `reaches_of_three_mul_add_one`, `ge_of_mod_eq`, `reaches_trans`, `collatz_8`, `inverse_tree_covers`) |
| Theorem 1 | `main_theorem_infinite` |
| Lemma 5.4's uniqueness analogue | `rExp_unique_mod` (proved in the file, unused by Theorem 1) |

**Fidelity conventions.** (a) Every hypothesis of every classical statement appears in the corresponding Lean statement; the standing "class at depth $K$" convention collects exactly the pair of side conditions (`ValidClass c`, `c < 3^(K+1)`) that Lean states per-theorem. (b) No step of any proof below appeals to a verification not discharged in the formal proof; where the formal proof delegates to arithmetic automation, the classical text performs the arithmetic inline. (c) The order of construction matches the formalization's dependency order; the only reordering is the grouping of small arithmetic facts into Lemmas 1.1, 3.1, and 6.1, which are used in exactly the same places as their Lean counterparts.
