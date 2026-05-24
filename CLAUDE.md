# CLAUDE.md — Product Documentation

> Project-level instructions for Claude when working in AB's PRD workspace. Always loaded. Skills (in `.claude/skills/`) handle specific tasks; this file describes the world they operate in.

## Project context

**What this is:** A working repository of product specifications for the Platform — a maritime/aviation/energy/commercial connectivity platform built on SDN, NFV, and SD-WAN principles, with deep integration into network grid, IPAM, and template-driven provisioning.

**Who reads PRDs (in this order of depth):**
- **Engineering leads** (CCIE/CCDE-grade networking, SDN/NFV): need functional requirements, NFRs, corner cases, data model. Skim §1, live in §4 onwards.
- **Design** (UX/IxD): live in §4 User Stories and any UI/UX-flavoured FRs.
- **Execs / customer-facing**: read §1.1 (Problem), §1.2 (Solution), §1.3 (Principles) only. If §1 doesn't stand alone, the PRD has failed exec-readability.

**Verticals served:** Maritime (primary), Aviation, Energy, Commercial. Every PRD declares the vertical(s) in §1 or §2.1.

## Stack & commands

PRDs live in **Google Docs**. Source notes live in **Confluence**. Companion specs are referenced by name + version (e.g., `Companion Document: Grid Template Attachment PRD v2.0`).

There's no `build/test/lint` in the usual sense, but every PRD has **readiness gates** — treat these as CI:

| Gate | Pass condition |
|---|---|
| Exec-readable | §1.1, §1.2, §1.3 readable end-to-end by a non-engineer |
| Identifier integrity | Every `US-N`, `FR-N`, `C-N` unique within the doc; no gaps in a run |
| Companion linkage | Cross-doc references include version pin (e.g., v2.0), not just title |
| Corner-case coverage | ≥10 `C-N` entries on any non-trivial PRD; each external integration, state transition, race, timeout, and partial-failure mode covered |
| NFR completeness | All six NFR categories present: Performance, Scalability, Availability, Security, Data Integrity, Usability |
| Quantified non-functionals | No "fast", "reliable", "scalable" without numbers (p95 latency, capacity, SLA) |

A PRD is ready to hand to Dev only when all six pass.

## Conventions

**Spine (required section order):**
1. Executive Summary — 1.1 Problem, 1.2 Solution, 1.3 Design Principles
2. Scope — 2.1 In Scope, 2.2 Out of Scope
3. Key Concepts and Definitions (terminology table)
4. User Stories (`US-N`, grouped by capability area)
5. High-Level Architecture (Layers/Tiers + End-to-End Workflow as numbered steps)
6–N. Functional Requirements (`FR-N`, grouped by module)
+1. Audit, Compliance, and Reporting
+2. Non-Functional Requirements
+3. Corner Case Handling (`C-N`)
+4. Migration Path (phased) — only when replacing an existing workflow
+5. Appendix: Data Model

**Identifiers:** `US-N` for user stories, `FR-N` for functional requirements (use `FR-Na`, `FR-Nb` for tightly-coupled sub-items, not decimals), `C-N` for corner cases.

**User Story format (strict Connextra, em-dash):**
> As a [role], I want [capability]—so that [benefit].

**Corner Case format (signature pattern):**
> **C-N: [descriptive title]**
> **Condition:** [what triggers this edge case]
> **Behaviour:** [what the system does — exact, not vague]

**Spelling:** British / Commonwealth — "Behaviour", "behaviour", "centre", "organisation", "favourite". Never "behavior" in a house-style PRD.

**Design Principles:** always present as a named, bulleted list in §1.3. Bookend convention.

**Tables** for: terminology, lifecycle states, policy fields, API endpoints, data model. Inline prose for narrative.

**Tone:** technical depth without apology below §1. Use VRF / CIDR / HSRP / BGP / RFC numbers directly — assume CCIE-tier readers from §4 onward. Every claim concretised with a numeric example, RFC anchor, or named pool.

**NFR categories (fixed set, in this order):** Performance, Scalability, Availability, Security, Data Integrity, Usability. Don't drop any; mark "N/A" with a one-line justification if irrelevant.

## Do's

1. **Lead §1 with the broken status quo, not the solution.** Name the manual workflow and its error classes (transcription, overlap, stale data, no lifecycle, scale ceiling) before §1.2 introduces the system.
2. **Enumerate corner cases exhaustively.** Past mistake: PRDs handed to dev with only the happy path. Every external integration, state transition, race, timeout, partial-failure mode, and offline scenario gets a `C-N`. Aim for ≥10 on any non-trivial PRD.
3. **Quantify every non-functional.** "<200ms for 100K+ prefixes", "<2s auto-allocation", "500K+ records", "WCAG 2.1 AA". No "fast" without a number.
4. **Pin companion docs by version.** Cross-PRD references include explicit version (`v2.0`). Add an `FR-Xa: Schema Dependency` sub-FR when this PRD relies on a contract owned by another.
5. **State migration explicitly.** If the PRD replaces an existing workflow (Confluence, spreadsheet, manual), include §14 with Phase 1 (import) / Phase 2 (parallel) / Phase 3 (deprecation).

## Don'ts

1. **Don't hand to dev without corner cases.** Single largest historical defect source. If §13 has fewer than 10 entries for a non-trivial PRD, the PRD is not ready.
2. **Don't write "fast / scalable / reliable / robust / seamless".** Replace with a number and an instrument (p95 latency, registry size, SLA %). Vague NFRs failed past reviews.
3. **Don't drop vertical context.** "This is for ships" isn't enough. Name the vertical(s) and call out where behaviour diverges (aviation = ARINC 781/791, energy = NERC CIP, maritime = ITU/IMO).
4. **Don't invent terminology.** Reuse industry terms (IPAM, VRF, prefix, aggregate, pool, route distinguisher) and prior-PRD glossary entries. New terms get a row in §3.
5. **Don't paste raw Confluence into the PRD.** Confluence is the *input*. PRDs restructure it into the spine. Field-by-field copy is a smell.
6. **Don't write user stories as feature descriptions.** "User can do X" is wrong. Connextra is the only acceptable form.

## External references

- **Confluence** — source of raw notes, site sheets, customer call transcripts. Always input, never output.
- **Google Docs** — PRD living-document home. Final output.
- **Jira / Linear** — where `FR-N`s become tickets after dev handoff. Each `FR` should be ticketable as-is (atomic, actionable, testable).
- **Companion PRDs** — e.g., Grid Template Attachment PRD, Smart Site Data Portal PRD. Pin by version when cross-referencing.
- **Compliance frameworks** — ARINC 781/791 (aviation), ITU/IMO (maritime), NERC CIP (energy). Reference in §11 when in vertical scope.

## Skill index

- `prd-drafter` — Draft a new PRD or convert Confluence notes into a house-style PRD. Triggers on "draft a PRD", "spec out X", "turn this Confluence into a PRD", and persona-prefixed asks ("As a senior network architect…").
