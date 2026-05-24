# Trigger Test — `prd-drafter`

Five prompts to verify the skill's description triggers correctly. Run each in a fresh Claude Code session (with the skill installed and `CLAUDE.md` loaded) and record whether the skill was auto-invoked **without** typing `/prd-drafter`.

---

## Should invoke (3)

### Prompt 1 — Direct ask, no skill name mentioned

> "I've got a bunch of Confluence notes on a new feature to support Sys Log. Can you turn these into a proper PRD?
>
> [paste Confluence content]"

**Why this should invoke:** "Confluence notes" + platform context (Sys Log) + "turn these into a proper PRD" hits three triggers in the description (raw-Confluence input pattern, platform domain, explicit "PRD" verb).

- [Y] Invoked
- [ ] Did not invoke


---

### Prompt 2 — Persona-prefixed, AB's natural style

> "Assume you are a senior network architect with CCIE/CCDE-level accreditation, with prior product experience at Cisco, Juniper, and Versa. Below are notes from a customer call about overlay tunnel auto-healing. Draft a PRD."

**Why this should invoke:** "Draft a PRD" verb + the persona-prefix pattern is explicitly named in the description as a trigger.

- [Y] Invoked
- [ ] Did not invoke



---

### Prompt 3 — Implicit, no "PRD" word

> "Spec out the IPAM auto-allocation flow for aviation sites — full doc, dev-ready, the way we usually do it."

**Why this should invoke:** "Spec out" + platform context (IPAM, aviation) + "full doc, dev-ready" + "the way we usually do it" together signal a house-style PRD even without the literal word "PRD". The description's "when in doubt and the conversation is in platform context, prefer invoking" line is what catches this case.

- [Y] Invoked
- [ ] Did not invoke


---

## Should NOT invoke (2)

### Prompt 4 — Adjacent but wrong format

> "Write me an Amazon-style 6-pager on the Smart Site Data Portal."

**Why this should NOT invoke:** "Do NOT invoke" section explicitly excludes Amazon-style 6-pagers. Domain context alone should not be enough to drag the skill in.

- [Y] Did not invoke (correct)
- [ ] Invoked (false positive)


---

### Prompt 5 — Summary, not drafting

> "Read this Smart Site Data Portal PRD and give me the executive summary in 5 bullets.
>
> [paste existing PRD]"

**Why this should NOT invoke:** User wants summarisation of an *existing* PRD, not a new one. The "Do NOT invoke when" section names this case directly.

- [Y] Did not invoke (correct)
- [ ] Invoked (false positive)
