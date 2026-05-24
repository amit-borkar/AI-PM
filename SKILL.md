---
name: prd-drafter
description: Use when the user wants to draft, spec out, or convert raw notes into a house-format Product Requirements Document. Produces a dev-ready PRD following the strict house spine (Exec Summary → Scope → Definitions → User Stories → Architecture → Functional Requirements → NFRs → Corner Cases → Migration → Data Model) with British spelling, US-N/FR-N/C-N identifiers, Connextra user stories, and the signature **Condition / Behaviour** corner-case format. Triggers on phrases like "draft a PRD", "PRD this", "spec out X", "turn this Confluence into a PRD", "write a PRD for Y", "convert these notes into a PRD", and persona-prefixed asks like "as a senior network architect / CCIE / CCDE, draft a PRD". Use this skill whenever a house-context PRD is requested even if the user does not explicitly say the word "PRD" — for example "spec out the IPAM auto-allocation flow", "I have Confluence notes, give me a proper document", or "write up the overlay tunnel auto-healing module dev-ready". When in doubt and the conversation is in platform product context (maritime, IPAM, Grid, templates, hub/spoke, satellite), prefer invoking this skill over a generic draft.
---

# PRD Drafter

Encodes the house pattern for product requirements documents. Outputs a complete, dev-ready PRD that an engineering lead with CCIE/CCDE-grade network expertise can take straight to implementation.

## When to invoke

**Invoke when:**
- User pastes Confluence notes, customer-call notes, an exec ask, or a system sketch and wants a PRD out of it.
- User says any of: "draft a PRD", "spec out X", "PRD this", "turn this into a PRD", "convert this to a PRD", "write a house-style PRD for X".
- User persona-prefixes the request: "Assume you are a senior network architect / CCIE / CCDE / with prior work at Cisco, Juniper, Versa…"
- Conversation is clearly inside the platform's product context (maritime, IPAM, Grid, templates, hub/spoke, satellite, NMS, controller) and the user asks for a "doc", "spec", or "writeup" of a system.

**Do NOT invoke when:**
- User wants a *summary* of an existing PRD, not a new one. (Use plain summarisation.)
- User wants a one-pager, exec brief, pitch deck, or Amazon-style 6-pager — those have different shapes; PRD is the wrong template.
- User wants Jira tickets, user stories alone, or an RFC. PRDs are larger and more structured.
- User explicitly asks for a non-standard format ("Amazon 6-pager", "RFC-2119-style spec", "lightweight 1-pager").

## Inputs the skill expects

Before drafting, confirm or extract:

1. **Raw material** (required) — Confluence dump, notes, call transcript, sketch, or problem brief. If thin, ask one targeted clarifying question; otherwise proceed with assumptions listed explicitly in §2.2 Out of Scope.
2. **Vertical(s)** (required) — Maritime / Aviation / Energy / Commercial. Drives §11 compliance references.
3. **Companion PRDs** (optional) — Names + version pins of any PRDs this one depends on. Drives the `FR-Xa: Schema Dependency` pattern.
4. **Replacing an existing workflow?** (optional) — If yes, §14 Migration Path is required.
5. **Audience emphasis** (optional) — Default is ENG + Design + EXEC. Adjust §1 readability accordingly.

If a *required* input is missing, ask once. Don't ping-pong; if the user wants to proceed with gaps, fill with reasonable assumptions and call them out in §2.2.

## Step-by-step process

1. **Parse the raw material.** Identify: the broken status quo, the proposed system, components, actors (delivery engineer, network architect, capacity planner, compliance officer…), data entities, external systems touched.
2. **Draft §1 Executive Summary.** Three subsections in order:
   - 1.1 Problem (status quo + error classes, *exec-readable, no CCIE jargon*).
   - 1.2 Solution Overview (the system in N tightly-integrated components).
   - 1.3 Design Principles (named, bulleted — "Fail fast, fail early", "Single source of truth", "Automatic with human confirmation", "Immutable audit trail", etc. Reuse house principles where applicable; coin new ones sparingly.).
