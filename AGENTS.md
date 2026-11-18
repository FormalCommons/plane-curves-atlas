# Instructions for agents

This repository is an atlas of plane algebraic curves: a map, in prose, of
the theory that a Lean library is to cover. It follows Fulton's *Algebraic
Curves* (2008 edition). It states the **milestones**, the results the library
must prove; gives their **sources**; records what matters for formalizing
them, including advice on how to build the library; and says what Mathlib
and Tau Ceti already have. No Lean code lives here.

## Layout

- `atlas/` holds the theory of plane curves, one page per chapter of the
  book; `atlas/README.md` is the front page, with the scope, the
  conventions and the advice on building the library.
- `auxiliary/` holds prerequisites that stand on their own (affine and
  projective varieties, rational maps). Atlas pages may build on auxiliary
  pages, never the other way round: an auxiliary page never links into
  `atlas/` or relies on a result that only an atlas page states. Milestones
  in `auxiliary/` count like the others.
- `atlas/references.bib` holds every source.

## Milestones

A milestone is a callout whose first line is its name, unique across the
atlas:

```markdown
> [!milestone] Bézout's theorem for plane curves
> Statement, with every hypothesis.
>
> Sources: [@Fulton2008, §5.3].
>
> Proof routes: …
>
> Formalization notes: …
>
> Lean status: …
```

Its statement is precise and complete: the field and its characteristic,
reducedness or irreducibility, degrees, and the degenerate cases.

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
EOF