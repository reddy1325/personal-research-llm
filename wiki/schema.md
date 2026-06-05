# wiki/schema.md

The knowledge backend. Every paper, book, and primary source I ingest gets a structured note. The wiki is institutional memory, not a notes folder.

## What goes in the wiki

- Academic papers (one note per paper)
- Books (one note per chapter, plus a master note linking the chapters)
- Primary sources (inscriptions, coins, archaeological reports)
- Synthesis pages that pull from multiple notes

Drafts, working notes, and chat transcripts do not belong in the wiki. They live elsewhere.

## File format

Every wiki entry follows the same shape:

```markdown
---
name: short-kebab-case-slug
description: One-line summary for index display
metadata:
  authors: [Last, F.]
  year: 2024
  type: paper | book-chapter | primary-source | synthesis
  topics: [topic-1, topic-2]
  source-tier: primary | secondary | contested
---

# Title

## Citation
Full bibliographic reference.

## One-line takeaway
The single most important claim, in one sentence.

## Key claims
- Claim 1, with the page or section locator
- Claim 2, with locator
- ...

## Method
How the claim was established. Sample size, technique, what was measured.

## Where this fits
- Supports: [[other-note-1]], [[other-note-2]]
- Contests: [[other-note-3]]
- Extends: [[other-note-4]]

## Caveats and reliability
What the paper does not show. Where the author is known to be unreliable. Any sample-size or method limitations.

## Quotes worth keeping
> Direct quote 1
> Direct quote 2
```

## The index layer

Each topic area has its own `INDEX_<topic>.md` collecting one-line pointers to every wiki entry on that topic. Same trick as memory: the index is small enough to load, the bodies are loaded on demand.

```markdown
# INDEX_<topic>.md

## Primary sources
- [Author 2024, short title](author_2024_short_title.md), One-line takeaway

## Synthesis
- [Topic overview](topic_overview.md), How the pieces fit together
```

## Cross-linking discipline

Every claim of significance gets a `[[wikilink]]` to its supporting source. When a new paper contradicts an existing entry, the new entry's `Contests:` section names the existing one, and the existing entry's `Caveats and reliability` section gets updated.

This is how the wiki stays consistent as it grows. The graph is more valuable than any single node.

## Source-tier register

Some authors are great on some topics and unreliable on others. The wiki tracks this. A `contested` source tier means the entry is summarized for completeness but specific claims require caveats when used. The contested-claim list lives in `memory/` for fast lookup.

## What ingestion looks like in practice

1. PDF or book chapter dropped into the working folder.
2. Assistant reads it (full extraction, not skim).
3. Assistant drafts the wiki entry following this schema.
4. Cross-links are added; the `INDEX_<topic>.md` is updated.
5. If the source changes a standing memory rule (for example, a fact pack), the memory entry is updated with a pointer to the new source.
6. The PDF itself stays in a private `papers/` folder. The wiki entry is the public-facing artifact.

See `workflows/research.md` for the full ingestion playbook.

## See also

- `example-paper-note.md` for a sanitized real example
- `workflows/research.md` for the ingestion playbook
- `workflows/verification.md` for how wiki entries get checked before being trusted
