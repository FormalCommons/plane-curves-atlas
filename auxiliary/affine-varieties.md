# Affine algebraic sets and varieties

This auxiliary page covers Chapter 1, "Affine Algebraic Sets" (§§1.1–1.10, pp. 1–16), and Chapter 2, "Affine Varieties" (§§2.1–2.11, pp. 17–30), of Fulton's *Algebraic Curves* [@Fulton2008]. It is a self-contained prerequisite: it uses nothing from later chapters, and the plane-curve pages build on it.

- §1.1 Algebraic preliminaries (pp. 1–3): rings, UFDs, PIDs, quotient rings, characteristic, algebraically closed fields, partial derivatives, Euler's theorem.
- §§1.2–1.5 (pp. 4–9): affine space, algebraic sets $V(S)$, ideals $I(X)$, radicals, the Hilbert basis theorem, irreducible components.
- §1.6 Algebraic subsets of the plane (pp. 9–10): two polynomials without a common factor have finitely many common zeros; the irreducible algebraic subsets of $\mathbb{A}^2$.
- §1.7 Hilbert's Nullstellensatz (pp. 10–12), with Zariski's lemma completed in §§1.8–1.10 (pp. 12–16: modules and finiteness conditions, integral elements, field extensions).
- §§2.1–2.3 (pp. 17–20): coordinate rings $\Gamma(V)$, polynomial maps, affine changes of coordinates.
- §2.4 Rational functions and local rings (pp. 20–22): $k(V)$, $\mathcal{O}_P(V)$, $\mathfrak{m}_P(V)$.
- §2.5 Discrete valuation rings (pp. 22–24); §2.6 Forms (pp. 24–25): homogenization $f^*$ and dehomogenization $F_*$.
- §§2.7–2.8 (pp. 25–26): direct products, operations on ideals.
- §2.9 Ideals with a finite number of zeros (pp. 26–27): $k[X_1,\dots,X_n]/I \cong \prod_i \mathcal{O}_{P_i}/I\mathcal{O}_{P_i}$.
- §§2.10–2.11 (pp. 27–30): quotient modules, exact sequences, free modules.

Most of these two chapters is standard commutative algebra that Mathlib already has. **The library should simply take from Mathlib** the Hilbert basis theorem, both Nullstellensätze, Zariski's lemma (Proposition 4 of §1.10), everything on integral elements and field extensions (§§1.8–1.10, Problems 1.41–1.54), Noetherian rings and the maximal-member lemma, ideal arithmetic and comaximality (§2.8, Problems 2.39–2.42), the definition and characterizations of discrete valuation rings (Proposition 4 of §2.5), direct products, quotient modules, exact sequences and additivity of length (§2.10), and free modules (§2.11). **From Tau Ceti** it should take the weak Nullstellensatz in packaged form, dehomogenization `TauCeti.dehomogenize`, the dimension of spaces of forms, and the cokernel-length lemma that is Problem 2.53. All of this is recorded in the correspondence table.

The page makes milestones of twelve results that the plane-curve library needs and that the libraries do not have in the needed form, or that the later chapters use constantly: finiteness of the intersection of two coprime plane polynomials, hypersurfaces and their reduced equations, Corollary 4 of the Nullstellensatz, the local ring $\mathcal{O}_P(V)$ and its quotient description, discrete valuation rings as Fulton uses them, order versus colength in a DVR, homogenization and the splitting of binary forms, dimension counts for forms and for $k[X_1,\dots,X_n]/(X_1,\dots,X_n)^m$, the decomposition of Proposition 6 of §2.9, and the exact-sequence and length facts.

Parts of this page:

1. Library status.
2. Conventions: the field, the book's conventions, the libraries' conventions, and the traps between them; advice on representing the page's notions.
3. Milestones (12): §1.6 (2), §1.7 (1), §2.4 (1), §2.5 (1), §2.6 (2), §2.8 (2), §2.9 (1), §2.10 (2).
4. Correspondence with Mathlib and Tau Ceti: every definition, every numbered result, the important unnumbered ones, and every starred problem.
5. Notes on the source.
6. Deferred.

> [!important] Library status: checked 2026-11-18
> All library information on this page was checked on 2026-11-18 against Mathlib at commit `048fffa` (the commit Tau Ceti pins) and Tau Ceti `main` at commit `cd41bff`. Every listed declaration was located in the source; nothing was compiled. "—" means that searches of both libraries, by declaration name and by docstring keywords, found nothing: evidence of absence, not proof.

