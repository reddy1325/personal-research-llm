# workflows/failure-recovery.md

The learning loop. Failures become durable rules. The system gets quieter over time.

## The pattern

Something goes wrong. The fix is not just to correct the immediate output. The fix is to write a spec entry so the same failure cannot happen again. Next session, the rule loads automatically. Same failure mode is now closed.

This is the difference between a chatbot and a system. A chatbot forgets. A system encodes.

## Anatomy of a recovery

1. **Incident.** Something shipped wrong, or nearly did.
2. **Diagnosis.** What was the assistant relying on that turned out to be wrong? Was a rule missing? Was a rule misapplied? Was a verification skipped?
3. **Encoded fix.** A new spec entry, with the rule, the `Why:` (citing the incident), and the `How to apply:`.
4. **Index update.** `MEMORY.md` gets a pointer to the new rule.
5. **Quiet.** Next session, the rule loads. The failure mode is closed.

## Examples

These are sanitized versions of real recoveries.

### Duplicate wiki entry for the same source

**Incident.** A paper was ingested twice into the wiki under slightly different filenames, splitting the cross-link graph in two and causing downstream drafts to miss half the relevant claims.

**Diagnosis.** The ingestion playbook did not include a duplicate check. The second ingestion treated the paper as new because the filename differed.

**Encoded fix.** Before any new wiki entry is written, grep for the author and year. If a match exists, the new ingestion is appended to or merged with the existing entry rather than creating a duplicate.

**Status.** Encoded in the research workflow. Duplicate entries have not recurred.

### Composer-clear treated as "shipped"

**Incident.** A publishing surface silently dropped an item. The composer cleared, suggesting success. The item never appeared.

**Diagnosis.** The assistant equated "composer cleared" with "shipped." The surface's actual state was not checked.

**Encoded fix.** Post-ship verification step: after any batch, check the actual published artifact, not the composer state.

**Status.** Encoded as a standing rule in the publishing playbook.

### Fact-error caught on review

**Incident.** A draft contained a number that was off by a factor of ten. The human caught it on review.

**Diagnosis.** The number was a load-bearing claim. It was not verified before drafting; it was carried over from memory of a related paper.

**Encoded fix.** Verify load-bearing facts before drafting, not after. Every load-bearing claim gets a verification annotation in the draft.

**Status.** The rule applies to every draft now.

### Voice drift across a long article

**Incident.** A multi-section article started in the right voice and drifted into AI-slop cadence by the closing sections.

**Diagnosis.** The voice rules check was run on the opening section but not on later sections in the same batch.

**Encoded fix.** The voice rules check runs on every section in a batch, not just the first. A drift in the closer is treated as a check failure.

**Status.** Encoded in the voice workflow.

## What this looks like in aggregate

Across the life of the project, dozens of incidents have produced standing rules. The memory file grows with each one. The system has not regressed on any closed failure mode.

The shape of the work has changed as a result. Early sessions had to be watched closely. Recent sessions need less supervision because the rules have accumulated.

## The PM read

A system that encodes its mistakes is the same idea as a regression test suite, applied to an LLM. Each incident becomes a test. The test never goes away. The behavior is permanent.

This is what makes the system trustworthy enough to delegate real work to.

## See also

- `spec-driven.md`, why behaviors are written down first
- `memory/schema.md`, how rules are stored
- `verification.md`, the rules that protect the fact layer
