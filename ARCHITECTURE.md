# Architecture

Four layers, each doing one job. The layers compose. None of them depend on a specific model or vendor.

```
+-------------------------------------------------------------+
|  Skills           docx | pptx | xlsx | deep-research | ...  |
|  (capabilities)   loaded on demand, composable              |
+-------------------------------------------------------------+
|  Playbooks        research | verification | voice | ...     |
|  (workflows)      how work gets done, written down          |
+-------------------------------------------------------------+
|  Wiki             every paper, book, source as a note       |
|  (knowledge)      frontmatter + cross-links + index         |
+-------------------------------------------------------------+
|  Memory           who I am, what I am working on, what to   |
|  (state)          avoid. Loads every session.               |
+-------------------------------------------------------------+
```

## Layer 1: Memory

A typed, frontmatter-tagged state layer that persists across every conversation. Four types: `user` (who I am), `feedback` (how to work with me), `project` (what is in flight), `reference` (where to look). Each memory is a single MD file. A thin index file (`MEMORY.md`) is loaded into every new conversation as a list of one-line pointers. Deep content is lazy-loaded by reference.

The index trick is the whole game. The full memory corpus is too big to live in context. The index is small enough that it always fits, and rich enough that the assistant knows where to look.

See `memory/schema.md` for the full spec.

## Layer 2: Wiki

The knowledge backend. Every paper I read, every book I work through, every primary source gets a structured note. Notes have frontmatter (author, year, topic tags, source tier), a summary, key claims, and `[[wikilinks]]` to related notes. An `INDEX.md` per topic area collects pointers.

This is institutional memory. Not a notes folder. A future collaborator could read it and onboard.

See `wiki/schema.md`.

## Layer 3: Playbooks

Every workflow has a written rulebook. How a paper goes from PDF to wiki entry. How a factual claim gets verified before it ships. How drafts get reviewed. What gets delegated back to the human. Playbooks live in `workflows/` and are version-controlled like code.

When a workflow fails, the fix is a new playbook entry or a new memory rule. Failures do not regress.

## Layer 4: Skills

Specialist capabilities loaded on demand. Document creation (docx, pptx, xlsx, pdf). Deep research (multi-source fan-out with adversarial verification). Skill authoring (skills that create other skills). Same knowledge base, different output surfaces.

## Cross-cutting choices

### Markdown everywhere

Every layer uses plain Markdown files. Not a database. Not a vector store. Not a SaaS notes app.

Why:

- Diffable in git. I can see what changed.
- Greppable in one command.
- LLM-native. No parser, no schema validator, no SDK.
- Portable. The same files work in any editor, any tool, any future model.
- Self-documenting via YAML frontmatter.
- Composable via `[[wikilinks]]`.
- No vendor lock-in. If I swap models tomorrow, the files still work.

Markdown is a hedge against tool churn. Most knowledge systems get rebuilt every two years because their substrate dies. This one will not.

### Context-window economics

The wiki is several gigabytes. It cannot live in the assistant's context. The architecture is shaped by that constraint.

Solution: a thin index (`MEMORY.md`) loads every session and points to deep content. The assistant reads only what it needs, when it needs it. This is the difference between a system that scales and one that does not.

### Spec-driven, not chat-driven

Every behavior is written down before it runs. `agent/instructions.md` is a spec. `agent/persona.template.md` is a spec. `agent/voice-rules.md` is a spec. Every standing rule in memory has a `Why:` line and a `How to apply:` line.

This is the same discipline you would use to spec a feature at work. Plain English, version-controlled, reviewable. The assistant follows the spec. When the spec is wrong, you edit the spec.

### Two-way fact-checking

The assistant verifies load-bearing facts before drafting, not after. The human verifies the assistant's recall against domain knowledge the assistant does not have. Both sides correct each other. See `workflows/verification.md`.

### Failure recovery as a system feature

Nearly every standing rule in `memory/` traces to a specific incident. The system encodes its own mistakes so they do not happen twice. See `workflows/failure-recovery.md`.