Status words: **full** (the book's statement, in its generality or more general), **partial** (a special case, one direction, or a nearby formulation, explained), **related** (supporting API only), **—** (nothing found).

## Conventions

### The field and its characteristic

- **Chapter 1** works over an arbitrary field $k$, except §1.7 (the Nullstellensatz and its four corollaries, and Problems 1.32–1.38 where stated), which assumes $k$ algebraically closed throughout (p. 10). Individual problems add hypotheses: Problem 1.4 needs $k$ infinite, Problem 1.14 algebraically closed.
- **Chapter 2** opens (p. 17) by fixing $k$ algebraically closed "from now on". So, although the 1969 preface says $k$ is algebraically closed "from Chapter 3 on", every statement of Chapter 2 is made over an algebraically closed field. Many of them do not need it: §§2.5–2.8 and §§2.10–2.11 are pure algebra over any field (often any ring), Proposition 5 of §2.6 is stated over any domain $R$, and Proposition 6 of §2.9 needs algebraic closure only to identify the maximal ideals containing $I$ with the points of $V(I)$. Each milestone below states the generality it actually needs.
- From Chapter 2 on, all rings contain $k$ and all ring homomorphisms are the identity on $k$ (p. 17): in Lean, $k$-algebras and `AlgHom`s.
- **Characteristic.** Nothing in the text of Chapters 1–2 assumes characteristic zero. (The 1969 preface warns that characteristic zero is sometimes assumed "without comment" in the problems; we found no problem of Chapters 1–2 that silently needs it.) The only explicit hypothesis is in Problem 1.53★ (an irreducible polynomial over a field of characteristic zero has distinct roots in a splitting field); perfectness of the field suffices there. Euler's theorem (§1.1, (6)) is the identity $mF = \sum_i X_i F_{X_i}$ in every characteristic (with $m$ read in $k$, possibly zero). Appendix A (p. 113), on nonzero characteristic, discusses Proposition 9(2) of §6.5, differentials in Chapter 8, Problem 5.26 and Lemma 1 of §7.4; it mentions nothing from Chapters 1–2.

### The book's conventions, and the traps against Mathlib

- **Rings** are commutative with $1$, homomorphisms preserve $1$, a domain has at least two elements (§1.1). This is Mathlib's `CommRing` with `IsDomain`.
- **Prime ideals include the unit ideal.** Fulton defines a prime ideal by "$ab \in I$ implies $a \in I$ or $b \in I$" (p. 2), without requiring $I \ne R$, and only then says that a *proper* ideal is prime iff the quotient is a domain. Mathlib's `Ideal.IsPrime` requires `I ≠ ⊤`. Correspondingly Fulton calls an algebraic set irreducible if it is not a union of two proper algebraic subsets (§1.5), so $\varnothing$ is irreducible (Corollary 2 of §1.6 lists it among the irreducible subsets of $\mathbb{A}^2$) and $I(\varnothing) = k[X]$ is "prime". Mathlib's `IsIrreducible` requires nonemptiness. Proposition 1 of §1.5 is true in both conventions, but the library should follow Mathlib's and exclude $\varnothing$.
- **Irreducible elements** exclude $0$ and units in Fulton; Mathlib's `Irreducible` excludes units, and $0$ is never irreducible in a nontrivial domain. No trap.
- **"No common factors"** (Proposition 2 of §1.6, and Chapter 3 throughout) is Mathlib's `IsRelPrime F G` (every common divisor is a unit), **not** `IsCoprime F G` (which asks for $AF + BG = 1$). In $k[X,Y]$ the polynomials $X$ and $Y$ are relatively prime but not coprime. In a PID the two agree (`isRelPrime_iff_isCoprime`, Mathlib/RingTheory/PrincipalIdealDomain.lean), which is how Fulton's proof of Proposition 2 passes to $k(X)[Y]$.
- **Degrees.** Fulton lets the zero polynomial have any degree (p. 2) and calls $0$ a form of every degree. Mathlib: `MvPolynomial.totalDegree 0 = 0`, `Polynomial.degree 0 = ⊥`, `Polynomial.natDegree 0 = 0`, and `MvPolynomial.IsHomogeneous 0 n` holds for every `n`. Statements about $f^*$, $F_*$ and degrees must treat $0$ separately.
- **Points and algebraic sets.** $\mathbb{A}^n(k)$ is `Fin n → k`. Fulton's $V(S)$ is Mathlib's `MvPolynomial.zeroLocus k (Ideal.span S)` (`MvPolynomial.zeroLocus_span`), and $I(X)$ is `MvPolynomial.vanishingIdeal k X` (Mathlib/RingTheory/Nullstellensatz.lean). Mathlib's versions take coefficients in a field $k$ and points in a $k$-algebra field $K$ (`zeroLocus K I : Set (σ → K)`); Fulton's are the case $K = k$. Fulton's "hypersurface $V(F)$" requires $F$ nonconstant. Mathlib has **no Zariski topology on `σ → k`**: Tau Ceti's `MvPolynomial.zariskiClosure` (TauCeti/RingTheory/MvPolynomial/ZariskiClosure.lean) is a closure operator whose closed sets are the zero loci, but irreducibility and irreducible components exist only on `PrimeSpectrum`, `MaximalSpectrum` (with its `MaximalSpectrum.zariskiTopology`) and schemes, not for subsets of $k^n$.
- **Coordinate rings and rational functions.** $\Gamma(V) = k[X_1,\dots,X_n]/I(V)$ is `MvPolynomial (Fin n) k ⧸ MvPolynomial.vanishingIdeal k V`; $k(V)$ is `FractionRing Γ(V)`. Neither library has a type of classical affine varieties.
- **Local rings.** Fulton defines $\mathcal{O}_P(V)$ as a subring of $k(V)$, only for a variety $V$ (§2.4). Mathlib's `Localization.AtPrime 𝔪` is an abstract ring with an `IsLocalization` structure; `Localization.subalgebra.ofField` realizes it as a subalgebra of the fraction field when subrings of $k(V)$ must be compared (Proposition 2 of §2.4). Chapter 3 needs local rings of reducible and non-reduced curves, which Fulton handles as $\mathcal{O}_P(\mathbb{A}^2)/(F)$; the milestones below are stated so that this is covered.
- **Orders of DVRs.** Fulton's $\operatorname{ord}\colon K \to \mathbb{Z}\cup\{\infty\}$ has $\operatorname{ord}(0) = \infty$ and $\operatorname{ord} \ge 0$ on $R$. Mathlib's `IsDiscreteValuationRing.addVal R : AddValuation R ℕ∞` agrees with it on $R$. On the fraction field Mathlib uses multiplicative valuations into `ℤᵐ⁰`: the adic valuation `(IsDiscreteValuationRing.maximalIdeal R).valuation K` sends $x$ to $\exp(-\operatorname{ord} x)$ (order reversed, $\le 1$ on $R$), while `Ring.ordFrac R : K →*₀ ℤᵐ⁰` sends $x \ne 0$ to $\exp(\operatorname{ord} x)$ and $0$ to $0$, which is $-\infty$, not $+\infty$, in additive terms (Mathlib/RingTheory/OrderOfVanishing/Basic.lean). A library-facing `ord` valued in `WithTop ℤ` with $\operatorname{ord}(0) = \top$ should be a thin wrapper.
- **Dimensions.** Fulton writes $\dim_k$ and allows the value $\infty$. Mathlib's `Module.finrank` returns `0` on infinite-dimensional spaces, so it silently breaks statements such as "$\dim_k \mathcal{O}/(F,G) = \infty$ when $F,G$ share a component". Use `Module.length` (valued in `ℕ∞`) over the local ring, or `Module.rank`, and convert to $\dim_k$ when the residue field is $k$ (`IsLocalRing.length_restrictScalars`, Mathlib/RingTheory/LocalRing/Length.lean; Tau Ceti `TauCeti.finrank_eq_length_mul_finrank_residueField`, TauCeti/RingTheory/Length.lean).
- **Partial derivatives** $F_{X_i}$ are `MvPolynomial.pderiv i F`.
- **Homogenization.** Fulton's $F_* = F(X_1,\dots,X_n,1)$ dehomogenizes with respect to the *last* variable, and $f^* = X_{n+1}^{\deg f} f(X_1/X_{n+1},\dots)$ homogenizes *to the degree of $f$*. Tau Ceti's `TauCeti.dehomogenize i` works for every coordinate `i : Fin (n + 1)`, keeping the others in increasing order via `i.succAbove`; for the plane, Fulton's $F(X,Y,1)$ is `TauCeti.dehomogenize 2 F`. The existing homogenization operations (Mathlib's one-variable `Polynomial.homogenize p n`, Tau Ceti's `MvPolynomial.blockHomogenize` and its Rees-algebra `MvPolynomial.weightHomogenization`) homogenize to a *prescribed* degree, and place the new variable differently (last in `Polynomial.homogenize`, first in `blockHomogenize` on `Unit ⊕ σ`). See the milestone on homogenization.

### Advice on representing the page's notions

These are suggestions; the builders decide.

- Polynomials: `MvPolynomial (Fin n) k`; in the plane `MvPolynomial (Fin 2) k` with $X$ = `X 0`, $Y$ = `X 1`, and $Z$ = `X 2` in `MvPolynomial (Fin 3) k`, matching Tau Ceti's charts. Pass to `k[X][Y]` with Mathlib's `Polynomial.Bivariate.equivMvPolynomial` when an argument needs one-variable polynomials over $k[X]$ or $k(X)$ (Gauss's lemma, resultants).
- Algebraic sets and their ideals: Mathlib's `MvPolynomial.zeroLocus` and `MvPolynomial.vanishingIdeal`. State irreducibility algebraically (prime vanishing ideal) rather than building a Zariski topology on `Fin n → k` just for this page.
- Local rings: $\mathcal{O}_P(\mathbb{A}^n)$ = `Localization.AtPrime (MvPolynomial.vanishingIdeal k {P})`, and for an ideal $J$ (a curve $F$, a pair $(F,G)$) work with the quotient $\mathcal{O}_P(\mathbb{A}^n)/J\,\mathcal{O}_P(\mathbb{A}^n)$; the milestone on local rings of subvarieties identifies this with Fulton's $\mathcal{O}_P(V)/J'\mathcal{O}_P(V)$.
- Multiplicities and colengths: `Module.length` over the local ring, valued in `ℕ∞`.
- State the algebra at its natural generality (any field, often any Noetherian ring) and derive Fulton's form; keep points $k$-rational and $k$ algebraically closed only where the statement needs points.

## Milestones

### §1.6 Algebraic Subsets of the Plane

> [!milestone] Coprime plane polynomials have finitely many common zeros
> Let $k$ be any field and $F, G \in k[X,Y]$ with no common nonconstant factor (`IsRelPrime F G`; if one of them is $0$, the other must be a nonzero constant). Then:
> (a) the ideal $(F,G)$ contains a nonzero $D \in k[X]$ and a nonzero $E \in k[Y]$;
> (b) $V(F,G) \subset \mathbb{A}^2(k)$ is finite, with at most $\deg D \cdot \deg E$ points, and the same holds for the common zeros in $K^2$ for any extension field $K$ of $k$;
> (c) $k[X,Y]/(F,G)$ is finite-dimensional over $k$, of dimension at most $\deg D\cdot\deg E$.
> If $F$ or $G$ is a nonzero constant, $(F,G) = (1)$, $V(F,G) = \varnothing$ and the dimension is $0$.
>
> Sources: [@Fulton2008, §1.6, Proposition 2] (states (b) over any field; (a) is in its proof).
>
> Proof routes: The book's: no common factor in $k[X][Y]$ implies none in $k(X)[Y]$ (Gauss's lemma, §1.1), which is a PID, so $RF + SG = 1$ there; clearing denominators gives $AF + BG = D \in k[X] \setminus 0$; every common zero has $X$-coordinate a root of $D$; the same with the roles of $X$ and $Y$ exchanged. Part (c) follows from (a) without the Nullstellensatz: $k[X,Y]/(F,G)$ is a quotient of $k[X,Y]/(D(X),E(Y)) \cong k[X]/(D)\otimes_k k[Y]/(E)$, of dimension $\deg D\cdot\deg E$. An alternative for (a) is the resultant $\operatorname{Res}_Y(F,G)$, which lies in $(F,G)\cap k[X]$; it needs case distinctions when $F$ or $G$ has $Y$-degree $0$, so the Gauss-lemma route is simpler.
>
> Formalization notes: Aim for (a) and (c) over an arbitrary field: the book reaches finite-dimensionality of $k[X,Y]/(F,G)$ (property (1) of intersection numbers in §3.3) only through Corollary 4 of the Nullstellensatz and Corollary 1 of §2.9, and (c) gives it directly. For the Gauss step use Tau Ceti's `IsRelPrime.map_fraction_map`, or Mathlib's `Polynomial.IsPrimitive.dvd_of_fraction_map_dvd_fraction_map` or `Polynomial.IsPrimitive.irreducible_iff_irreducible_map_fraction_map` (Mathlib/RingTheory/Polynomial/GaussLemma.lean) over `FractionRing k[X]`, then `isRelPrime_iff_isCoprime` in the PID. For (c), `AdjoinRoot.powerBasis` gives the dimension of each one-variable factor. Pitfall: formalizing "no common factor" as `IsCoprime` makes the statement vacuous for most curves (see Conventions).
>
> Lean status: Mathlib —. Tau Ceti —. **Related**: Mathlib `Polynomial.exists_mul_add_mul_eq_C_resultant` and `Polynomial.resultant_ne_zero` (Mathlib/RingTheory/Polynomial/Resultant/Basic.lean) give the resultant route over a domain, but `resultant_ne_zero` assumes `IsCoprime f g`, which fails for relatively prime $F,G$ in $k[X][Y]$, so it applies only after passing to the PID $k(X)[Y]$; `Polynomial.Bivariate.equivMvPolynomial` (Mathlib/Algebra/Polynomial/Bivariate.lean). Tau Ceti: `IsRelPrime.map_fraction_map` (TauCeti/RingTheory/Polynomial/GaussLemma.lean), relatively prime in $R[X]$ over a GCD domain implies relatively prime in $K[X]$, which is exactly the book's Gauss step; `Ideal.height_span_pair_eq_two_of_isRelPrime` (TauCeti/RingTheory/Ideal/Height.lean), height $2$ for $(x,y)$ relatively prime and proper in a Noetherian UFD, another route to (c) with `Module.finite_iff_krullDimLE_zero`.

> [!milestone] Reduced equations and irreducible components of affine hypersurfaces
> Let $k$ be algebraically closed and $F \in k[X_1,\dots,X_n]$ nonconstant, with factorization $F = \mu \prod_{i=1}^r F_i^{e_i}$ into pairwise non-associated irreducible $F_i$, $\mu \in k^\times$, $e_i \ge 1$. Then $I(V(F)) = (F_1\cdots F_r) = \operatorname{Rad}(F)$, and $V(F) = V(F_1)\cup\dots\cup V(F_r)$ is the decomposition of $V(F)$ into irreducible components (each $V(F_i)$ is irreducible, none contains another). Hence $V(F) = V(G)$ iff $F$ and $G$ have the same irreducible factors up to scalars, and $F \mapsto V(F)$ is a bijection from irreducible polynomials up to $k^\times$ to irreducible hypersurfaces. The multiplicities $e_i$ are not recovered from $V(F)$.
> In the plane over any **infinite** field: if $F$ is irreducible and $V(F)$ is infinite then $I(V(F)) = (F)$ and $V(F)$ is irreducible; and the irreducible algebraic subsets of $\mathbb{A}^2(k)$ are $\mathbb{A}^2(k)$, the points, and the $V(F)$ with $F$ irreducible and $V(F)$ infinite (and $\varnothing$ in Fulton's convention). Over non-closed fields $V(F)$ can be finite or empty (Problems 1.26, 1.30), which is why the first part needs algebraic closure.
>
> Sources: [@Fulton2008, §1.6, Corollaries 1–3], [@Fulton2008, §1.7, Corollary 3].
>
> Proof routes: The book's: in the plane, from Proposition 2 of §1.6 (a polynomial vanishing on infinitely many points of $V(F)$ is divisible by $F$) and Problem 1.14★ ($V(F)$ infinite when $k$ is algebraically closed); in $n$ variables, from the Nullstellensatz. Library route: Mathlib's `MvPolynomial.vanishingIdeal_zeroLocus_eq_radical` gives $I(V(F)) = \operatorname{Rad}(F)$, and `Ideal.radical_span_singleton_eq_span_radical` gives $\operatorname{Rad}(F) = (\operatorname{rad} F)$ with `UniqueFactorizationMonoid.radical F` the product of the distinct prime factors; the components correspond to the minimal primes $(F_i)$ over $(F)$.
>
> Formalization notes: This is the bridge between the plane-curve library's curves (polynomials up to scalars, multiple components allowed, as in §3.1) and point sets; it is what makes "the components of $F$ can be recovered from $V(F)$" a theorem. `Ideal.radical_span_singleton_eq_span_radical` needs a `NormalizationMonoid` instance, which Mathlib does not provide for `MvPolynomial`; use `UniqueFactorizationMonoid.strongNormalizationMonoid` locally or state the result up to `Associated`. Express irreducibility of $V(F_i)$ as primality of $I(V(F_i))$ (see Conventions). The statement depends on the earlier milestone on coprime plane polynomials in the planar part.
>
> Lean status: Mathlib **partial**: `MvPolynomial.vanishingIdeal_zeroLocus_eq_radical` (Mathlib/RingTheory/Nullstellensatz.lean) and `Ideal.radical_span_singleton_eq_span_radical` (Mathlib/RingTheory/Nilpotent/Lemmas.lean) compose to $I(V(F)) = (\operatorname{rad} F)$; the component decomposition, the bijection and the planar statements over infinite fields are absent. Tau Ceti —.

### §1.7 Hilbert's Nullstellensatz

> [!milestone] Finite zero sets and finite-dimensional quotients of polynomial rings
> Let $k$ be a field and $I$ an ideal of $k[X_1,\dots,X_n]$.
> (a) For any field $k$: the number of points of $V(I)\subset\mathbb{A}^n(k)$ is at most $\dim_k k[X_1,\dots,X_n]/I$ (vacuous if the dimension is infinite).
> (b) If $k$ is algebraically closed: $V(I)$ is finite iff $k[X_1,\dots,X_n]/I$ is finite-dimensional.
> Degenerate case: $I = (1)$ gives $V(I) = \varnothing$ and dimension $0$. Over a non-closed field (b) fails: $(X^2+1)\subset\mathbb{R}[X,Y]$ has no real zeros and an infinite-dimensional quotient. The natural general form of (b): for any field $k$, $k[X]/I$ is finite-dimensional iff $I$ has finitely many zeros in $\bar k^{\,n}$.
>
> Sources: [@Fulton2008, §1.7, Corollary 4].
>
> Proof routes: The book's: (a) polynomials $F_i$ with $F_i(P_j) = \delta_{ij}$ (Problem 1.17★) have linearly independent residues; (b) if $V(I) = \{P_1,\dots,P_r\}$ then $\prod_i (X_j - a_{ij})$ vanishes on $V(I)$, so a power lies in $I$ by the Nullstellensatz, and the monomials with all exponents below a fixed bound span the quotient. Alternative for (b): `Module.finite_iff_krullDimLE_zero` (a finite-type algebra over a field is finite iff it has Krull dimension $0$) together with the Nullstellensatz identification of maximal ideals with points. The book's route is elementary and probably shorter.
>
> Formalization notes: Use `Set.encard`/`Set.ncard` and `Module.rank` (or a `Module.Finite` hypothesis) for (a); do not state it with `Module.finrank` alone, which is `0` in the infinite case. Uses Problem 1.17★, for which neither library has a lemma. Later uses: property (1) of intersection numbers in §3.3 (a curve's coordinate ring is infinite-dimensional), the finiteness hypothesis of the decomposition milestone of §2.9, and Chapters 5–8. In the plane, the milestone on coprime plane polynomials already gives the finiteness that Chapter 3 needs, over any field.
>
> Lean status: Mathlib **partial**: `Module.finite_iff_krullDimLE_zero` (Mathlib/RingTheory/Jacobson/Artinian.lean) is the ring-theoretic core of (b); with `MvPolynomial.isMaximal_iff_eq_vanishingIdeal_singleton` (Mathlib/RingTheory/Nullstellensatz.lean) it gives (b). (a) —. Tau Ceti —.

### §2.4 Rational Functions and Local Rings

> [!milestone] Local ring of an affine variety at a point
> Let $k$ be algebraically closed, $V \subset \mathbb{A}^n$ a nonempty variety, $P \in V$, and $\mathfrak{m}_P = \{a \in \Gamma(V) : a(P) = 0\}$. Then:
> (a) $\mathcal{O}_P(V) = \{f \in k(V) : f = a/b,\ a,b\in\Gamma(V),\ b(P)\ne 0\}$ is a subring of $k(V)$ containing $\Gamma(V)$, canonically isomorphic to the localization $\Gamma(V)_{\mathfrak{m}_P}$;
> (b) $f \mapsto f(P)$ is a well-defined surjective $k$-algebra map $\mathcal{O}_P(V)\to k$ whose kernel $\mathfrak{m}_P(V)$ is the set of non-units, so $\mathcal{O}_P(V)$ is local with residue field $k$;
> (c) $\mathcal{O}_P(V)$ is a Noetherian local domain (Proposition 3);
> (d) the pole set of $f \in k(V)$ is an algebraic subset of $V$, and $\Gamma(V) = \bigcap_{P\in V}\mathcal{O}_P(V)$ in $k(V)$ (Proposition 2);
> (e) a polynomial map $\varphi\colon V\to W$ with $\varphi(P) = Q$ induces a unique local homomorphism $\tilde\varphi\colon\mathcal{O}_Q(W)\to\mathcal{O}_P(V)$ extending $\Gamma(W)\to\Gamma(V)$ (Problem 2.21★); for an affine change of coordinates $T$ with $T(P) = Q$ this gives isomorphisms $\mathcal{O}_Q(\mathbb{A}^n)\cong\mathcal{O}_P(\mathbb{A}^n)$ and $\mathcal{O}_Q(V)\cong\mathcal{O}_P(V^T)$ (Problem 2.22★);
> (f) prime ideals of $\mathcal{O}_P(V)$ correspond to subvarieties of $V$ through $P$ (Problem 2.18).
>
> Sources: [@Fulton2008, §2.4, Proposition 2], [@Fulton2008, §2.4, Proposition 3], [@Fulton2008, §2.4, Lemma] (p. 21), [@Fulton2008, §2.4, Problems 2.18, 2.21 and 2.22] (p. 22; 2.21 and 2.22 starred).
>
> Proof routes: The book's: (c) because an ideal $I$ of $\mathcal{O}_P(V)$ is generated by $I\cap\Gamma(V)$, and $\Gamma(V)$ is Noetherian (Problem 1.22★); (d) the "denominator ideal" $J_f = \{G : Gf\in\Gamma(V)\}$ has zero set the pole set, and if it is empty then $1\in J_f$ by the Nullstellensatz. Library route: (a)–(c) are the localization API; (d) is Mathlib's `MaximalSpectrum.iInf_localization_eq_bot` (the intersection over all maximal ideals, proved without the Nullstellensatz) plus the Nullstellensatz identification of maximal ideals of $\Gamma(V)$ with points; (f) is `IsLocalization.AtPrime.orderIsoOfPrime` plus Problem 2.2★.
>
> Formalization notes: Define $\mathcal{O}_P$ as `Localization.AtPrime` of the maximal ideal of $P$, for **any** ideal $J$ with $P \in V(J)$ (that is, for $k[X]/J$, not only for prime $I(V)$): the plane-curve library needs $\mathcal{O}_P(F)$ for reducible and non-reduced $F$. Use `Localization.subalgebra.ofField` only where subrings of $k(V)$ for different points must be compared, as in (d). Over a non-closed field the definitions work verbatim at $k$-rational points. Residue field: `IsLocalization.AtPrime.equivQuotMaximalIdeal` together with $k[X]/\mathfrak{m}_P\cong k$. The book's proof of (c) uses the starred Problem 1.22, and (d) uses the Nullstellensatz. The next milestone (local rings of subvarieties as quotients) is the form in which Chapter 3 uses all this.
>
> Lean status: Mathlib **partial**: for any Noetherian domain and prime ideal, (c) is `IsLocalization.isNoetherianRing` (Mathlib/RingTheory/Localization/Submodule.lean), the instance `Localization.AtPrime.isLocalRing` (Mathlib/RingTheory/Localization/AtPrime/Basic.lean) and `IsLocalization.isDomain_localization` (Mathlib/RingTheory/Localization/Defs.lean); (d)'s intersection is `MaximalSpectrum.iInf_localization_eq_bot` (Mathlib/RingTheory/Spectrum/Maximal/Localization.lean) indexed by maximal ideals; (e)'s map is `Localization.localRingHom`; (f) is `IsLocalization.AtPrime.orderIsoOfPrime`; the subring realization is `Localization.subalgebra.ofField` (Mathlib/RingTheory/Localization/AsSubring.lean). Nothing ties these to points of $V\subset k^n$; the pole set statement is absent. Tau Ceti —.

### §2.5 Discrete Valuation Rings

> [!milestone] Discrete valuation rings via principal maximal ideals
> Let $R$ be a domain that is not a field, with fraction field $K$. The following are equivalent: (1) $R$ is Noetherian and local and its maximal ideal is principal; (2) there is an irreducible $t\in R$ such that every nonzero $z\in R$ is $z = ut^n$ for a unique unit $u$ and a unique $n\ge 0$. Such an $R$ is a discrete valuation ring; any $t$ as in (2) is a uniformizing parameter, and the others are the $ut$ with $u$ a unit. Every $z \in K^\times$ is uniquely $ut^n$ with $n\in\mathbb{Z}$, and $\operatorname{ord}(z) = n$, $\operatorname{ord}(0) = \infty$, defines a function independent of $t$ (Problem 2.23★) with $R = \{\operatorname{ord}\ge 0\}$ and $\mathfrak{m} = \{\operatorname{ord} > 0\}$. Moreover:
> - order functions on $K$ (surjective maps $K\to\mathbb{Z}\cup\{\infty\}$ that are $\infty$ only at $0$, additive on products and satisfying $\varphi(a+b)\ge\min$) correspond bijectively to DVRs with fraction field $K$ (Problem 2.28★);
> - if $\operatorname{ord}(a) < \operatorname{ord}(b)$ then $\operatorname{ord}(a+b) = \operatorname{ord}(a)$; a sum in which one term has strictly smallest order is nonzero (Problem 2.29★);
> - if $z\in K\setminus R$ then $z^{-1}\in\mathfrak{m}$; and if $R\subset S\subset K$ with $S$ a DVR whose maximal ideal contains $\mathfrak{m}$, then $S = R$ (Problem 2.26★).
>
> Sources: [@Fulton2008, §2.5, Proposition 4] (p. 22), [@Fulton2008, §2.5, Problems 2.23, 2.26, 2.28 and 2.29] (p. 23; all starred).
>
> Proof routes: The book's (1)⇒(2): a generator $t$ of $\mathfrak{m}$ divides every non-unit, and an infinite chain $z = z_1t$, $z_1 = z_2t,\dots$ gives a strictly increasing chain of principal ideals, contradicting Noetherianity. (2)⇒(1) is marked "we don't really need this part", but Problem 2.28★ and later chapters do use it. Mathlib proves all of this through `IsDiscreteValuationRing.TFAE`.
>
> Formalization notes: **Take from Mathlib.** Fulton's (1) is item 4 of `IsDiscreteValuationRing.TFAE` (for a Noetherian local domain that is not a field), so `(IsDiscreteValuationRing.TFAE R h).out 0 4` is Fulton's equivalence; (2) is `IsDiscreteValuationRing.eq_unit_mul_pow_irreducible` with uniqueness `IsDiscreteValuationRing.unit_mul_pow_congr_pow`, and its converse `IsDiscreteValuationRing.ofHasUnitMulPowIrreducibleFactorization`. Mathlib's class `IsDiscreteValuationRing` (a local PID that is not a field) needs an `IsDomain` instance. Mind the sign and infinity conventions of Mathlib's valuations (see Conventions). Later uses: Theorem 1 of §3.2 (a point is simple iff its local ring is a DVR), property (8) of §3.3, and the places, divisors and orders of Chapters 7–8. Depends on the milestone on the local ring of an affine variety for Noetherianity of $\mathcal{O}_P$.
>
> Lean status: Mathlib **full**: `IsDiscreteValuationRing.TFAE` (Mathlib/RingTheory/DiscreteValuationRing/TFAE.lean); `IsDiscreteValuationRing.eq_unit_mul_pow_irreducible`, `IsDiscreteValuationRing.unit_mul_pow_congr_pow`, `IsDiscreteValuationRing.ofHasUnitMulPowIrreducibleFactorization`, `IsDiscreteValuationRing.addVal`, `IsDiscreteValuationRing.addVal_add` (Mathlib/RingTheory/DiscreteValuationRing/Basic.lean); Problem 2.28★ in Mathlib's multiplicative language: `Valuation.valuationSubring_isDiscreteValuationRing` (Mathlib/RingTheory/Valuation/Discrete/Basic.lean) and `IsDiscreteValuationRing.maximalIdeal` with the instance `IsDiscreteValuationRing.isRankOneDiscrete` (Mathlib/RingTheory/Valuation/Discrete/IsDiscreteValuationRing.lean); Problem 2.29★: `Valuation.map_add_eq_of_lt_left`, `Valuation.map_sum_eq_of_lt` (Mathlib/RingTheory/Valuation/Basic.lean); Problem 2.26★: `ValuationSubring.mem_or_inv_mem`, `ValuationSubring.nonunits_le_nonunits` (Mathlib/RingTheory/Valuation/ValuationSubring.lean). Tau Ceti: not needed.

### §2.6 Forms

> [!milestone] Homogenization and dehomogenization of polynomials
> Let $R$ be a domain. For a form $F\in R[X_1,\dots,X_{n+1}]$ put $F_* = F(X_1,\dots,X_n,1)$; for $f\in R[X_1,\dots,X_n]$ of degree $d$ with homogeneous components $f_0,\dots,f_d$ put $f^* = \sum_i X_{n+1}^{d-i}f_i$, a form of degree $d$ (and $0^* = 0$). Then:
> (1) $(FG)_* = F_*G_*$ and $(fg)^* = f^*g^*$;
> (2) if $F\ne 0$ and $X_{n+1}^r$ is the largest power of $X_{n+1}$ dividing $F$, then $X_{n+1}^r(F_*)^* = F$; and $(f^*)_* = f$;
> (3) $(F+G)_* = F_* + G_*$; and if $f$, $g$, $f+g$ are nonzero with $s = \deg f$, $r = \deg g$, $t = r+s-\deg(f+g)$, then $X_{n+1}^t(f+g)^* = X_{n+1}^r f^* + X_{n+1}^s g^*$.
> Consequently, up to powers of $X_{n+1}$, factorizations of a form $F$ correspond to factorizations of $F_*$: a form not divisible by $X_{n+1}$ is irreducible iff its dehomogenization is, and the factors of a form are forms.
>
> Sources: [@Fulton2008, §2.6, Proposition 5] (p. 24, proof left to the reader), [@Fulton2008, §2.6, Corollary] (first claim); [@Fulton2008, §1.1, Problem 1.1] (starred) for "factors of forms are forms".
>
> Proof routes: Direct computation on homogeneous components. In Lean, define the homogenization to a prescribed degree $N \ge \deg f$ as $\sum_{j\le N}(\text{component } j \text{ of } f)\cdot X_i^{N-j}$, which is the construction in Tau Ceti's proof of `TauCeti.exists_isHomogeneous_dehomogenize_eq`; Fulton's $f^*$ is the case $N = \deg f$. With $N$ fixed, homogenization is additive, (3) loses its correction term, and (2) reads "a form of degree $N$ is the degree-$N$ homogenization of its dehomogenization".
>
> Formalization notes: Make the degree-$N$ homogenization, for every coordinate `i : Fin (n + 1)`, the primary API, matching `TauCeti.dehomogenize i` (Chapters 4–5 dehomogenize with respect to any coordinate, and Tau Ceti's charts use `TauCeti.dehomogenize 2` in the plane); prove Fulton's (1)–(3) for $N = \deg f$ as corollaries. Multiplicativity for several variables exists only for Tau Ceti's Rees-algebra homogenization `MvPolynomial.weightHomogenization` (`TauCeti.MvPolynomial.weightHomogenization_mul`), which takes values in `(MvPolynomial σ R)[X]`; transporting it along Mathlib's `MvPolynomial.optionEquivLeft` or `MvPolynomial.finSuccEquiv` (Mathlib/Algebra/MvPolynomial/Equiv.lean; the latter makes `X 0` the new variable) and a `rename` to the last coordinate may be cheaper than reproving it. Connect to Tau Ceti's `MvPolynomial.blockHomogenize` (one block, `B = Unit`) by a `rename` lemma rather than duplicating it. Pitfalls: $F_*$ can drop degree (exactly when $X_{n+1}\mid F$); `totalDegree 0 = 0`. Later uses: the projective closure and $I^*$ in §4.3 and Chapter 5, both of which cite Proposition 5 explicitly; tangent lines in §3.1 through the milestone on binary forms, which depends on this one.
>
> Lean status: Mathlib **partial**: `Polynomial.homogenize p n` with `Polynomial.homogenize_mul`, `Polynomial.isHomogeneous_homogenize`, `Polynomial.homogenize_eq_of_isHomogeneous` (Mathlib/Algebra/Polynomial/Homogenize.lean): (1) and (2) for degree-$N$ homogenization in the one-variable case only. Tau Ceti **partial**: `TauCeti.dehomogenize` (an algebra map, so (1) and (3) for $F_*$), `TauCeti.dehomogenize_injOn`, `TauCeti.exists_isHomogeneous_dehomogenize_eq` (existence of a degree-$N$ homogenization, not a function) in TauCeti/RingTheory/MvPolynomial/Dehomogenization.lean; `MvPolynomial.blockHomogenize`, `MvPolynomial.blockHomogenize_add`, `MvPolynomial.aeval_blockHomogenize_one_X`, `MvPolynomial.blockHomogenize_aeval_one_X` (TauCeti/RingTheory/MvPolynomial/BlockHomogenize.lean), homogenization to prescribed degrees without a multiplicativity lemma; `MvPolynomial.IsHomogeneous.of_mul_left` and `MvPolynomial.IsHomogeneous.of_mul_right` (TauCeti/RingTheory/MvPolynomial/Homogeneous.lean) for factors of forms. Fulton's $f^*$ is `blockHomogenize` with one block and degree `f.totalDegree`, up to moving the new variable from first to last place; no lemma says so. **Related**: `MvPolynomial.weightHomogenization` with `TauCeti.MvPolynomial.weightHomogenization_mul` and `MvPolynomial.eval_one_weightHomogenization` (TauCeti/RingTheory/MvPolynomial/WeightFiltration.lean), a homogenization to prescribed weight $N$ with values in `(MvPolynomial σ R)[X]`, built for Rees algebras; with all weights $1$ it is degree-$N$ homogenization, and these lemmas are (1) and $(f^*)_* = f$ in that form (multiplicativity on polynomials of degree $\le m$ and $\le n$ into degree $m+n$); `Ideal.homogenization` (TauCeti/RingTheory/MvPolynomial/Homogenization.lean), the homogenization $I^*$ of ideals used in §4.3. Fulton's (1) for $f^*$ in `MvPolynomial (Fin (n + 1))`, (2) with the power of $X_{n+1}$, and the factorization corollary are absent.

> [!milestone] Binary forms over an algebraically closed field split into linear forms
> Let $k$ be algebraically closed and $F\in k[X,Y]$ a nonzero form of degree $d$. Then $F = \varepsilon\prod_{i=1}^d(a_iX + b_iY)$ with $\varepsilon\in k^\times$ and $(a_i,b_i)\ne(0,0)$; explicitly $F = \varepsilon Y^r\prod_{j=1}^{d-r}(X-\lambda_jY)$, where $r$ is the exponent of $Y$ in $F$ and the $\lambda_j$ are the roots of $F(X,1)$ with multiplicity. The linear factors are unique up to order and nonzero scalars. Over any field, a nonzero binary form is a product of linear forms iff $F(X,1)$ splits. For $d = 0$ the product is empty.
>
> Sources: [@Fulton2008, §2.6, Corollary] (second claim, p. 24).
>
> Proof routes: The book's: write $F = Y^rG$ with $Y\nmid G$; $G_* = F_*$ splits over $k$; rehomogenize using (2) of the previous milestone. Library route: Mathlib's `Polynomial.homogenize_eq_of_isHomogeneous` writes $F$ as the degree-$d$ homogenization of $F(X,1)$, and Tau Ceti's `Polynomial.homogenize_eq_C_mul_X_pow_mul_prod` evaluates the homogenization of a split polynomial as $\varepsilon X_1^{d-\deg}\prod(X_0 - aX_1)$.
>
> Formalization notes: State it in `MvPolynomial (Fin 2) k` with $X$ = `X 0`, $Y$ = `X 1`; uniqueness is unique factorization (`UniqueFactorizationMonoid.normalizedFactors`). Later uses: tangent lines at a point (§3.1 cites this Corollary: the lowest form of a curve at $P$ factors as $\prod L_i^{r_i}$), and the tangents $L_i$, $M_j$ fed into Problem 2.35★(c) in the proof of property (5) of §3.3. Depends on the homogenization milestone.
>
> Lean status: Mathlib **related**: `Polynomial.homogenize_eq_of_isHomogeneous` (Mathlib/Algebra/Polynomial/Homogenize.lean), and `IsAlgClosed.card_roots_eq_natDegree` (Mathlib/FieldTheory/IsAlgClosed/Basic.lean), which supplies the root-count hypothesis of Tau Ceti's lemma. Tau Ceti **partial**: `Polynomial.homogenize_eq_C_mul_X_pow_mul_prod`, and `MvPolynomial.IsHomogeneous.exists_linear_factors` for binary forms without squared linear factor whose dehomogenization splits (TauCeti/Algebra/Polynomial/Homogenize.lean). Composing the two libraries gives the statement; no single declaration states it.

### §2.8 Operations with Ideals

> [!milestone] Local rings of a subvariety as quotients of the local ring of affine space
> Let $k$ be a field, $P = (a_1,\dots,a_n)\in\mathbb{A}^n(k)$, $I_P = (X_1-a_1,\dots,X_n-a_n)$, $\mathcal{O} = \mathcal{O}_P(\mathbb{A}^n) = k[X]_{I_P}$, $\mathfrak{m} = \mathfrak{m}_P(\mathbb{A}^n)$.
> (a) $I_P\mathcal{O} = \mathfrak{m}$ and $I_P^r\mathcal{O} = \mathfrak{m}^r$ for all $r$ (Problem 2.43★, stated at the origin); and $k[X]/I_P^r\cong\mathcal{O}/\mathfrak{m}^r$.
> (b) Let $I\subset J$ be ideals of $k[X]$ with $P\in V(J)$, $J'$ the image of $J$ in $k[X]/I$, and $\mathcal{O}_P(k[X]/I)$ the localization of $k[X]/I$ at the image of $I_P$. Then the natural map $\mathcal{O}/J\mathcal{O}\to\mathcal{O}_P(k[X]/I)/J'\mathcal{O}_P(k[X]/I)$ is an isomorphism; in particular $\mathcal{O}/I\mathcal{O}\cong\mathcal{O}_P(k[X]/I)$. For $k$ algebraically closed and $I = I(V)$ with $V$ a variety through $P$ this is Fulton's $\mathcal{O}_P(\mathbb{A}^n)/J\mathcal{O}_P(\mathbb{A}^n)\cong\mathcal{O}_P(V)/J'\mathcal{O}_P(V)$ and $\mathcal{O}_P(\mathbb{A}^n)/I(V)\mathcal{O}_P(\mathbb{A}^n)\cong\mathcal{O}_P(V)$ (Problem 2.44★).
> General form: for a ring $A$, ideals $I\subset J$ and a prime $\mathfrak{p}\supset I$, $(A/I)_{\mathfrak{p}}\cong A_{\mathfrak{p}}/IA_{\mathfrak{p}}$ compatibly with $J$; and for a maximal $\mathfrak{p}$, $A/\mathfrak{p}^r\cong A_{\mathfrak{p}}/\mathfrak{p}^rA_{\mathfrak{p}}$.
>
> Sources: [@Fulton2008, §2.8, Problems 2.43 and 2.44] (p. 26; both starred), with the natural maps of [@Fulton2008, §2.8, Problem 2.42] (starred).
>
> Proof routes: Localization commutes with quotients (exactness of localization); (a) because every element of $\mathfrak{m}$ is $a/b$ with $a\in I_P$ and $b$ a unit. Fulton obtains $k[X]/I_P^r\cong\mathcal{O}/\mathfrak{m}^r$ from Corollary 2 of §2.9; Mathlib has it directly.
>
> Formalization notes: State (b) for arbitrary ideals $I\subset J$, not only $I = I(V)$ prime: Chapter 3 applies it to $(F)\subset(F,G)$ with $F$ reducible, in the form $\mathcal{O}_P(\mathbb{A}^2)/(F,G)\cong\mathcal{O}_P(F)/(g)$ (property (8) of §3.3), and Chapter 7 to projective curves. For (a) at a general point, translate to the origin with Tau Ceti's Taylor shift `MvPolynomial.taylor`. Tau Ceti's `TauCeti.ord_eq_length_quotient_span_pair` (TauCeti/RingTheory/Intersection.lean) is the length identity that, combined with (b), turns $\dim_k\mathcal{O}/(F,G)$ into an order of vanishing on $\mathcal{O}/(F)$.
>
> Lean status: Mathlib **partial**: the instance `IsLocalization (Algebra.algebraMapSubmonoid (R ⧸ I) M) (S ⧸ I.map (algebraMap R S))` (Mathlib/RingTheory/Localization/Ideal.lean) is the general form of (b) for $J = I$, and with `DoubleQuot.quotQuotEquivQuotOfLE` (Mathlib/RingTheory/Ideal/Quotient/Operations.lean) gives (b); `IsLocalization.AtPrime.equivQuotMaximalIdealPow` and `IsLocalization.AtPrime.map_eq_maximalIdeal` (Mathlib/RingTheory/Localization/AtPrime/Basic.lean) give $A/\mathfrak{p}^r\cong A_{\mathfrak{p}}/\mathfrak{m}^r$ and $\mathfrak{p}A_{\mathfrak{p}} = \mathfrak{m}$. Tau Ceti **related**: `MvPolynomial.ker_constantCoeff` (TauCeti/RingTheory/MvPolynomial/Ideal.lean: the ideal of the origin is generated by the variables), `MvPolynomial.mem_pow_vanishingIdeal_singleton_iff` (TauCeti/RingTheory/MvPolynomial/LocalOrder.lean: membership in $I_P^r$ by Taylor order). That $I(\{P\})$ is generated by the $X_i - a_i$ is not stated as a lemma in either library.

> [!milestone] Dimensions of spaces of forms and of polynomials modulo a power of the ideal of a point
> Let $k$ be a field.
> (a) The forms of degree $d$ in $k[X_1,\dots,X_n]$ form a $k$-vector space $V(d,n)$ with the monomials of degree $d$ as basis, and $\dim V(d,n) = \binom{d+n-1}{n-1}$; so $\dim V(d,2) = d+1$ and $\dim V(d,3) = (d+1)(d+2)/2$ (Problems 2.35★(a,b), 2.36).
> (b) If $L_1,L_2,\dots$ and $M_1,M_2,\dots$ are nonzero linear forms in $k[X,Y]$ with no $L_i$ proportional to any $M_j$ (repetitions among the $L_i$, or among the $M_j$, allowed), the products $A_{ij} = L_1\cdots L_iM_1\cdots M_j$ with $i+j = d$ form a basis of $V(d,2)$ (Problem 2.35★(c)).
> (c) Let $I = (X_1,\dots,X_n)$. Then $I^m$ consists of the polynomials with no terms of degree $<m$, the residues of the monomials of degree $<m$ form a basis of $k[X_1,\dots,X_n]/I^m$, and $\dim_k k[X_1,\dots,X_n]/I^m = \binom{m+n-1}{n}$; for $n = 2$ this is $m(m+1)/2$ (Example of §2.8, Problem 2.46★). The same holds for $I_P = (X_1-a_1,\dots,X_n-a_n)$ at any point by translation.
>
> Sources: [@Fulton2008, §2.6, Problems 2.35 and 2.36] (p. 25), [@Fulton2008, §2.8, Example] (p. 25) and [@Fulton2008, §2.8, Problem 2.46] (p. 26).
>
> Proof routes: (a) counting monomials (stars and bars). (b) modulo $L_1$ the forms of degree $d$ make a one-dimensional space, spanned by $A_{0d} = M_1\cdots M_d$ because no $M_j$ is proportional to $L_1$; so $V(d,2) = L_1V(d-1,2) + kA_{0d}$, and induction on $d$, applied to the shifted sequence $L_2, L_3,\dots$, shows that the $d+1$ products span, hence form a basis by (a). (c) from (a) summed over degrees $<m$. The book leaves all of it as exercises.
>
> Formalization notes: Use Mathlib's `MvPolynomial.homogeneousSubmodule (Fin n) k d` and `MvPolynomial.restrictTotalDegree`; for (c), the polynomials of total degree $<m$ map isomorphically onto $k[X]/I^m$. Note that Chapter 3 applies (b) with repeated tangents ($L_i = L_m$ for $i > m$), which the hypothesis allows. Later uses: Theorem 2 of §3.2 (Hilbert–Samuel computation), property (5) of §3.3 (via (b) and (c)), linear systems of curves of degree $d$ in §5.2 (dimension $(d+1)(d+2)/2$).
>
> Lean status: Tau Ceti **partial**: (a) in full via `TauCeti.homogeneousMonomialBasis`, `TauCeti.finrank_homogeneousSubmodule_eq_multichoose` (dimension `(Fintype.card σ).multichoose d`, which is $\binom{d+n-1}{n-1}$), `TauCeti.finrank_homogeneousSubmodule_fin_two`, and `TauCeti.finrank_restrictTotalDegree` (degree $\le m$: $\binom{m+n}{n}$), all in TauCeti/RingTheory/MvPolynomial/Finrank.lean. Mathlib **partial**: `MvPolynomial.mem_pow_idealOfVars_iff` (Mathlib/RingTheory/MvPolynomial/Ideal.lean) is the first claim of (c). The dimension of $k[X]/I^m$ and (b) are absent.

### §2.9 Ideals with a Finite Number of Zeros

> [!milestone] Global-to-local decomposition of a polynomial ring modulo an ideal with finitely many zeros
> Let $k$ be algebraically closed, $I\subset k[X_1,\dots,X_n]$ an ideal with $V(I) = \{P_1,\dots,P_N\}$ finite, and $\mathcal{O}_i = \mathcal{O}_{P_i}(\mathbb{A}^n)$. The maps $k[X]\to\mathcal{O}_i$ induce a $k$-algebra isomorphism
> $$k[X_1,\dots,X_n]/I\;\cong\;\prod_{i=1}^N\mathcal{O}_i/I\mathcal{O}_i .$$
> Corollary 1: $\dim_k k[X]/I = \sum_i\dim_k\mathcal{O}_i/I\mathcal{O}_i$ (finite by Corollary 4 of §1.7). Corollary 2: if $V(I) = \{P\}$ then $k[X]/I\cong\mathcal{O}_P(\mathbb{A}^n)/I\mathcal{O}_P(\mathbb{A}^n)$. If $V(I) = \varnothing$ then $I = (1)$ and both sides are $0$.
> General form, over any field: if $k[X]/I$ is finite-dimensional, then $k[X]/I\cong\prod_{\mathfrak{m}\supset I}k[X]_{\mathfrak{m}}/Ik[X]_{\mathfrak{m}}$ over the finitely many maximal ideals containing $I$; more generally $A/I\cong\prod_{\mathfrak{m}\supset I}A_{\mathfrak{m}}/IA_{\mathfrak{m}}$ for any ring $A$ and ideal $I$ with $A/I$ Artinian. For algebraically closed $k$ the $\mathfrak{m}$ are the $I_{P_i}$.
>
> Sources: [@Fulton2008, §2.9, Proposition 6, Corollaries 1 and 2] (p. 27).
>
> Proof routes: The book's: build orthogonal idempotents $e_i$ in $k[X]/I$ from separating polynomials (Problem 1.17★), using $\bigcap I_{P_j}^d\subset I$ (Nullstellensatz and Problem 2.41★) and comaximality (Problems 2.40★, 2.45★), then check injectivity and surjectivity by hand. Library route, much shorter: $k[X]/I$ is Artinian (`IsArtinianRing.of_finite`), so `PrimeSpectrum.toPiLocalizationEquiv` decomposes it as the product of its localizations; each localization is $\mathcal{O}_i/I\mathcal{O}_i$ by the localization-of-quotient instance (milestone on local rings of subvarieties); the primes of $k[X]/I$ are the $I_{P_i}$ by `MvPolynomial.isMaximal_iff_eq_vanishingIdeal_singleton`. Corollary 1 is `IsArtinianRing.finrank_eq_sum_primeSpectrum`.
>
> Formalization notes: Index the product by the finite set $V(I)$ of points, and make the isomorphism explicitly the map induced by $k[X]\to\mathcal{O}_i$: Chapter 5 (Noether's theorem) uses its components, not just an abstract isomorphism. Chapter 7 (Proposition 2 of §7.5) applies Corollary 1 to $\Gamma(U)/(g)$ for an affine variety $U$, which is an instance of $k[X]/(I(U),G)$, so the general Artinian form is the right one to prove. Later uses: properties (1), (5), (9) of §3.3, Theorem 2 of §3.2, Chapters 5 and 7. Depends on the milestones on finite zero sets (§1.7), the local ring of an affine variety and the local rings of subvarieties.
>
> Lean status: Mathlib **partial**: `PrimeSpectrum.toPiLocalizationEquiv` (Mathlib/RingTheory/Spectrum/Prime/Topology.lean), which applies to Artinian rings through the instance `DiscreteTopology (PrimeSpectrum R)` for `[IsArtinianRing R]` (Mathlib/RingTheory/Spectrum/Prime/Noetherian.lean); `IsArtinianRing.finrank_eq_sum_primeSpectrum` (same file); `IsArtinianRing.of_finite` (Mathlib/RingTheory/Artinian/Module.lean). Tau Ceti **partial**: `Ideal.finrank_quotient_eq_finsum` (TauCeti/RingTheory/Artinian/Finrank.lean) is Corollary 1 for a quotient of a *finite* $k$-algebra $A$ (not of $k[X]$); **related**: `TauCeti.Localization.AtPrime.exists_mk_algebraMap_eq_of_isMaximal` (TauCeti/RingTheory/Localization/ChineseRemainder.lean). Nothing is stated for polynomial rings and points.

### §2.10 Quotient Modules and Exact Sequences

> [!milestone] Additivity of dimension and length along exact sequences
> (1) For an exact sequence $0\to V'\to V\to V''\to0$ of finite-dimensional vector spaces, $\dim V = \dim V' + \dim V''$. (2) For an exact sequence $0\to V_1\to V_2\to V_3\to V_4\to 0$ of finite-dimensional spaces, $\dim V_4 = \dim V_3 - \dim V_2 + \dim V_1$. Together with the standard sequences of Problem 2.49★: $0\to N/P\to M/P\to M/N\to0$ for $P\subset N\subset M$; $\dim V/U = \dim V/W + \dim W/U$ for $U\subset W\subset V$ with $V/U$ finite-dimensional; $0\to I/J\to R/J\to R/I\to0$ for ideals $J\subset I$; and $0\to\mathfrak{m}^n/\mathfrak{m}^{n+1}\to\mathcal{O}/\mathfrak{m}^{n+1}\to\mathcal{O}/\mathfrak{m}^n\to0$ for a local ring $(\mathcal{O},\mathfrak{m})$. More generally, length is additive on short exact sequences of modules over any ring, with values in $\mathbb{N}\cup\{\infty\}$ and no finiteness hypothesis.
>
> Sources: [@Fulton2008, §2.10, Proposition 7] (p. 28), [@Fulton2008, §2.10, Problems 2.49 and 2.51] (pp. 28–29; 2.49 starred).
>
> Proof routes: Rank–nullity; the four-term case by splitting at the image of the middle map. Mathlib proves the length version by composition series.
>
> Formalization notes: **Take from Mathlib.** Prefer the `Module.length` form (`Module.length_eq_add_of_exact`) in the plane-curve library, since intersection numbers and colengths can be infinite; convert to $\dim_k$ with `Module.length_eq_finrank` or, over a local ring with residue field $k$, `IsLocalRing.length_restrictScalars`. Mathlib also has the alternating-sum form for finite exact sequences of finite-dimensional spaces (`Module.sum_neg_one_pow_finrank_eq_zero_of_exact`), which gives (2) and Problem 2.51 directly; or apply the short-exact version twice, as the book does. Later uses: Theorem 2 of §3.2, properties (5) and (6) of §3.3, the dimension counts of Chapter 5, the vector spaces $L(D)$ of Chapter 8.
>
> Lean status: Mathlib **full**: `Module.length_eq_add_of_exact`, `Module.length_eq_finrank` (Mathlib/RingTheory/Length.lean); `LinearMap.finrank_range_add_finrank_ker` (Mathlib/LinearAlgebra/FiniteDimensional/Lemmas.lean); `Submodule.finrank_quotient_add_finrank` (Mathlib/LinearAlgebra/Dimension/RankNullity.lean); `Submodule.quotientQuotientEquivQuotient` (Mathlib/LinearAlgebra/Isomorphisms.lean); `Submodule.liftQ` (Mathlib/LinearAlgebra/Quotient/Basic.lean); the alternating sum (part (2), Problem 2.51): `Module.sum_neg_one_pow_finrank_eq_zero_of_exact` (Mathlib/Algebra/Exact/Sequence.lean). Tau Ceti **related**: `TauCeti.length_quotient_eq_length_quotient_comap_subtype_add` (TauCeti/RingTheory/Length.lean), additivity along $P\subset N\subset M$ in length form; `TauCeti.finsum_neg_one_pow_finrank_eq_zero_of_exact`, `TauCeti.sum_range_neg_one_pow_finrank_eq_of_exact` (TauCeti/LinearAlgebra/Exact.lean).

> [!milestone] Order equals colength in a discrete valuation ring with residue field k
> Let $R$ be a DVR with maximal ideal $\mathfrak{m}$, uniformizer $t$ and fraction field $K$, containing a field $k$ such that $k\to R/\mathfrak{m}$ is an isomorphism. Then:
> (a) every $z\in R$ has a unique $\lambda\in k$ with $z-\lambda\in\mathfrak{m}$, and for every $n\ge0$ unique $\lambda_0,\dots,\lambda_n\in k$ and $z_n\in R$ with $z = \lambda_0+\lambda_1t+\dots+\lambda_nt^n+z_nt^{n+1}$ (Problem 2.30★);
> (b) $\dim_k\mathfrak{m}^n/\mathfrak{m}^{n+1} = 1$ for all $n\ge0$, and $\dim_k R/\mathfrak{m}^n = n$ for all $n\ge0$;
> (c) for $z\ne0$, $\operatorname{ord}(z) = n$ iff $(z) = \mathfrak{m}^n$, hence $\operatorname{ord}(z) = \dim_k R/(z)$; for $z = 0$ both sides are $\infty$ (Problem 2.50★).
> General form, without the field $k$: $\operatorname{length}_R R/(z) = \operatorname{ord}(z)$ and $\operatorname{length}_R R/\mathfrak{m}^n = n$ in any DVR.
>
> Sources: [@Fulton2008, §2.10, Problem 2.50] (p. 29), [@Fulton2008, §2.5, Problem 2.30] (p. 23).
>
> Proof routes: $(z) = \mathfrak{m}^{\operatorname{ord} z}$; multiplication by $t^n$ identifies $R/\mathfrak{m}$ with $\mathfrak{m}^n/\mathfrak{m}^{n+1}$; then the exact sequence of Problem 2.49★(e) and induction. In Lean: Mathlib's length statements, converted to $\dim_k$ because the residue field is $k$.
>
> Formalization notes: State over a $k$-algebra `R` with `IsDiscreteValuationRing R` and `algebraMap k (IsLocalRing.ResidueField R)` bijective; derive the $\dim_k$ forms from Mathlib's length forms via `IsLocalRing.length_restrictScalars` or Tau Ceti's `TauCeti.finrank_eq_length_mul_finrank_residueField`. Part (a) is the start of power-series expansions (Problem 2.32, used in Chapter 8). Later uses: property (8) of §3.3 ($I(P,F\cap G) = \operatorname{ord}_P^F(G)$), Theorem 1 of §3.2, degrees of divisors in Chapter 8. Depends on the DVR milestone and the exact-sequence milestone.
>
> Lean status: Mathlib **partial**: `IsDiscreteValuationRing.length_quotient_pow_maximalIdeal` (Mathlib/RingTheory/DiscreteValuationRing/Basic.lean: $\operatorname{length}_R R/\mathfrak{m}^n = n$); `Ring.ord` (defined as `Module.length R (R ⧸ Ideal.span {x})`, Mathlib/RingTheory/OrderOfVanishing/Basic.lean) with `Ring.ord_eq_addVal` (Mathlib/RingTheory/OrderOfVanishing/Noetherian.lean), which is the general form of (c); `IsLocalRing.finrank_CotangentSpace_eq_one` (Mathlib/RingTheory/DiscreteValuationRing/TFAE.lean) is (b) for $n = 1$. Tau Ceti **partial**: `TauCeti.finrank_eq_length_mul_finrank_residueField` (TauCeti/RingTheory/Length.lean) converts lengths to $\dim_k$; (a) only for rational places of function fields: `TauCeti.Place.existsUnique_sub_sum_mem_filtration` (TauCeti/FieldTheory/FunctionField/Place/Expansion/Basic.lean).

## Correspondence with Mathlib and Tau Ceti

★ marks a starred problem: Fulton's preface says the results of starred problems are used freely in the text. Unstarred problems are listed when later chapters cite them or when they are standard results. "M:" refers to a milestone above.

### Chapter 1

| Item | Location | Status | Declarations | Comment |
|---|---|---|---|---|
| Rings, domains, quotient field; injective homomorphisms extend to the quotient field | §1.1, p. 1 | full | `CommRing`, `IsDomain`, `FractionRing`, `IsFractionRing.lift` (Mathlib/RingTheory/Localization/FractionRing.lean) | |
| Polynomial rings, monomials, forms, degree, universal property | §1.1, pp. 1–2 | full | `MvPolynomial`, `MvPolynomial.aeval`, `MvPolynomial.IsHomogeneous`, `MvPolynomial.homogeneousComponent`, `MvPolynomial.totalDegree` | Degree of $0$ differs (Conventions). |
| Gauss's lemma; $R$ UFD ⇒ $R[X]$ UFD; $k[X_1,\dots,X_n]$ UFD | §1.1, p. 2 | full | `Polynomial.IsPrimitive.irreducible_iff_irreducible_map_fraction_map` (Mathlib/RingTheory/Polynomial/GaussLemma.lean), `MvPolynomial.uniqueFactorizationMonoid` (Mathlib/RingTheory/Polynomial/UniqueFactorization.lean) | |
| Ideals: prime, maximal, principal, PID, residue rings | §1.1, p. 2 | full | `Ideal.IsPrime`, `Ideal.IsMaximal`, `IsPrincipalIdealRing`, `Ideal.Quotient.mk`, `Ideal.Quotient.lift` | Fulton's prime ideals include $R$ (Conventions). |
| Characteristic; algebraically closed fields; roots with multiplicity | §1.1, pp. 2–3 | full | `ringChar`, `CharP`, `IsAlgClosed`, `Polynomial.roots` | |
| Partial derivatives, rules (1)–(5) | §1.1, p. 3 | full | `MvPolynomial.pderiv`, `MvPolynomial.pderiv_mul`, `MvPolynomial.pderiv_C`, `MvPolynomial.pderiv_comm` (Mathlib/Algebra/MvPolynomial/PDeriv.lean); chain rule (4): Tau Ceti `MvPolynomial.pderiv_aeval` (TauCeti/Algebra/MvPolynomial/PDeriv.lean) | |
| Euler's theorem (6) | §1.1, p. 3 | full | `MvPolynomial.IsHomogeneous.sum_X_mul_pderiv` (Mathlib/RingTheory/MvPolynomial/EulerIdentity.lean) | Any characteristic. |
| Problem 1.1★ (products and factors of forms) | §1.1, p. 3 | full | `MvPolynomial.IsHomogeneous.mul`; Tau Ceti `MvPolynomial.IsHomogeneous.of_mul_left`, `MvPolynomial.IsHomogeneous.of_mul_right` (TauCeti/RingTheory/MvPolynomial/Homogeneous.lean) | (b) needs the form nonzero (every polynomial divides $0$). |
| Problem 1.2★ (reduced fractions in a UFD) | §1.1, p. 3 | partial | `IsFractionRing.exists_reduced_fraction` (Mathlib/RingTheory/Localization/NumDen.lean) | Existence; uniqueness up to units not packaged. |
| Problem 1.3★ (nonzero primes of a PID) | §1.1, p. 3 | full | `IsPrime.to_maximal_ideal` (Mathlib/RingTheory/PrincipalIdealDomain.lean), `Submodule.IsPrincipal.generator` | |
| Problem 1.4★ (polynomials vanishing on $k^n$, $k$ infinite) | §1.1, p. 3 | full | `MvPolynomial.funext` (Mathlib/Algebra/MvPolynomial/Funext.lean); Tau Ceti `MvPolynomial.vanishingIdeal_pi_eq_bot` (TauCeti/RingTheory/MvPolynomial/ZariskiClosure.lean) | |
| Problem 1.5★ (infinitely many monic irreducibles) | §1.1, p. 3 | — | — | Used only for Problem 1.44. |
| Problem 1.6★ (algebraically closed fields are infinite) | §1.1, p. 3 | full | instance `Infinite K` for `[IsAlgClosed K]` (Mathlib/FieldTheory/IsAlgClosed/Basic.lean) | |
| Problem 1.7★ (Taylor expansion at a point; $F(a) = 0$ ⇒ $F\in(X_i-a_i)$) | §1.1, p. 3 | partial | Tau Ceti `MvPolynomial.taylor` (TauCeti/RingTheory/MvPolynomial/OrderAt.lean), `MvPolynomial.ker_constantCoeff` (TauCeti/RingTheory/MvPolynomial/Ideal.lean) | (b) follows by Taylor shift; not stated. |
| Affine space, zeros, hypersurfaces, algebraic sets $V(S)$; properties (1)–(5) | §1.2, pp. 4–5 | partial | `MvPolynomial.zeroLocus`, `MvPolynomial.zeroLocus_span`, `MvPolynomial.zeroLocus_anti_mono`, `MvPolynomial.zeroLocus_bot`, `MvPolynomial.zeroLocus_top`; Tau Ceti `MvPolynomial.zariskiClosure_isClosed_iff` | Unions and intersections of zero loci not stated. |
| Problem 1.8★ (algebraic subsets of $\mathbb{A}^1$) | §1.2, p. 5 | related | `Polynomial.card_roots'` | |
| Problem 1.14★ ($\mathbb{A}^n\setminus V(F)$ and $V(F)$ infinite) | §1.2, p. 5 | — | — | Used in Corollary 3 of §1.6. |
| Problem 1.15★ (products of algebraic sets) | §1.2, p. 5 | — | — | |
| $I(X)$; properties (6)–(9) | §1.3, pp. 5–6 | full | `MvPolynomial.vanishingIdeal`, `MvPolynomial.vanishingIdeal_anti_mono`, `MvPolynomial.le_vanishingIdeal_zeroLocus`, `MvPolynomial.zeroLocus_vanishingIdeal_le`, `MvPolynomial.zeroLocus_vanishingIdeal_galoisConnection`; Tau Ceti `MvPolynomial.vanishingIdeal_zariskiClosure` | (9) is the Galois-connection identity. |
| Property (7): $I(\mathbb{A}^n) = 0$ for infinite $k$; ideal of a point | §1.3, p. 6 | partial | Tau Ceti `MvPolynomial.vanishingIdeal_pi_eq_bot`; Mathlib `MvPolynomial.mem_vanishingIdeal_singleton_iff` | Generators $X_i-a_i$ not stated. |
| Radical; property (10) | §1.3, p. 6 | partial | `Ideal.radical`, `Ideal.IsRadical` | That $I(X)$ is radical is not a named lemma. |
| Problem 1.16★ ($V = W$ iff $I(V) = I(W)$) | §1.3, p. 6 | partial | `MvPolynomial.zeroLocus_vanishingIdeal_galoisConnection` | |
| Problem 1.17★ (separating and interpolating polynomials) | §1.3, p. 6 | — | — | Used in Corollary 4 of §1.7, Proposition 6 of §2.9, §3.3, Chapters 5–6. Easy; prove early. |
| Problem 1.18★ (radicals) | §1.3, p. 6 | full | `Ideal.radical`, `Ideal.radical_isRadical`, `Ideal.IsPrime.isRadical` (Mathlib/RingTheory/Ideal/Operations.lean) | |
| Problem 1.20★ ($V(I) = V(\operatorname{Rad} I)$, $\operatorname{Rad} I\subset I(V(I))$) | §1.3, p. 6 | partial | `MvPolynomial.radical_le_vanishingIdeal_zeroLocus` | Second half only. |
| Problem 1.21★ ($(X_i-a_i)$ maximal, residue field $k$) | §1.3, p. 6 | partial | maximality instance for `MvPolynomial.vanishingIdeal K {x}` (Mathlib/RingTheory/Nullstellensatz.lean); Tau Ceti instance `MvPolynomial.isMaximal_idealOfVars` | Generators and $k\cong$ quotient not stated. |
| Theorem 1 (algebraic sets are finite intersections of hypersurfaces) | §1.4, p. 6 | partial | `MvPolynomial.isNoetherianRing`, `MvPolynomial.zeroLocus_span` | $\mathbb{A}^n$ is the empty intersection. |
| Hilbert basis theorem; Corollary ($k[X_1,\dots,X_n]$ Noetherian) | §1.4, p. 7 | full | `Polynomial.isNoetherianRing`, `MvPolynomial.isNoetherianRing` (Mathlib/RingTheory/Polynomial/Basic.lean) | |
| Problem 1.22★ (ideals of $R/I$; quotients of Noetherian rings) | §1.4, p. 7 | full | `Ideal.relIsoOfSurjective` (Mathlib/RingTheory/Ideal/Maps.lean), `Ideal.Quotient.isNoetherianRing` | |
| Reducible and irreducible algebraic sets; Proposition 1 ($V$ irreducible iff $I(V)$ prime) | §1.5, p. 7 | partial | `PrimeSpectrum.isIrreducible_iff_vanishingIdeal_isPrime` (Mathlib/RingTheory/Spectrum/Prime/Topology.lean) | On `PrimeSpectrum` only; $\varnothing$ convention differs. |
| Lemma (maximal members in Noetherian rings) | §1.5, p. 8 | full | `set_has_maximal_iff_noetherian` (Mathlib/RingTheory/Noetherian/Defs.lean) | |
| Theorem 2 (irreducible components) | §1.5, p. 8 | partial | `TopologicalSpace.NoetherianSpace.finite_irreducibleComponents` (Mathlib/Topology/NoetherianSpace.lean), `Ideal.finite_minimalPrimes_of_isNoetherianRing` (Mathlib/RingTheory/Ideal/MinimalPrime/Noetherian.lean) | No statement for subsets of $k^n$. |
| Problem 1.24 (proper ideals lie in maximal ideals) | §1.5, p. 8 | full | `Ideal.exists_le_maximal` | Unstarred, but used in the proof of the weak Nullstellensatz. |
| Problem 1.29★ ($\mathbb{A}^n$ irreducible, $k$ infinite) | §1.5, p. 9 | partial | Tau Ceti `MvPolynomial.vanishingIdeal_pi_eq_bot` | $I(\mathbb{A}^n) = 0$, which is prime. |
| Proposition 2 (coprime $F, G$: $V(F,G)$ finite) | §1.6, p. 9 | — | — | M: Coprime plane polynomials have finitely many common zeros. |
| Corollaries 1–3 (irreducible subsets of $\mathbb{A}^2$; $I(V(F))$) | §1.6, p. 9 | partial | see milestone | M: Reduced equations and irreducible components of affine hypersurfaces. |
| Weak Nullstellensatz | §1.7, p. 10 | full | Tau Ceti `MvPolynomial.zeroLocus_eq_empty_iff`, `MvPolynomial.span_eq_top_iff_forall_exists_aeval_ne_zero` (TauCeti/RingTheory/Nullstellensatz.lean); Mathlib `MvPolynomial.eq_vanishingIdeal_singleton_of_isMaximal` | |
| Hilbert's Nullstellensatz | §1.7, p. 10 | full | `MvPolynomial.vanishingIdeal_zeroLocus_eq_radical` (Mathlib/RingTheory/Nullstellensatz.lean) | Coefficients in any $k$, points in an algebraically closed extension. |
| Corollary 1 (radical ideals ↔ algebraic sets) | §1.7, p. 11 | partial | `MvPolynomial.vanishingIdeal_zeroLocus_eq_radical`, `MvPolynomial.zeroLocus_vanishingIdeal_galoisConnection` | Bijection not packaged. |
| Corollary 2 (primes ↔ irreducible sets; maximal ideals ↔ points) | §1.7, p. 11 | partial | `MvPolynomial.IsPrime.vanishingIdeal_zeroLocus`, `MvPolynomial.isMaximal_iff_eq_vanishingIdeal_singleton` | Irreducibility of subsets of $k^n$ absent. |
| Corollary 3 (hypersurfaces) | §1.7, p. 11 | partial | see milestone | M: Reduced equations and irreducible components of affine hypersurfaces. |
| Corollary 4 ($V(I)$ finite iff $\dim_k k[X]/I<\infty$) | §1.7, p. 11 | partial | `Module.finite_iff_krullDimLE_zero` | M: Finite zero sets and finite-dimensional quotients of polynomial rings. |
| Problem 1.37★ (basis $1,\dots,\bar X^{n-1}$ of $K[X]/(F)$) | §1.7, p. 12 | full | `AdjoinRoot.powerBasis` (Mathlib/RingTheory/AdjoinRoot.lean) | |
| Problem 1.38★ (algebraic subsets of $V(I)$ ↔ radical ideals of $k[X]/I$) | §1.7, p. 12 | partial | `Ideal.relIsoOfSurjective`, `MvPolynomial.vanishingIdeal_zeroLocus_eq_radical` | |
| Modules, submodules, finite generation | §1.8, pp. 12–13 | full | `Module`, `Submodule.span`, `Module.Finite` | |
| Module-finite, ring-finite, finitely generated field extensions | §1.8, p. 13 | full | `Module.Finite`, `Algebra.FiniteType`, `Algebra.adjoin`, `IntermediateField.adjoin`, `IntermediateField.FG` | |
| Problem 1.41★ (module-finite ⇒ ring-finite) | §1.8, p. 13 | full | `Module.Finite.finiteType` (Mathlib/RingTheory/FiniteType.lean) | |
| Problem 1.43★ (ring-finite field ⇒ finitely generated extension) | §1.8, p. 13 | related | `Algebra.FiniteType`, `IntermediateField.FG` | One line. |
| Problem 1.44★ ($K(X)$ not ring-finite) | §1.8, p. 13 | related | `finite_of_finite_type_of_isJacobsonRing` | Follows from Zariski's lemma; not stated. |
| Problem 1.45★ (transitivity of finiteness) | §1.8, p. 13 | full | `Module.Finite.trans`, `Algebra.FiniteType.trans` | (c) for fields: one line. |
| Integral elements; Proposition 3 | §1.9, p. 14 | full | `IsIntegral`, `IsIntegral.fg_adjoin_singleton`, `IsIntegral.of_mem_of_fg`, `IsIntegral.of_finite` | Mathlib: any commutative rings. |
| Corollary (integral elements form a subring) | §1.9, p. 14 | full | `integralClosure` (Mathlib/RingTheory/IntegralClosure/Algebra/Basic.lean) | |
| Problem 1.46★ (transitivity of integrality) | §1.9, p. 14 | full | `isIntegral_trans`, `Algebra.IsIntegral.trans` | |
| Problem 1.47★ (ring-finite: module-finite iff integral) | §1.9, p. 14 | full | `Algebra.finite_iff_isIntegral_and_finiteType` | |
| Problem 1.48★ (algebraically closed subfields) | §1.9, p. 14 | full | `IsAlgClosed.algebraMap_bijective_of_isIntegral`, `IntermediateField.eq_bot_of_isAlgClosed_of_isAlgebraic` | |
| Problem 1.49★ ($K[X]$ integrally closed; no universal denominator) | §1.9, pp. 14–15 | partial | `UniqueFactorizationMonoid.instIsIntegrallyClosed` | (a) only. |
| Problem 1.50★ (algebraic elements form a field; intermediate rings of finite extensions are fields) | §1.9, p. 15 | full | `algebraicClosure` (Mathlib/FieldTheory/AlgebraicClosure.lean), `Subalgebra.isField_of_algebraic` | |
| Simple extensions $K(v)$ (Cases 1–2) | §1.10, p. 15 | full | `minpoly`, `AdjoinRoot` | |
| Proposition 4 (Zariski's lemma) | §1.10, p. 15 | full | `finite_of_finite_type_of_isJacobsonRing` (Mathlib/RingTheory/Jacobson/Ring.lean) | Fields are Jacobson. |
| Problem 1.51★ ($K[X]/(F)$ for irreducible $F$) | §1.10, pp. 15–16 | full | `AdjoinRoot.instField`, `AdjoinRoot.liftAlgHom`, `minpoly.dvd`, `Polynomial.mul_divByMonic_eq_iff_isRoot` | |
| Problem 1.52★ (splitting fields) | §1.10, p. 16 | full | `Polynomial.SplittingField` | |
| Problem 1.53★ (irreducible polynomials separable in characteristic 0) | §1.10, p. 16 | full | `Irreducible.separable` (Mathlib/FieldTheory/Separable.lean), `Polynomial.nodup_roots` | Perfect fields suffice. |
| Problem 1.54★ (integral multiples; integral bases) | §1.10, p. 16 | partial | `IsAlgebraic.exists_integral_multiple` (Mathlib/RingTheory/Algebraic/Integral.lean) | (a) only. |

### Chapter 2

| Item | Location | Status | Declarations | Comment |
|---|---|---|---|---|
| Standing assumption: $k$ algebraically closed, $k$-algebras | Ch. 2, p. 17 | related | `IsAlgClosed`, `Algebra`, `AlgHom` | See Conventions. |
| Affine variety; $\Gamma(V)$; polynomial functions | §2.1, p. 17 | partial | `MvPolynomial (Fin n) k ⧸ MvPolynomial.vanishingIdeal k V` | No classical variety type. |
| Problem 2.1★ ($k[X]\to$ functions on $V$, kernel $I(V)$) | §2.1, p. 17 | partial | `Ideal.Quotient.mk`, `MvPolynomial.eval` | By definition of `vanishingIdeal`. |
| Problem 2.2★ (subsets/subvarieties/points of $V$ ↔ radical/prime/maximal ideals of $\Gamma(V)$) | §2.1, p. 18 | partial | `Ideal.relIsoOfSurjective`, `MvPolynomial.vanishingIdeal_zeroLocus_eq_radical`, `MvPolynomial.isMaximal_iff_eq_vanishingIdeal_singleton` | |
| Problem 2.3★ ($\Gamma(W)\cong\Gamma(V)/I_V(W)$) | §2.1, p. 18 | partial | `DoubleQuot.quotQuotEquivQuotOfLE` (Mathlib/RingTheory/Ideal/Quotient/Operations.lean) | |
| Problem 2.4★ ($V$ a point iff $\Gamma(V) = k$ iff $\dim_k\Gamma(V)<\infty$) | §2.1, p. 18 | — | — | From Corollary 4 of §1.7; used in §6.5. |
| Problem 2.5 ($F$ monic in $Y$: $\Gamma(V)$ module-finite over $k[X]$) | §2.1, p. 18 | partial | `AdjoinRoot.powerBasis'`; Tau Ceti `Polynomial.Bivariate.monic_equivMvPolynomial_symm` (TauCeti/Algebra/MvPolynomial/Fin/Two.lean) | |
| Polynomial maps; Proposition 1 (polynomial maps ↔ $k$-algebra maps $\Gamma(W)\to\Gamma(V)$); isomorphisms | §2.2, p. 18 | partial | `MvPolynomial.aeval`, `MvPolynomial.algHom_ext`, `Ideal.Quotient.liftₐ` | The algebra side only; used in §6.3. |
| Problems 2.6★, 2.9★, 2.10★ (composition; restriction; projections) | §2.2, p. 19 | related | `AlgHom.comp`, `MvPolynomial.aeval` | |
| Problem 2.7★ (preimages of algebraic sets; irreducibility test) | §2.2, p. 19 | related | Tau Ceti `MvPolynomial.image_zariskiClosure_subset_zariskiClosure_image` (TauCeti/RingTheory/MvPolynomial/ZariskiClosure.lean) | Continuity form only. |
| $F^T$, $V^T$; affine changes of coordinates | §2.3, p. 19 | partial | `AffineEquiv`, `MvPolynomial.aeval`; Tau Ceti `MvPolynomial.zariskiClosure_image_linearEquiv` | |
| Problem 2.14★ (linear subvarieties; dimension) | §2.3, p. 20 | partial | `AffineSubspace`, `AffineEquiv` | As affine subspaces of $k^n$; the zero-locus form is absent. |
| Problem 2.15★ (lines; moving a point and two lines) | §2.3, p. 20 | — | — | (d) is used in Theorem 1 of §3.2. |
| $k(V)$; "defined at $P$"; pole set | §2.4, p. 20 | partial | `FractionRing`, `Localization.subalgebra.ofField` | |
| $\mathcal{O}_P(V)$, value $f(P)$, $\mathfrak{m}_P(V)$; Propositions 2 and 3 | §2.4, p. 21 | partial | see milestone | M: Local ring of an affine variety at a point. |
| Lemma (local rings: non-units form an ideal) | §2.4, p. 21 | full | `IsLocalRing` (Mathlib/RingTheory/LocalRing/Defs.lean), `IsLocalRing.of_nonunits_add` (Mathlib/RingTheory/LocalRing/Basic.lean) | |
| Problem 2.18 (primes of $\mathcal{O}_P(V)$) | §2.4, p. 22 | partial | `IsLocalization.AtPrime.orderIsoOfPrime` | |
| Problems 2.21★, 2.22★ (induced maps of local rings; coordinate changes) | §2.4, p. 22 | partial | `Localization.localRingHom` | In the local-ring milestone. |
| Proposition 4; DVR, uniformizing parameter, $\operatorname{ord}$ | §2.5, pp. 22–23 | full | see milestone | M: Discrete valuation rings via principal maximal ideals. |
| Problems 2.23★, 2.26★, 2.28★, 2.29★ | §2.5, p. 23 | full | see milestone | |
| Problem 2.24★ ($\mathcal{O}_a(\mathbb{A}^1)$ and $\mathcal{O}_\infty$ are DVRs) | §2.5, p. 23 | partial | `IsLocalization.AtPrime.isDiscreteValuationRing_of_dedekind_domain` (Mathlib/RingTheory/DedekindDomain/Dvr.lean); `RatFunc.inftyValuation` (Mathlib/FieldTheory/RatFunc/Valuation.lean) | (b) related. |
| Problem 2.30★ (residues and truncated expansions in a DVR) | §2.5, p. 23 | partial | Tau Ceti `TauCeti.Place.existsUnique_sub_sum_mem_filtration` (TauCeti/FieldTheory/FunctionField/Place/Expansion/Basic.lean) | Rational places of function fields only. M: Order equals colength in a discrete valuation ring… |
| Problem 2.31 ($k[[X]]$ is a DVR) | §2.5, p. 23 | full | instance `IsDiscreteValuationRing k⟦X⟧` (Mathlib/RingTheory/PowerSeries/Inverse.lean) | |
| Problem 2.32 (power series expansion $R\hookrightarrow k[[X]]$) | §2.5, p. 24 | partial | Tau Ceti `TauCeti.Place.powerSeriesExpansion` (TauCeti/FieldTheory/FunctionField/Place/Expansion/PowerSeries.lean) | Rational places of function fields only; used in Chapter 8. |
| $F_*$, $f^*$; Proposition 5 | §2.6, p. 24 | partial | see milestone | M: Homogenization and dehomogenization of polynomials. |
| Corollary (factoring forms; binary forms split) | §2.6, p. 24 | partial | see milestones | M: Homogenization…; M: Binary forms… |
| Problem 2.34 ($F+G$ irreducible for coprime forms of degrees $r$, $r+1$) | §2.6, p. 24 | — | — | Used in Chapter 3 problems. |
| Problems 2.35★, 2.36 (dimension of $V(d,n)$; basis $A_{ij}$) | §2.6, p. 25 | partial | see milestone | M: Dimensions of spaces of forms… |
| Direct products; universal property | §2.7, p. 25 | full | `Pi.evalRingHom`, `RingHom.pi` (Mathlib/Algebra/Ring/Pi.lean) | |
| Problem 2.38★ ($\dim\prod R_i = \sum\dim R_i$) | §2.7, p. 25 | full | `Module.finrank_pi_fintype` (Mathlib/LinearAlgebra/Dimension/Constructions.lean) | |
| $IJ$, $I^n$, $IS$, $I+J$, comaximal ideals; Lemma ($IJ\subset I\cap J$, equality if comaximal) | §2.8, pp. 25–26 | full | `Ideal.mul_le_inf`, `Ideal.mul_eq_inf_of_isCoprime`, `Ideal.isCoprime_iff_sup_eq` (Mathlib/RingTheory/Ideal/Operations.lean), `Ideal.map` (Mathlib/RingTheory/Ideal/Maps.lean) | |
| Example ($I^n$ for $I = (X_1,\dots,X_r)$) | §2.8, p. 25 | partial | `MvPolynomial.mem_pow_idealOfVars_iff` | M: Dimensions of spaces of forms… |
| Problem 2.39★ (distributivity; powers of products) | §2.8, p. 26 | full | `Ideal.sup_mul`, `mul_pow` | |
| Problem 2.40★ (powers of comaximal ideals) | §2.8, p. 26 | full | `IsCoprime.pow` (Mathlib/RingTheory/Coprime/Lemmas.lean), `Ideal.prod_eq_iInf_of_pairwise_isCoprime` | |
| Problem 2.41★ ($I$ f.g., $I\subset\operatorname{Rad}J$ ⇒ $I^n\subset J$) | §2.8, p. 26 | full | `Ideal.exists_pow_le_of_le_radical_of_fg` (Mathlib/RingTheory/Finiteness/Ideal.lean) | |
| Problem 2.42★ (natural maps $R/I\to R/J$, $R/I\to S/IS$) | §2.8, p. 26 | full | `Ideal.Quotient.factor`, `Ideal.quotientMap` | |
| Problems 2.43★, 2.44★ | §2.8, p. 26 | partial | see milestone | M: Local rings of a subvariety as quotients… |
| Problem 2.45★ (comaximal iff disjoint zero sets) | §2.8, p. 26 | partial | Tau Ceti `MvPolynomial.zeroLocus_eq_empty_iff` | Apply to $I + J$. |
| Problem 2.46★ ($\dim k[X,Y]/(X,Y)^n = n(n+1)/2$) | §2.8, p. 26 | partial | see milestone | M: Dimensions of spaces of forms… |
| Proposition 6; Corollaries 1, 2 | §2.9, p. 27 | partial | see milestone | M: Global-to-local decomposition… |
| Problem 2.47 (finite-dimensional $k$-algebras are products of local rings) | §2.9, p. 27 | full | `PrimeSpectrum.toPiLocalizationEquiv`, `IsArtinianRing.of_finite` | |
| Module homomorphisms, quotient modules, exact sequences | §2.10, pp. 27–28 | full | `LinearMap`, `Submodule.mkQ`, `Function.Exact` | |
| Proposition 7 | §2.10, p. 28 | full | see milestone | M: Additivity of dimension and length along exact sequences. |
| Problem 2.48★ ($0\to\ker\to M\to\operatorname{im}\to0$) | §2.10, p. 28 | full | `LinearMap.ker`, `LinearMap.range`, `Function.Exact` | |
| Problem 2.49★ (quotient maps; standard exact sequences) | §2.10, pp. 28–29 | partial | `Submodule.liftQ`, `Submodule.quotientQuotientEquivQuotient`, `Submodule.finrank_quotient_add_finrank` | (d), (e) not packaged; in the exact-sequence milestone. |
| Problem 2.50★ ($\operatorname{ord}(z) = \dim_k R/(z)$) | §2.10, p. 29 | partial | see milestone | M: Order equals colength in a discrete valuation ring… |
| Problem 2.51 (alternating sum of dimensions) | §2.10, p. 29 | full | `Module.sum_neg_one_pow_finrank_eq_zero_of_exact` (Mathlib/Algebra/Exact/Sequence.lean); Tau Ceti `TauCeti.finsum_neg_one_pow_finrank_eq_zero_of_exact` (TauCeti/LinearAlgebra/Exact.lean) | |
| Problem 2.52★ (first isomorphism theorem $N/N\cap P\cong(N+P)/P$) | §2.10, p. 29 | full | `LinearMap.quotientInfEquivSupQuotient` (Mathlib/LinearAlgebra/Isomorphisms.lean) | |
| Problem 2.53★ ($\dim V/T(V) = \dim W/T(W)$ for injective $T$ with $T(W)\subset W$) | §2.10, p. 29 | full | Tau Ceti `TauCeti.length_quotient_range_restrict` (TauCeti/RingTheory/Length/Endomorphism.lean) | Length form, needs only $V/W$ of finite length; used in Proposition 2 of §7.5. |
| Free modules; bases | §2.11, pp. 29–30 | full | `Finsupp`, `Finsupp.linearCombination`, `Module.Basis`, `Module.Free` | |
| Problem 2.55 ($R[X]/(F)$ free for monic $F$) | §2.11, p. 30 | full | `AdjoinRoot.powerBasis'` (Mathlib/RingTheory/AdjoinRoot.lean) | |

## Notes on the source

- **Numbering in the 2008 edition.** Theorems and propositions are numbered consecutively through each *chapter*, not each section: Chapter 1 has Theorems 1 (§1.4) and 2 (§1.5) and Propositions 1 (§1.5), 2 (§1.6), 3 (§1.9), 4 (§1.10); Chapter 2 has Propositions 1–7 in §§2.2, 2.4 (two), 2.5, 2.6, 2.9, 2.10, and no theorems. Corollaries are numbered per result (Corollaries 1–3 of Proposition 2, §1.6; Corollaries 1–4 of the Nullstellensatz, §1.7; Corollaries 1–2 of Proposition 6, §2.9), and several unnumbered "Corollary" and "Lemma" items occur (§§1.4, 1.5, 1.9, 2.4, 2.6, 2.8). The Hilbert basis theorem, the weak Nullstellensatz and Hilbert's Nullstellensatz are named, not numbered. Problems are numbered $n.m$ through the chapter (1.1–1.54, 2.1–2.56). Fulton's own cross-references read "Proposition 7 of §2.10", "Corollary 1 of §2.9", which is the form used on this page.
- **Starred problems used in the text.** Several proofs in these chapters rest on starred problems: Problem 1.17 (Corollary 4 of §1.7, Proposition 6 of §2.9), Problem 1.22 (Proposition 3 of §2.4), Problems 1.21 and 1.48 (the weak Nullstellensatz), Problems 1.45 and 1.49 (Proposition 4 of §1.10), Problems 2.40, 2.41, 2.42, 2.45 (Proposition 6). The unstarred Problem 1.24 (every proper ideal of a Noetherian ring lies in a maximal ideal) is used in the proof of the weak Nullstellensatz. Chapter 3 uses Problems 2.15(d), 2.22, 2.35(c), 2.42, 2.43, 2.44, 2.46, 2.49(e), 2.50.
- **Where $k$ becomes algebraically closed.** The preface says "from Chapter 3 on"; the text fixes it at the start of Chapter 2 (p. 17). §1.7 also assumes it.
- **Prime ideals and the empty set.** Fulton's definition of a prime ideal (p. 2) does not exclude the unit ideal, and with his definition of irreducibility $\varnothing$ is irreducible (listed in Corollary 2 of §1.6). The statement on p. 2 that a principal ideal $(a)$ in a UFD is prime iff $a$ is irreducible or zero is, under this definition, false for units $a$. Harmless, but the library should follow Mathlib.
- **Hilbert basis theorem proof (p. 7).** The leading-coefficient argument writes $F = a_1 + a_1X + \dots + a_dX^d$ (so on the printed page, not only in the text extraction), a typo for $a_0 + a_1X + \cdots$. The ideals $J_m$ ("leading coefficients of polynomials in $I$ of degree $\le m$") are ideals because multiplying by a power of $X$ does not change the leading coefficient; the proof is correct as written. Mathlib has the theorem.
- **Theorem 1 of §1.4** says every algebraic set is a finite intersection of hypersurfaces; $\mathbb{A}^n$ is the empty intersection, and $\varnothing = V(X_1)\cap V(X_1-1)$ since hypersurfaces need nonconstant equations.
- **Proposition 2 of §1.6** is stated and proved over an arbitrary field, but its proof gives more than finiteness of $V(F,G)$: a nonzero element of $(F,G)\cap k[X]$ and one of $(F,G)\cap k[Y]$, whence finite-dimensionality of $k[X,Y]/(F,G)$ without the Nullstellensatz (see the milestone). The book instead gets finite-dimensionality in §3.3 through Corollary 4 of §1.7 and Corollary 1 of §2.9.
- **Corollary 4 of §1.7**: the inequality $\#V(I)\le\dim_k k[X]/I$ holds over any field; only the converse needs algebraic closure. The proof cites Problem 1.17 for polynomials $F_i$ with $F_i(P_j) = \delta_{ij}$, which is Problem 1.17(b) with $V = \varnothing$.
- **Proposition 5 of §2.6** is left to the reader. Part (3) needs $f$, $g$ and $f+g$ nonzero for the degrees to make sense; with Fulton's convention that $0$ has any degree the identity is still true but vacuous. The Corollary's second claim needs $F\ne0$.
- **Proposition 4 of §2.5**: the direction (2)⇒(1) is marked "we don't really need this part", but Problem 2.28★ (DVRs from order functions) and the later chapters do need it.
- **Problem 2.53★** assumes both $V/W$ and $W/T(W)$ finite-dimensional; only $V/W$ is needed (Tau Ceti's version). Its use in Proposition 2 of §7.5 applies it to multiplication by $g$ on $\Gamma(U)\subset\Gamma(U')$, with $\Gamma(U')/\Gamma(U)$ finite-dimensional.
- **Problem 2.12** (p. 19) begins "(a)]" on the printed page, a typographic slip.
- **Problem 1.53★** assumes characteristic zero; perfectness of $K$ suffices. It is the only characteristic hypothesis in Chapters 1–2.
- The proof of Corollary 2 of §1.6 ("if $V$ is finite … it is of the required type") uses that a finite irreducible set is a point or empty.

## Deferred

No numbered result of Chapters 1–2 is deferred; those not made milestones are in the libraries (see the table) or are routine. Left out of the milestone set, as exercises or examples rather than theory the library needs: Problems 1.27 and 1.28 (components of a subset lie in components; no component is covered by the others), routine consequences of Theorem 2 of §1.5; Problems 1.9–1.13, 1.19, 1.23, 1.25, 1.26, 1.30–1.36, 1.39, 1.40, 1.42 (examples over $\mathbb{R}$, $\mathbb{C}$ and finite fields; Problem 1.40 and Problem 2.13, the curve $V(X^2-Y^3, Y^2-Z^3)$, recur as examples in later chapters and could serve as tests); Problems 2.8, 2.11–2.13, 2.17, 2.19, 2.20, 2.33, 2.37, 2.54, 2.56; Problem 2.16 (path-connectedness of $\mathbb{C}^n\setminus V$ in the classical topology), which is analytic and out of scope; Problems 2.25 and 2.27 (the DVRs with fraction field $\mathbb{Q}$, and those of $k(X)$ containing $k$), whose content belongs with the function-field material of the later chapters, where Tau Ceti's places of function fields apply.
