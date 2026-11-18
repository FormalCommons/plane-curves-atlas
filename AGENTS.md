# Instructions for agents

This repository is an atlas of plane algebraic curves: a map, in prose, of
the theory that a Lean library is to cover. It follows Fulton's *Algebraic
Curves* (2008 edition). It states the **milestones**, the results the library
must prove; gives their **sources**; records what matters for formalizing
them, including advice on how to build the library; and says what Mathlib
and Tau Ceti already have. No Lean code lives here.

## Layout

- `atlas/` holds the theory of plane curves: one page for each of Chapters
  3, 5, 7 and 8 of the book (Appendix A is treated with the chapters it
  concerns). `atlas/README.md` is the front page, with the scope, the
  conventions, the milestone counts and the advice on building the library.
- `auxiliary/` holds prerequisites that stand on their own: one page for
  Chapters 1–2 (affine varieties), one for Chapter 4 (projective varieties)
  and one for Chapter 6 (varieties and rational maps). Atlas pages may build
  on auxiliary pages, never the other way round: an auxiliary page never
  links into `atlas/` or relies on a result that only an atlas page states.
  Milestones in `auxiliary/` count like the others.
- `atlas/references.bib` holds every source.

## Pages

Every page has the same parts, in this order:

1. A `# Title`, then an introduction: the chapter and sections covered, with
   pages, and a list of the page's parts, including the number of milestones
   by section. Keep that number, and the table on the front page, equal to
   the number of milestone callouts.
2. The library status block, exactly in this form:

   ```markdown
   > [!important] Library status: checked 2026-11-18
   > All library information on this page was checked on 2026-11-18 against Mathlib at commit `048fffa` (the commit Tau Ceti pins) and Tau Ceti `main` at commit `cd41bff`. Every listed declaration was located in the source; nothing was compiled. "—" means that searches of both libraries, by declaration name and by docstring keywords, found nothing: evidence of absence, not proof.
   ```

   followed by the status words: **full** (the book's statement, in its
   generality or more general), **partial** (a special case, one direction,
   or a nearby formulation, explained), **related** (supporting API only),
   **—** (nothing found). When you re-check against newer commits, update
   the date and both commits.
3. Conventions: the book's, the libraries', and the traps between them.
4. Milestones, grouped under `### §x.y Title` headings that follow the
   book's sections.
5. Correspondence with Mathlib and Tau Ceti: a table with a row for every
   definition, numbered result and starred problem of the chapter: item,
   location, status, declarations (exact names and file paths), comment.
6. Notes on the source: errors and gaps in the book, and what was checked.
7. Deferred (or "Not made milestones"): what is left out, and why.

## Milestones

A milestone is a callout whose first line is its name, unique across the
atlas:

```markdown
> [!milestone] Bézout's theorem for projective plane curves
> Statement, with every hypothesis.
>
> Sources: [@Fulton2008, §5.3, Bézout's Theorem, pp. 57–58].
>
> Proof routes: …
>
> Formalization notes: …
>
> Lean status: Mathlib —. Tau Ceti — …
```

Its statement is precise and complete: the field and its characteristic,
reducedness or irreducibility, degrees, and the degenerate cases. Its Lean
status uses the status words above, for each library. Milestones that a
library already has in full are listed on the front page, so that they are
not counted as work.

## Rules

1. **Never invent a source or a location.** Every entry in
   `references.bib` is checked against an authoritative record (the
   publisher or DOI, arXiv, zbMATH Open, MathSciNet, or the author's own
   copy). A location (section, theorem, page) is given only if it has been
   seen in the source.
2. **Library status is a dated, checked fact.** Name declarations exactly,
   with their files, and say against which commits of Mathlib and Tau Ceti
   they were checked. Say that something is missing only after searching
   both libraries for it, by name and by keywords.
3. **Write in your own words.** Quote the book only for a few words at a
   time.
4. **Say what you doubt.** A statement you are unsure of, or an error in
   the book, goes in the page's notes on the source, not into confident
   prose.
