# workflows/spec-driven.md

Every behavior is written down before it runs. This is the discipline that makes the system reliable.

## The principle

If a rule is not in a spec file, it does not exist. The assistant does not infer rules from past chats. It reads them from disk every session.

Specs in this system:

- `agent/instructions.md`, how the assistant operates
- `agent/persona.template.md`, who the assistant writes as
- `agent/voice-rules.md`, the adversarial QA layer
- `workflows/*.md`, how each kind of work gets done
- `memory/*.md`, standing rules, project state, references

Every spec is plain English, in Markdown, in version control. A future you can read it. A collaborator can review it. The assistant can follow it.

## How a new rule enters the system

1. Something happens that should not happen again. Or something works that should keep working.
2. The reason is identified. Not just "what went wrong" but *why*.
3. A spec entry is written: the rule, a `Why:` line, a `How to apply:` line.
4. The entry goes in the right place (memory if it is a standing rule, a workflow doc if it is a process).
5. The index (`MEMORY.md`) gets a pointer.
6. Next session, the rule loads automatically.

The `Why:` line is what makes this work in edge cases. A rule without a reason gets misapplied. A rule with a reason can be reasoned about.

## What this looks like in practice as a PM

Speccing an agent is the same discipline as speccing a feature. You write what it should do, why, and how to know it is working. You version-control the spec. You review it before it ships.

What changes:

- The "implementation" is plain English. There is no code translation step.
- The reviewer is the model. It reads the spec and follows it.
- Iteration is fast. A new rule is a commit, not a deploy.

What stays the same:

- Specs are written first.
- Edge cases produce new spec entries.
- Failures get encoded so they do not regress.

## The encoded learning loop

Failures produce spec entries. Spec entries prevent recurrence. The system gets quieter over time, not louder.

A real example: when an early run shipped two responses to the same parent item, the fix was not "remind the assistant to check." The fix was a memory rule: never respond twice to the same item, with a `Why:` and a `How to apply:`. The rule has been in place ever since. The failure has not recurred.

This is the difference between a chatbot that does what you say and a system that does what you have already taught it.

## See also

- `agent/instructions.md`, the top-level spec
- `memory/schema.md`, how rules are formatted
- `failure-recovery.md`, examples of incidents that produced rules
