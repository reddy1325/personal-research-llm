# workflows/delegation.md

What the assistant routes back to the human. The PM-in-the-loop role.

## The reversal

The default frame for LLM work is "human prompts, model produces." This system inverts that. The assistant runs the research, drafting, verification, and bookkeeping. The human is the reviewer, the source of judgment, and the holder of authority for anything irreversible.

That is the PM job. The system names it because it matters.

## What the assistant routes back

### Final approval on irreversible actions

Nothing ships to an external surface without explicit approval. Posts, sends, publishes, deletes. The assistant prepares, the human approves.

Why: the cost of a wrong ship is high. The cost of an approval round is low. The asymmetry is permanent.

### Calls requiring taste

Is this draft too academic for the intended audience? Does the opening land? Is the closing argument fair to the source? Taste cannot be specced in advance. The assistant flags the call and the human makes it.

### Calls requiring political or cultural judgment

Should this draft engage on a borderline topic at all? Is the stance the right one for this moment? These calls have consequences the assistant cannot weigh. The human decides.

### Paywalled or inaccessible sources

When a source cannot be fetched, the assistant says so and asks the human to provide the material or skip the claim. No fabrication, no guessing.

### Real-world authentication

The human holds the keys. Account credentials, device access, anything that touches an external system. The assistant does not handle credentials and does not assume access it does not have.

### Priority and ordering when a batch is ambiguous

When five things are queued and three are equally pressing, the assistant asks rather than guesses. The order of operations is a judgment call.

### Course corrections when the assistant drifts

If a draft starts to read off-voice, the human says so. The correction goes into memory as a rule. The drift does not happen twice.

### Truth-checks against domain memory

The assistant's recall is fallible. When something feels off, the human spots it. "We did not say that," "the paper actually argues the opposite," "this number is from a different study." These corrections are the most valuable feedback the system gets.

## What the human routes to the assistant

The reverse flow:

- Initial research and ingestion of new sources
- Drafting against the existing knowledge base
- Cross-referencing and pattern identification
- Voice rules check on candidate drafts
- Maintaining the memory and wiki layers
- Preparing the work for the human's final review

The assistant handles the volume. The human handles the calls. Both sides have well-defined jobs.

## When delegation breaks down

It breaks when the assistant guesses instead of asking, or when the human ships without reviewing. Both produce bad outputs.

The discipline is mutual. The assistant asks more than it answers when the call is ambiguous. The human reviews before they approve. Neither side cuts corners on the handoff.

## See also

- `agent/instructions.md` — the operating spec that names the routing rules
- `verification.md` — the two-way fact-check layer
- `spec-driven.md` — why this is encoded rather than improvised
