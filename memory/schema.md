# memory/schema.md

The state layer. Persists across every conversation. Loaded automatically at session start.

## The four types

Every memory file is exactly one type. The type determines how the memory is used.

### user

Facts about who I am, my role, my expertise, what I work on. Used to tailor explanations and judgment.

Example: "User is a product manager focused on AI-native tooling. Frame technical explanations in product-management terms rather than implementation terms."

### feedback

Standing rules about how to work with me. Both corrections (what to stop doing) and validations (what to keep doing). Always include the `Why:` so edge cases can be judged, not pattern-matched.

Example: "Show full draft text for approval before publishing any batch. Why: a batch shipped directly once contained a fact error the human would have caught. How to apply: every batch run, drafts go to a durable file plus the chat for review."

### project

What is in flight. Who is doing what, by when, why. These change fast; the schema requires absolute dates so memories stay interpretable after time passes.

Example: "Long-form article series in progress on a single topic. Order: part 1 (done) → part 2 → part 3 → part 4 → part 5. Author stance: source-faithful exposition, defense reserved for the final part. As of 2026-05-25."

### reference

Pointers to external systems. Where to look for things that live outside this repo.

Example: "Publishing playbook for long-form articles: outline conventions, citation style, image-handling rules, pre-publication checklist. Lives at `reference_publishing_playbook.md` in private memory."

## File format

Every memory file is a single Markdown file with YAML frontmatter:

```markdown
---
name: short-kebab-case-slug
description: One-line summary used to decide relevance in future conversations
metadata:
  type: user | feedback | project | reference
---

Body of the memory. For feedback and project types, lead with the rule or fact,
then a **Why:** line and a **How to apply:** line.

Cross-link related memories with [[their-name-slug]].
```

## The index trick

`MEMORY.md` is the index file. It loads into every conversation automatically. It contains one-line pointers to every memory file, grouped by topic:

```markdown
## Working style, hard rules
- [Present drafts before shipping](feedback_present_drafts.md), Show full draft for approval

## Voice bans
- [No em dashes](feedback_no_em_dashes.md), Top AI tell; never in any output

## User profile
- [Product manager focus](user_role.md), AI-native PM, frame explanations as such
```

The index is small enough to always fit in context. The bodies load on demand. This is how a multi-megabyte memory layer fits inside a context window.

## What never goes in memory

- Code patterns or architecture (derivable from the repo)
- Git history (use `git log`)
- Ephemeral task state (use the task tool)
- Sensitive personal information unless explicitly requested

## Update discipline

- When you learn something new about how the human wants to work, save it.
- When a correction is given, save the rule plus the `Why:`.
- When something validated works, save that too. Skipping confirmations causes drift toward over-cautious behavior.
- Before recommending from memory, verify the underlying fact has not become stale.

## See also

- `examples/` for sanitized samples of each memory type
- `workflows/spec-driven.md` for how memory connects to the broader spec layer
