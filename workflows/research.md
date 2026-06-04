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

For long or complex documents (papers over 30 pages, technical specs, requirements documents, dense academic monographs), reading happens in three passes:

**Pass 1: skeleton.** Extract the table of contents, abstract or executive summary, all section headings, every figure and table caption. Produce an outline of the argument before reading any body text. This pass is fast and tells you where to focus.

**Pass 2: targeted deep read.** Read the sections that carry the argument. Quote verbatim from each section to confirm the body content is in context. Skim sections that are background or boilerplate.

**Pass 3: detail capture.** Walk through methodology, sample size, controls, edge cases. This is the pass that catches what a careless reader misses.

Why three passes: a single-pass read on a long document burns context and produces shallow notes. Three passes produce an outline, then a structured argument, then defensible detail. Each pass writes to the wiki entry in progress.

Why: skim-summaries miss the load-bearing details. The wiki only stays useful if entries reflect what the source actually says.

## 3. Wiki entry drafted

Every entry is built in three layers so the source can be consumed at whatever depth the next task requires:

**Layer A: one-line takeaway.** The single most important claim, in one sentence. This is what feeds article-series hooks, deck cover slides, and inline citations.

**Layer B: key claims.** Five to ten bullet-sized claims with locators (page, section, figure). This is what feeds report bodies and synthesis pages.

**Layer C: full notes.** Methodology, sample size, controls, caveats, quotes worth keeping. This is what verification draws on when a downstream draft is challenged.

The three layers map to the wiki schema in `wiki/schema.md`. Drafts pull from the layer they need; they do not re-read the source.

Before writing the entry, check whether the source has already been ingested. Duplicate entries fragment the knowledge graph. `grep -l "<author> <year>" wiki/*.md` is the cheap check.

The entry is the contract with future-you about what this source said. Treat it accordingly.

## 4. Cross-links

Every claim of significance gets a `[[wikilink]]` to its supporting source. If the new entry contradicts an existing one, the existing entry's caveats get updated.

Why: cross-linking is what turns the wiki into a graph. A pile of notes is not infrastructure. A graph is.

## 5. Index updated

The relevant `INDEX_<topic>.md` gets a new one-line pointer with the one-line takeaway. The index is what makes the entry findable.

## 6. Memory updated (if needed)

If the new source changes a standing fact pack, the memory entry gets updated with a pointer to the new source. If the source extends a contested-author register (reliable on one topic, weak on another), that goes in memory too.

Most ingestions do not require a memory update. The wiki is the right home for source-level detail.

## 7. Source filed privately

The original PDF stays in a private `papers/` folder. It does not go in the public repo and it does not go on GitHub. The wiki entry is the public-facing artifact.

## Cross-referencing as a one-grep operation

The point of the schema is that "every paper on a given topic" becomes:

```
grep -l "topics:.*genetics" wiki/*.md
```

"Every author who is contested on a specific claim" becomes a frontmatter filter. "Every paper that supports a given claim" becomes a backlink search. The wiki turns reading into a query-able substrate.

## Pattern identification

When 10 to 20 papers in a topic area converge on a finding, a synthesis page gets written that distills the canonical fact. The synthesis page links back to every supporting entry. Future drafts pull from the synthesis page rather than re-deriving the pattern.

This is how a library of papers becomes a small set of high-confidence claims that any draft can rely on.

## See also

- `wiki/schema.md` for the entry format
- `verification.md` for the fact-check layer
- `multi-surface-output.md` for how a single ingestion feeds many outputs