3. **Define §2 Scope.** §2.1 In Scope (concrete capabilities, bulleted). §2.2 Out of Scope (anything ambiguous goes here as an assumption with rationale).
4. **Build §3 Key Concepts and Definitions.** Terminology table. Every domain term used later gets a row. Reuse industry terms — don't invent.
5. **Write §4 User Stories.** Group by capability area (4.1, 4.2…). Format: `US-N: [Title]` then `As a [role], I want [capability]—so that [benefit].` Em-dash, no comma-splice.
6. **Specify §5 High-Level Architecture.** Layers or tiers (Layer 1 / Layer 2…). Include End-to-End Workflow as a numbered step list (Step 1, Step 2…) covering happy path AND branching (validation pass/fail, success/failure outcomes).
7. **Functional Requirements (§6 onwards).** One section per module. Number `FR-N`. Use `FR-Na`/`FR-Nb` for closely-coupled sub-items (e.g., FR-7 + FR-7a for a transaction model attached to an allocation engine). Each FR is atomic, testable, ticketable.
8. **§11 Audit, Compliance, and Reporting.** Required if any state-mutating operation exists. Reference vertical-appropriate frameworks (ARINC 781/791, ITU/IMO, NERC CIP).
9. **§12 Non-Functional Requirements.** All six categories in this order: Performance, Scalability, Availability, Security, Data Integrity, Usability. Every entry quantified.
10. **§13 Corner Case Handling.** Critical. Number `C-N`. Format strictly:
    > **C-N: [title]**
    > **Condition:** …
    > **Behaviour:** …

    Aim for ≥10 entries on non-trivial PRDs. Walk every external integration, state transition, race, timeout, partial-failure mode, offline scenario, version mismatch, and concurrent-user case. If you cannot list 10, you have not thought enough — return to §5 and §6 and find the gaps.
11. **§14 Migration Path** (if replacing existing). Three phases minimum: Phase 1 Import, Phase 2 Parallel, Phase 3 Deprecation.
12. **§15 Appendix: Data Model.** Entity table with key fields and relationships.
13. **Final sweep against the readiness gates in CLAUDE.md.** Self-check before declaring done.

## Output shape

A single Google-Docs-ready markdown document with:
- Title block: `Product Requirements Document (PRD) — [System Name]` + one-line subtitle + companion-doc reference if applicable.
- Sections numbered as above (1 through 15+).
- British spelling throughout ("Behaviour", "centre", "organisation").
- Em-dashes in Connextra user stories.
- Tables for terminology, lifecycle states, policy fields, API endpoints, data model.
- Numeric concretisation in every FR and NFR.
- No marketing language. No "leverage", "synergy", "best-in-class", "robust", "seamless", "world-class".

## Quality checks before declaring done

The PRD is **not** done unless every gate passes. Walk these in order; if any fails, fix and re-walk.

1. **Exec-readable §1.** Can a non-engineer read §1.1, §1.2, §1.3 end-to-end and explain what's being built? If §1 uses VRF/BGP/IPAM without a one-line gloss, rewrite §1.
2. **Identifier integrity.** Every `US-N`, `FR-N`, `C-N` unique within the doc. No gaps in numbering runs. Sub-identifiers (`FR-7a`) only for genuinely coupled items.
3. **Companion linkage.** Every cross-doc reference includes version pin. If this PRD depends on a schema owned by another, an `FR-Xa: Schema Dependency` sub-FR exists.
4. **Corner-case coverage.** §13 has ≥10 entries on non-trivial PRDs. Each external integration, state transition, race, timeout, partial-failure mode, offline mode, and version-mismatch case covered. **This is the single largest historical defect source — do not declare done if §13 looks thin.**
5. **NFR completeness.** All six NFR categories present in §12. No category dropped silently.
6. **Quantified NFRs.** No "fast", "reliable", "scalable", "robust". Every adjective replaced by a number with an instrument (p95 latency, capacity, SLA %, record count).

If any gate fails, return to the relevant step. Do not hand the user a PRD that fails its own readiness gates.

## Common failure modes to avoid

- **Field-by-field translation of Confluence input.** The skill *restructures* notes into the spine; it does not transliterate.
- **Thin corner cases.** Three-to-five Cs is the #1 failure mode. Force ≥10 on non-trivial PRDs.
- **Vague NFRs.** "The system should be fast" is a defect, not a requirement.
- **American spelling.** "Behavior" anywhere is a defect.
- **Feature-list user stories.** "User can upload CSV" is not a user story.
- **Missing version pin on companion docs.** "See the Template PRD" is wrong; "Companion Document: Grid Template Attachment PRD v2.0" is right.
- **Solution before problem.** §1 that opens with "we will build…" instead of "today's workflow breaks because…" — re-order.
