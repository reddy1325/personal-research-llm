# workflows/voice.md

How voice consistency is maintained across surfaces and across months.

## The three layers

**Persona** (`agent/persona.template.md`) — who I write as. Background, lanes, hard-no topics.

**Style bible** — synthesized voice rules. Hook types, structures, vocabulary that sounds native to serious writing.

**Voice rules check** (`agent/voice-rules.md`) — the kill list. Adversarial QA for AI tells.

All three load before any drafting begins. The order matters: persona shapes what gets said, style bible shapes how it gets said, the voice rules check filters what slips through.

## The check, in order

1. Persona match: is the draft something I would actually say given my topic lanes and hard-nos?
2. Structure match: does the shape fit the output (article, report, deck, briefing)?
3. Voice rules: scan for banned punctuation, banned phrases, banned structures.
4. Read-aloud: does it sound like a person or a press release?
5. Calibration: are hedges and certainties placed where they belong?
6. Approval round: present to the human before any irreversible ship.

A draft that fails the check is rewritten, not patched. Patching produces Frankenstein voice.

## Output shapes

The same voice has different shapes per output:

- **Long-form articles and article series.** Composed. Full citations where they belong. One stat per beat. Each beat stands alone.
- **Reports.** Prose, not bullets. Calibrated language. Source-faithful by default. Structure by argument, not by source.
- **Decks.** Spare. One idea per slide. The voice still shows up, just compressed.
- **Briefings.** Action-oriented. Compressed. What changed, what is next.

The voice rules check runs on every shape. A report written in deck cadence reads thin; a deck written in report cadence reads cluttered.

## Source-faithful default

The default mode is: represent what the source actually says before adding interpretation. Polemical, defensive, or argumentative framings are reserved for explicit invitation.

Why: most readers can tell when a writer has prejudged the source. A fair exposition first builds the credibility that lets an argument land later.

## When the voice rules check fails three times

Stop. If three rewrites in a row still trip the check, the angle is wrong, not the wording. Drop the draft and ask the human what they actually want to say.

## See also

- `agent/voice-rules.md` for the kill list
- `agent/persona.template.md` for the persona spec
- `failure-recovery.md` for how voice rules accumulate
