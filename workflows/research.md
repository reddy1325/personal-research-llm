# workflows/research.md

How a paper goes from PDF to durable knowledge.

## The pipeline

```
PDF / book chapter / web source
        |
        v
1. Identity verification
        |
        v
2. Full read (not skim)
        |
        v
3. Wiki entry drafted
        |
        v
4. Cross-links added
        |
        v
5. Index updated
        |
        v
6. Memory updated (if needed)
        |
        v
7. Source filed in private papers/
```

## 1. Identity verification

Before reading, confirm what the source actually is. Title, authors, year, journal, edition. If a PDF was downloaded from an aggregator, verify the metadata against a primary listing (publisher page, DOI, library catalog).

Why: the wrong attribution propagates. A wiki entry built on a misidentified source poisons every downstream draft.

## 2. Full read

The assistant reads the entire source, not just the abstract. For PDFs, this means forcing a verbatim quote of a passage from the body to confirm the actual content was loaded into context, not just metadata.

For books, ingestion is chapter by chapter, one wiki entry per chapter, with a master entry linking them.

Why: skim-summaries miss the load-bearing details. The wiki only stays useful if entries reflect what the source actually says.

## 3. Wiki entry drafted

Following the schema in `wiki/schema.md`:

- Frontmatter (authors, year, topics, source-tier)
- Citation
- One-line takeaway
- Key claims with locators
- Method
- Where this fits (supports / contests / extends links)
- Caveats and reliability
- Quotes worth keeping

The entry is the contract with future-you about what this source said. Treat it accordingly.

## 4. Cross-links

Every claim of significance gets a `[[wikilink]]` to its supporting source. If the new entry contradicts an existing one, the existing entry's caveats get updated.

Why: cross-linking is what turns the wiki into a graph. A pile of notes is not infrastructure. A graph is.

## 5. Index updated

The relevant `INDEX_<topic>.md` gets a new one-line pointer with the one-line takeaway. The index is what makes the entry findable.

## 6. Memory updated (if needed)

If the new source changes a standing fact pack, the memory entry gets updated with a pointer to the new source. If the source extends a contested-author register (great on X, weak on Y), that goes in memory too.

Most ingestions do not require a memory update. The wiki is the right home for source-level detail.

## 7. Source filed privately

The original PDF stays in a private `papers/` folder. It does not go in the public repo and it does not go on GitHub. The wiki entry is the public-facing artifact.

## Cross-referencing as a one-grep operation

The point of the schema is that "every paper on topic X" becomes:

```
grep -l "topics:.*topic-X" wiki/*.md
```

"Every author who is contested on a specific claim" becomes a frontmatter filter. "Every paper that supports a given claim" becomes a backlink search. The wiki turns reading into a query-able substrate.

## Pattern identification

When 10 to 20 papers in a topic area converge on a finding, a synthesis page gets written that distills the canonical fact. The synthesis page links back to every supporting entry. Future drafts pull from the synthesis page rather than re-deriving the pattern.

This is how a library of papers becomes a small set of high-confidence claims that any draft can rely on.

## See also

- `wiki/schema.md` for the entry format
- `verification.md` for the fact-check layer
- `multi-surface-output.md` for how a single ingestion feeds many outputs
