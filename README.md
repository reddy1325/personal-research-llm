# personal-research-llm

A personal LLM operations stack for research, writing, and decision support.

The assistant directs research and drafting. The human curates judgment, sources, and ship decisions. This repo is the system: how memory persists across sessions, how a knowledge backend gets built one paper at a time, how voice rules are encoded, how facts get verified before they ship, and how a single ingestion produces articles, reports, decks, and briefings from the same source material.

Built collaboratively with Claude over many months as a working PM's daily tool. Not a script. Not a single agent. An ops layer.

## What this is, in one paragraph

Most LLM workflows start over every chat. This one does not. A persistent memory layer loads at the start of every session and tells the assistant who I am, what I am working on, which authors are reliable on which topics, and which mistakes have already been made and corrected. A separate wiki layer holds the knowledge backend: structured notes on every paper, book, and primary source I have ingested, indexed and cross-linked. Playbooks codify how work gets done. Skills compose for specialist outputs. The result is an assistant that walks into every conversation already briefed.

## Why this exists

I am a product manager. I read a lot, write a lot, and care about getting facts right. Off-the-shelf chatbots forget everything between sessions, hallucinate citations, and produce voice that reads like a press release. None of that works for serious knowledge work. So I built the layer that makes the LLM useful for the work I actually do.

This repo is the framework, not the content. The content (papers, drafts, personal research notes) stays local. The system design is here for review.

## What you will find in this repo

- `ARCHITECTURE.md` — the four-layer view: memory, wiki, playbooks, skills. Why MD everywhere. Context-window economics.
- `agent/` — instructions, persona template, voice rules. The spec that tells the assistant how to behave.
- `memory/` — schema for the persistent state layer, with sanitized examples of each memory type.
- `wiki/` — schema for the knowledge backend, with a sample paper note.
- `workflows/` — eight playbooks covering research, verification, voice, context persistence, spec-driven development, delegation, failure recovery, and multi-surface output.
- `examples/` — end-to-end case studies (optional, sanitized).

## What this repo is not

Not a single-purpose tool. Not magic. The framework is the artifact; the private content it produces stays local.

## License

MIT. Take what is useful. Build your own.
