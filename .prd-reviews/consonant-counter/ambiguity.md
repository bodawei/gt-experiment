# Ambiguity Analysis

## Summary

The PRD is unusually clear for its size, and most of the potential ambiguities are either self-contained (resolved within the document) or explicitly called out in the Open Questions section. However, several key terms remain undefined or inconsistently used, and one structural contradiction exists between the "live feedback" goal and the use of `<input>` (single-line) vs `<textarea>` (multi-line) in the rough approach.

The most significant ambiguity is the definition of "consonant" — specifically whether 'y' counts — which the document explicitly raises but does not answer.

## Findings

### Critical Gaps / Questions

- **"y" is a consonant? — explicitly unresolved**
  The PRD raises this directly: "Should 'y' be counted as a consonant? (Common ambiguity — default: yes)" The parenthetical "default: yes" is not a decision — it's a tentative suggestion. The regex in the rough approach includes 'y'. If the decision is "yes," close the question. If "it depends on context," that's a different app.
  *Suggested question: Is 'y' always a consonant in this app, or should the user be able to configure it?*

- **`<input>` vs `<textarea>` — contradiction in rough approach**
  The rough approach mentions "an `<input>` or `<textarea>`" without resolving which. These are meaningfully different: `<input type="text">` is single-line; `<textarea>` supports multi-line. User Story 2 says "User pastes a paragraph" — a paragraph implies multi-line, which strongly implies `<textarea>`. But User Story 1 says "types 'Hello World'" which is a short string. The choice affects the UI layout and the UX meaningfully.
  *Suggested question: Should the input accept multi-line text (textarea) or single-line text (input)?*

### Important Considerations

- **"Instantaneous" is vague**
  The goal "Response is instantaneous (client-side computation)" uses a relative term. For practical purposes this is probably fine, but "instantaneous" to a user could mean <16ms (one frame), <100ms (perceptually immediate), or <1 second (fast). The parenthetical "client-side computation" is offered as justification, not as a threshold.

- **"Accessible: works with keyboard only" — what does this mean exactly?**
  Does this mean the user can tab to the input and type? Or does it mean full WCAG 2.1 keyboard navigation compliance? "Works with keyboard only" could mean different things to a developer vs a QA engineer vs an accessibility auditor.

- **"Screen-reader friendly label on input" — defined by whose standard?**
  An `<input>` with an associated `<label>` is the minimum. But screen-reader friendliness also encompasses ARIA roles, live region announcements (so the count updates are announced), and focus management. The PRD implies "put a label tag on the input" but may mean something more.

- **"Minimal readable styling" — subjective**
  The rough approach says "CSS for minimal readable styling." This is entirely subjective. Is there a design reference? A color palette? Should it match an existing product's look?

- **"No build step required" vs "No external dependencies / CDN required"**
  These two constraints are consistent but stated separately. It's worth confirming they're both absolute constraints, not preferences. If a CDN-hosted library were allowed, would the app still be acceptable?

### Observations

- The use of "should" vs "must" is inconsistent throughout. Goals say "must run entirely in the browser" but constraints say "No build step required" without "must." These are likely both hard requirements but the language doesn't make it clear.
- "Single-file deliverable preferred" — "preferred" is explicitly soft. The implementer could use 2–3 files and not violate any stated requirement.
- The example in User Story 1 ("Hello World" → 7) is a useful disambiguation anchor. H=consonant, l=consonant, W=consonant, r=consonant, ld=consonants: H-l-l-W-r-l-d = 7. This implicitly confirms 'h' is a consonant, double-l counts twice, etc. — good.

## Confidence Assessment

**Medium.** The PRD's own Open Questions section is admirably honest about what's unresolved. The `<input>` vs `<textarea>` question and the 'y' status are genuine ambiguities that two engineers would implement differently. The accessibility language is vague enough to cause a PR debate. The rest of the document is unusually unambiguous for a PRD at this stage.
