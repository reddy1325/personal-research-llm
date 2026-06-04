# workflows/multi-surface-output.md

One ingestion. Many outputs. How the same source material feeds threads, reports, decks, replies, and briefings without re-reading.

## The principle

A paper is ingested once. It produces a wiki entry. From that entry, many drafts can be written without ever re-reading the PDF. The cost of ingestion is paid once; the value compounds across every future use.

This is the opposite of how most people work with LLMs, where the same PDF gets pasted into chat over and over and the model has to re-summarize each time.

## The output surfaces

The system produces work for several surfaces from the same knowledge base:

- **Threads.** Long-form public posts in series. Composed register, careful citations, source-faithful.
- **Replies.** Short-form reactions. Phone-casual register, sharper kickers.
- **Reports.** Prose documents for personal or shared use. Calibrated language, full attribution.
- **Decks.** Slide presentations. Spare, one idea per slide.
- **Briefings.** Stand-ups or status pages. Compressed, action-oriented.

Each surface has its own playbook and its own register. The underlying material is the same.

## How a single source flows

A paper on a topic gets ingested. The wiki entry holds:

- Frontmatter (authors, year, topics, source-tier)
- One-line takeaway
- Key claims with locators
- Method
- Cross-links
- Caveats
- Quotes worth keeping

From that entry, a thread can be drafted by pulling key claims and quotes. A report can be drafted by pulling method and caveats. A deck can be drafted by pulling the one-line takeaway plus one supporting figure per slide. None of these need the original PDF in context. The wiki entry is sufficient.

## Why this matters at scale

Twenty papers on a topic, each with a wiki entry, produce:

- A canonical fact pack synthesizing the convergent findings
- A long-form report covering the topic comprehensively
- A thread series introducing the topic to a general audience
- A deck for any presentation on the topic
- Reply material whenever the topic surfaces in conversation

All from the same ingestions. No re-reading. The wiki turns reading into infrastructure that pays out over months.

## Surface-specific transforms

Each surface has its own transform from wiki to draft:

- **Thread.** Pick a hook claim, build the argument across posts, close with a sharp landing. One claim per post, one stat maximum.
- **Report.** Structure by argument, not by source. The wiki entries support the report; they do not dictate its shape.
- **Deck.** Each slide is one beat of the argument. The wiki provides the evidence; the deck provides the rhythm.
- **Reply.** The wiki provides the receipt; the reply provides the voice.

The transforms are documented in their own surface playbooks.

## Composability via skills

Specialist output formats (docx, pptx, xlsx, pdf) load on demand. The wiki and memory layers do not know which surface is being targeted. The skill handles the format. The system handles the substance.

This separation is what makes the system extensible. A new output surface is a new skill, not a rewrite.

## See also

- `wiki/schema.md` for what the wiki holds
- `research.md` for how sources get into the wiki
- `voice.md` for how registers differ across surfaces
