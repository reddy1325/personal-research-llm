# workflows/context-persistence.md

How the same conversation resumes cold weeks later. The flagship property of the system.

## The problem

Most LLM workflows lose state every chat. The user pastes context, the assistant pretends to understand, both sides start over. Three sessions in, the assistant has forgotten the rules established in session one. Six sessions in, the human has stopped trying to maintain continuity.

This breaks any kind of serious knowledge work. You cannot build a research arc, a long writing project, or a multi-week investigation if every session is a cold start.

## The solution

Three persistence mechanisms, layered:

### Memory (state)

`MEMORY.md` loads automatically at the start of every conversation. It contains one-line pointers to every standing rule, project, user fact, and reference. The assistant walks in already briefed.

A new chat about a thread series, three weeks after the last session, starts with the assistant knowing:

- The series order and which parts have shipped
- The author stance for this series
- The voice rules that apply
- Which sources are reliable and which are contested
- Which mistakes have been made before and how they are encoded as rules

### Wiki (knowledge)

The wiki is consulted on demand. When a draft needs a fact, the wiki has the source. When a synthesis needs ten papers, the wiki has the index. The wiki does not need to be in context; it just needs to be reachable.

### Working folder (project artifacts)

Drafts, batch files, in-progress work live in the working folder as MD files. The current state of any project is visible by looking at the folder. There is no hidden state.

## Why MD

Each layer uses plain Markdown. The persistence works because:

- Files survive between sessions. Trivially.
- Files survive across machines. Just copy the folder.
- Files survive across tools. If I switch models tomorrow, the files still work.
- Files are diffable. I can see exactly what changed between sessions.
- Files are greppable. "What did we decide about X" is one search away.

A SaaS notes app dies in 18 months. The filesystem does not.

## Resuming a cold conversation

Three weeks have passed. New chat opens. The assistant:

1. Loads `MEMORY.md`. Sees the project state, the voice rules, the user profile.
2. Reads any project-specific memory pointers. Now knows where the work stands.
3. Asks one clarifying question if anything is ambiguous. Does not guess.
4. Resumes work in the right register, with the right rules, against the right sources.

No re-briefing. No "let me catch you up on what we are doing." The context is already there.

## When persistence breaks

When it breaks, it almost always means a memory was not updated when it should have been. The fix:

- Identify the missing memory.
- Write it.
- Add the pointer to `MEMORY.md`.
- Next session, the gap is closed.

This is the learning loop. Failures produce durable rules, not just one-time fixes.

## See also

- `memory/schema.md` for the memory spec
- `wiki/schema.md` for the knowledge backend
- `spec-driven.md` for the broader written-down-first discipline
