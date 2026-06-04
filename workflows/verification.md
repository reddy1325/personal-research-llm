# workflows/verification.md

How facts get checked before they ship. Two-way: the assistant verifies the human's claims, the human verifies the assistant's recall.

## The rule

Verify load-bearing facts **before drafting**, not after. Verification means four things:

1. **Number.** Is the figure correct? Is the unit correct? Is the sample size what was claimed?
2. **Attribution.** Did this author actually say this? In which work? What page?
3. **Scope.** Does the claim apply where the draft is applying it? A finding from one region or period does not automatically generalize.
4. **Interpretation.** Even if the number and attribution are correct, does the draft's framing match what the source actually argued?

A draft built on a verified foundation is cheap to revise. A draft built on a wrong claim is expensive to retract.

## When verification kicks in

- Any specific number (sample size, date, percentage)
- Any direct attribution ("Smith 2024 found...")
- Any universal claim ("every member of a group does this," "no member of a group does this")
- Any historical or technical claim outside the wiki
- Any claim that would be embarrassing if wrong

When in doubt, verify. The cost of a search is small. The cost of shipping a wrong claim compounds.

## How verification is recorded

Drafts include a verification annotation per load-bearing claim. Format:

```
CLAIM: <the load-bearing claim>
VERIFIED: <source + locator + quote or URL>
```

If a claim cannot be verified to confidence, it is either dropped or flagged as contested in the draft itself. False certainty is the worst outcome.

## After-ship verification

Some publishing surfaces have a gap between "composer cleared" and "actually shipped." That gap is where errors hide. After any batch ship, the actual published artifact is checked against the draft. If the surface silently dropped an item, the verification step catches it.

This is a habit that emerged from a real incident. The rule is encoded.

## Two-way checking

The assistant verifies the human's claims.
The human verifies the assistant's recall.

When the assistant says "we ingested this paper in March," the human can spot a hallucination because the human knows the actual project history. When the human says "the paper claims something specific," the assistant can flag if the wiki entry disagrees.

Neither side ships without the other.

## What never counts as verification

- "I am confident in this" without a source
- A search result that does not actually contain the claim
- A quote that has been paraphrased into something different
- A claim derived from a contested author's contested topic without a caveat

## See also

- `research.md` for how facts get into the wiki in the first place
- `failure-recovery.md` for incidents that produced verification rules
