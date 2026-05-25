# PRD Review: Consonant Counter Web App

## Executive Summary

The Consonant Counter PRD is in good shape for a small, well-scoped utility. The core requirement is clear and technically trivial, the non-goals section is unusually strong, and there are no architectural or feasibility risks. The primary issue is that the PRD's own Open Questions section contains four unresolved items — two of which (`y` as consonant, `<input>` vs `<textarea>`) create implementation ambiguity, and two of which (breakdown feature, copy-to-clipboard) are unresolved scope decisions. Answering these four questions is the only prerequisite before implementation can start confidently.

---

## Before You Build: Critical Questions

### Scope / Feature Definition

**Q1: Is per-consonant frequency breakdown (e.g., "b×2, d×1, ...") in scope for v1?**
- Why this matters: If yes, the display area changes significantly (frequency table vs single number). If no, the implementer should know not to design the UI around it.
- Found by: scope, requirements
- Suggested answer options: (a) Out of scope for v1, explicitly deferred; (b) In scope — show both total count and per-letter breakdown; (c) In scope as an optional toggle

**Q2: Is copy-to-clipboard for the result count in scope for v1?**
- Why this matters: The result display area becomes interactive (needs a button, clipboard API, and feedback state). Deployment target also affects whether the clipboard API is available (requires HTTPS or localhost; won't work from `file://`).
- Found by: scope, gaps
- Suggested answer options: (a) Out of scope for v1; (b) In scope — add copy button; (c) In scope only if hosted on HTTPS

### Requirements / Ambiguity

**Q3: Is 'y' a consonant in this app? (Yes/No — commit to a definitive answer)**
- Why this matters: The regex in the rough approach includes 'y', but the PRD marks this as unresolved. Two implementers would produce different results. The regex changes from `[bcdfghjklmnpqrstvwxyz]` (y=yes) to `[bcdfghjklmnpqrstvwxz]` (y=no).
- Found by: requirements, ambiguity (also raised in PRD's own Open Questions)
- Suggested answer: "Yes, 'y' is always a consonant in this app." (This is the standard English-language convention and consistent with the rough approach.)

**Q4: Should the input field be multi-line (textarea) or single-line (input)?**
- Why this matters: User Story 2 says "User pastes a paragraph" which implies multi-line. `<input type="text">` is single-line and would force paragraphs onto one line. `<textarea>` allows multi-line. The rough approach says "an `<input>` or `<textarea>`" without deciding.
- Found by: ambiguity
- Suggested answer: `<textarea>` — User Story 2 explicitly describes pasting paragraphs.

---

## Important But Non-Blocking

**Unicode / non-ASCII character behavior**
The regex `/[bcdfghjklmnpqrstvwxyz]/gi` correctly ignores non-ASCII characters (é, ñ, emoji, CJK). Users get no signal that characters were skipped. For v1 this is probably fine, but a note in the UI ("counts English consonants only") could prevent confusion.
- Found by: gaps, feasibility

**Browser compatibility not specified**
"Works in a web browser" is unspecified. The required JS APIs (`String.prototype.match`, `addEventListener`) work in all browsers IE9+, so this is low-risk. Recommend adding "modern browsers (Chrome, Firefox, Safari, Edge current)" as a soft constraint.
- Found by: requirements, gaps

**ARIA live region for screen-reader users**
The accessibility constraint says "screen-reader friendly label on input" but doesn't specify that the live count should be announced as it updates. Without `aria-live="polite"` on the result element, keyboard-only screen-reader users can tab to the input but won't hear the count update.
- Found by: stakeholders, requirements

**Mobile responsiveness**
The PRD says "web only" but doesn't mention responsive layout. If mobile web users are in the target audience, minimal responsive CSS should be added to the constraint. If desktop-only, make that explicit.
- Found by: stakeholders

**Deployment/hosting target**
Whether the app runs from `file://`, localhost, or HTTPS affects clipboard API availability and certain browser security policies. Recommend adding one line: "Target: static hosting on HTTPS (e.g., GitHub Pages)."
- Found by: gaps

---

## Observations and Suggestions

- The user story for "Hello World" → 7 is an excellent, concrete acceptance anchor. Consider adding 2–3 more to the PRD as a mini test table: e.g., `""` → 0, `"aeiou"` → 0, `"bcdfg"` → 5, `"Hello World 123!"` → 7.
- "Single-file deliverable preferred" is a soft preference. Consider hardening it to a requirement if delivery simplicity is important, or dropping the preference if multi-file is acceptable.
- The PRD's Non-Goals section is the strongest part of the document — it is specific, well-reasoned, and prevents scope creep. This is a model for how non-goals should be written.
- Feasibility is high. The app is technically trivial. No risks from the implementation side.
- The 'y' question is easily resolved: English grammar conventions treat 'y' as a consonant. The rough approach already assumes yes. Just make it explicit.

---

## Confidence Assessment

| Dimension | Score | Notes |
|-----------|-------|-------|
| Requirements completeness | M | Missing: acceptance test table, browser target, accessibility acceptance criteria |
| Technical feasibility | H | No hard problems; client-side regex counting is solved |
| Scope clarity | M | Two unresolved Open Questions affect scope (breakdown, clipboard) |
| Ambiguity level | M | Two ambiguities must be resolved before build (`<input>` vs `<textarea>`, 'y' status) |
| Overall readiness | M | Ready to build after answering 4 questions above |

---

## Next Steps

- [ ] Human answers the 4 critical questions above (Q1–Q4)
- [ ] PRD updated with definitive answers (close the Open Questions section)
- [ ] Pour `design` convoy to generate implementation plan once PRD is finalized
