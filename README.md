# personal-llm

A pattern for personal LLM operations: persistent memory, a structured knowledge backend, voice and verification gates, and multi-surface output. An extension of [Andrej Karpathy's llm-wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern, formalized for serious knowledge work.

This is an idea repo. It is designed to be read end-to-end and then forked, adapted, or borrowed from. The framework here is platform-neutral; the actual content it produces (papers, drafts, working notes) stays private.

## The core idea

Most people's experience with LLMs and documents looks like RAG: you upload a collection of files, the LLM retrieves relevant chunks at query time, and generates an answer. This works, but the LLM is rediscovering knowledge from scratch on every question. Nothing accumulates. Ask a subtle question that requires synthesizing five sources, and the LLM finds and pieces together the relevant fragments every time.

Karpathy's `llm-wiki` pattern fixes this by having the LLM build and maintain a persistent wiki — a structured, interlinked collection of markdown files that sits between you and the raw sources. The wiki keeps getting richer with every source you add. Maintenance is near zero because the LLM does it.

`personal-llm` extends that pattern in four ways:

1. **A typed memory layer** that survives across sessions and tracks the things a wiki cannot: who you are, what you are working on, which rules to follow, which authors are unreliable on which topics. The wiki is what you know; the memory is how you work.
2. **Voice and verification gates** so drafts produced from the wiki ship in your voice with their load-bearing facts verified, before any irreversible action.
3. **A failure-recovery loop** that encodes every incident as a durable spec entry, so the system never regresses on a known failure mode.
4. **Multi-surface output** — articles, reports, decks, briefings — all produced from the same wiki, no re-reading.

The key difference in one sentence: **the wiki is your knowledge; the system around it is your discipline.**

## Architecture

There are five layers.

**Raw sources** — your private corpus of papers, books, web clippings, primary documents. Immutable. The LLM reads from them but never modifies them. Stays local; never gets committed to the public repo.

**The wiki** — a directory of LLM-generated markdown files. One structured note per source, with frontmatter (author, year, topic, source-tier), key claims with locators, caveats, and `[[wikilinks]]` to related notes. Plus topic indices (`INDEX_topic.md`) that turn "every paper on X" into a one-line grep. The LLM owns this layer entirely.

**Memory** — a typed, frontmatter-tagged state layer that loads at the start of every session. Four memory types: `user` (who I am), `feedback` (how to work with me), `project` (what is in flight), `reference` (where to look for things). A thin index file (`MEMORY.md`) carries one-line pointers; deep content is lazy-loaded by reference. The index trick is what makes this scale: the full memory corpus is too big to live in context, but the index always fits.

**Playbooks** — the workflows that specify how each kind of work gets done: ingestion, web research, verification, voice, multi-surface output, failure recovery, delegation between human and assistant. Every behavior is written down before it runs. Edits to behavior are edits to a spec file, version-controlled like code.

**Skills** — specialist capabilities loaded on demand: document creation (docx, pptx, xlsx, pdf), deep research, skill authoring. Same knowledge base, different output surface. New surfaces are new skills, not rewrites.

See [`ARCHITECTURE.md`](./ARCHITECTURE.md) for diagrams of all of the above, including the end-to-end process flow and the failure-recovery loop.

## Operations

This is everything the system actually does in daily use.

**Ingest.** You drop a source into the raw collection and tell the LLM to process it. The LLM verifies identity (title, authors, year, edition), does a three-pass read (skeleton, targeted, detail), and produces a three-layer note (one-line takeaway, key claims with locators, full notes with method and caveats). The wiki entry is the durable contract with future-you about what this source said. Long or complex documents — papers over 30 pages, technical specs, requirements documents, dense academic monographs — get the same treatment, sectioned. Books are ingested chapter by chapter with a master entry linking them.

**Read deeply.** The three-pass read is designed for documents that matter. Pass 1: extract the table of contents, abstract or executive summary, all section headings, every figure and table caption. Pass 2: read the sections that carry the argument, with verbatim quotes to confirm the body actually loaded into context. Pass 3: walk through methodology, sample size, controls, edge cases. The pass that catches what a careless reader misses.

**Cross-reference.** The wiki schema turns reading into a queryable substrate. "Every paper on a given topic" is `grep -l "topics:.*X" wiki/*.md`. "Every author who is contested on a specific claim" is a frontmatter filter. "Every paper that supports a given claim" is a backlink search. When a new source contradicts an existing entry, the contradicting entry's `Contests:` section names the existing one, and the existing entry's caveats get updated. A pile of notes is not infrastructure. A graph is.

**Query.** You ask questions against the wiki. The LLM finds relevant notes via the index, reads them, and synthesizes a cited answer. The answer can take many shapes — a markdown page, a comparison table, a slide deck, a chart, a brief — depending on what you asked for. Good answers get filed back into the wiki as new pages so your explorations compound just like ingested sources do.

**Synthesize.** When ten to twenty wiki entries converge on a finding, the system distills them into a canonical fact pack. Future drafts pull from the pack rather than re-deriving the pattern. This is how a library of papers becomes a small set of high-confidence claims that any draft can rely on.

**Draft across surfaces.** A single wiki entry can feed many outputs without re-reading the source:

- *Articles and article series* — long-form publishing in sequence, composed shape, careful citations, source-faithful by default
- *Reports* — prose documents structured by argument (not by source), calibrated language, full attribution
- *Decks* — slide presentations, one idea per slide, evidence pulled from the wiki, rhythm from the deck
- *Briefings* — compressed action-oriented summaries, action-relevant findings only, fast to read

Each surface has its own playbook. The transforms are documented so the same knowledge base feeds whatever shape you need.

**Verify.** Load-bearing facts get verified before drafting, not after. Verification means four things: number (is the figure correct?), attribution (did this author actually say this?), scope (does the claim apply where the draft is applying it?), and interpretation (does the framing match what the source argued?). Drafts include verification annotations so the source is traceable. A draft built on verified facts is cheap to revise. A draft built on a wrong claim is expensive to retract.

**Check voice.** Every draft runs through three layers: persona (who I write as), style bible (synthesized shape rules), and a voice-rules check that scans for AI tells — banned punctuation, banned phrases, banned structures. Failed checks get rewritten, not patched. A draft that fails three rewrites in a row signals the angle is wrong, not the wording.

**Ship with approval.** Nothing irreversible ships without explicit human approval. The assistant prepares; the human approves. Final approval, taste calls, stance decisions on borderline topics, course corrections, paywalled sources that need credentials — all routed back to the human. The assistant handles volume; the human handles judgment.

**Resume cold.** A conversation about a project can resume three weeks later with zero re-briefing. The memory index loads, the assistant walks in already knowing who I am, what I am working on, which rules to follow, which sources are reliable, and which incidents have produced durable rules. This is the flagship property: most LLM workflows lose state every chat; this one does not.

**Recover from failures.** Every incident produces a durable rule. The system gets quieter over time because the rules accumulate. A wiki entry was duplicated under a slightly different filename? New rule: grep for the author and year before any new entry. A publishing surface silently dropped an item? New rule: after-ship verification against the actually-published artifact. A fact-error caught on review? New rule: verify load-bearing facts before drafting, with an inline annotation per claim. See [`workflows/failure-recovery.md`](./workflows/failure-recovery.md) for the encoded learning loop.

**Browser research.** When the source is on the web, the browser-research playbook covers tool selection (dedicated MCP > browser MCP > raw fetch), one-shot extraction (no chunked slicing, no waiting on events that may never fire), site-type handling (news, blogs, academic journals, Substack and long-form, single-page apps, paywalled content), verification (did the right content load?), and escalation when content is blocked.

**Compose specialists.** Specialist output formats — docx, pptx, xlsx, pdf, deep-research — load on demand. The wiki and memory layers do not know which surface is being targeted. The skill handles the format. The system handles the substance. Adding a new output surface is a new skill, not a rewrite.

**Spec-driven, not chat-driven.** Every behavior is written down before it runs. `agent/instructions.md` is a spec. `agent/persona.template.md` is a spec. `agent/voice-rules.md` is a spec. Every standing rule in memory has a `Why:` line (so edge cases can be reasoned about) and a `How to apply:` line. When the spec is wrong, you edit the spec; the assistant follows whatever is on disk.

## Indexing and logging

Two patterns keep the system navigable as it grows.

`MEMORY.md` is the index for state. One line per memory entry, grouped by category. Loads automatically into every conversation. The full memory corpus is too big for context; the index always fits. Keep it under 200 lines.

`INDEX_<topic>.md` is the index per wiki topic. One line per source with the one-line takeaway. Findable by grep. Avoid the need for embedding-based RAG infrastructure at moderate scale.

A chronological `log.md` per project records what was ingested, what was drafted, what shipped. Consistent prefixes (`## [2026-06-04] ship | Article Title`) make the log parseable with simple unix tools — `grep "^## \[" log.md | tail -5` gives the last 5 entries.

## Why this works

The tedious part of maintaining a knowledge system is not the reading or the thinking — it is the bookkeeping. Updating cross-references, keeping summaries current, noting when new data contradicts old claims, maintaining voice consistency across dozens of drafts, encoding every failure into a rule so it does not recur. Humans abandon systems like this because the maintenance burden grows faster than the value.

LLMs do not get bored. They do not forget to update a cross-reference. They can touch fifteen wiki entries in one pass. They will check `MEMORY.md` at the start of every conversation without being asked. The system stays maintained because the cost of maintenance is near zero.

The human's job is to curate sources, direct the analysis, ask the right questions, make the taste calls, approve the ships, and think about what it all means. The assistant's job is everything else.

This pattern is related in spirit to Vannevar Bush's Memex (1945) — a personal, curated knowledge store with associative trails between documents. Bush's vision was closer to this than to what the web became: private, actively curated, with the connections between documents as valuable as the documents themselves. The part he could not solve was who does the maintenance. The LLM handles that.

## Tips

- The wiki is a git repo of markdown files. Version history, branching, and review are free.
- Markdown everywhere is a hedge against tool churn. The substrate does not die when the tool does.
- Keep `MEMORY.md` under 200 lines. Anything longer gets truncated when loaded. Move detail into topic files; the index stays thin.
- Every memory entry needs a `Why:` line. A rule without a reason gets misapplied. A rule with a reason can be reasoned about in edge cases.
- Source-tier discipline matters more than coverage. Track which authors are reliable on which topics. A great author on one subject can be contested on another. Encode the nuance so it does not get lost.
- Three-pass reading scales better than one-pass on long documents. Skim-summaries miss the load-bearing details.
- Multi-surface output compounds. Twenty wiki entries can produce a fact pack, a report, an article series, a deck, and briefings — all from the same ingestions.
- Failures are features. Every incident that produces a durable rule is a permanent upgrade to the system.
- The `Why:` line on standing rules is what lets the system handle edge cases. A rule without a `Why:` gets misapplied within a week.

## Note

This repo is intentionally a framework, not a finished product. The exact directory structure, the schema conventions, the page formats, the tooling — all of that will depend on your domain, your preferences, and your LLM of choice. Everything here is modular: pick what is useful, ignore what is not. Your raw sources will be different from mine. Your topic lanes will be different. Your voice will definitely be different. The right way to use this is to fork it, work with your LLM to instantiate a version that fits your needs, and let it co-evolve with you over months.

## Credits

Inspired by and built on top of Andrej Karpathy's [llm-wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) pattern. The wiki layer is his idea; the architecture diagrams owe debt to his way of drawing things; the phrasing of "the LLM does the bookkeeping" is his.

Built collaboratively with Claude across many months of real use.

## License

MIT.
