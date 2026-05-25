# Technical Feasibility

## Summary

The Consonant Counter is technically straightforward. The core requirement — counting consonants in a string client-side with live feedback — is a solved problem and requires no external dependencies, server infrastructure, or complex state management. The app is well within the reach of a single HTML file with inline CSS and JavaScript.

There are no hard technical problems here. The main risks are implementation-level decisions (regex correctness, Unicode handling, live-update debouncing for large inputs) rather than architectural or systemic feasibility issues. The app is highly buildable as described.

## Findings

### Critical Gaps / Questions

- **Regex correctness for case-insensitive matching**
  The PRD proposes `/[bcdfghjklmnpqrstvwxyz]/gi` with the `i` flag. This correctly handles case-insensitive ASCII matching. However, the `g` flag is needed for `String.prototype.match()` to return all matches; without it, `match()` returns at most one result. This is a correct approach — just worth confirming the implementer understands the flags.
  *No question needed — this is implementation guidance, not a feasibility blocker.*

- **Unicode normalization edge case**
  If the input contains characters like "ñ" (U+00F1) or "ç" (U+00E7), the regex will correctly ignore them (they're not in the ASCII consonant set). However, some composed characters can be entered as combining sequences (e.g., n + combining tilde = ñ as two code points). The regex will still correctly skip both code points, so there's no correctness risk — but it's worth noting.
  *Suggested question: Is there any expectation that diacritically-modified consonants (e.g., "ñ" as a variant of "n") should be counted? If not, the current approach is correct.*

### Important Considerations

- **Live update performance on large inputs**
  The PRD requires live feedback (count updates on every keystroke). For typical text (hundreds to thousands of characters), running a regex match on every `input` event is negligible. For pathological inputs (pasting 1MB of text), this could cause a brief freeze. Debouncing the event handler (e.g., 50–100ms) would mitigate this — but the PRD doesn't mention it and it's not a requirement. This is an implementation quality concern, not a feasibility blocker.

- **Clipboard API availability**
  The Open Questions section asks about copy-to-clipboard. The modern `navigator.clipboard.writeText()` API requires a secure context (HTTPS or localhost). If the app is served from `file://`, clipboard writes will be silently blocked in some browsers. The legacy `document.execCommand('copy')` workaround is deprecated. This is a dependency on deployment context, not a hard feasibility issue.

- **No framework or build step = no tree-shaking or minification**
  The constraint "no build step required" means the delivered file will be hand-authored HTML/CSS/JS. For an app of this simplicity this is entirely appropriate. The file will be small regardless.

- **Browser compatibility**
  `String.prototype.match()` with a regex is universally supported. `<input oninput>` or `addEventListener('input', ...)` works in all modern browsers and IE9+. No feasibility concerns here.

- **`<textarea>` resize behavior**
  If a `<textarea>` is used (which the rough approach leaves open), default browser resize handles may need to be suppressed via CSS (`resize: none`) depending on design intent. Not a feasibility issue but worth noting.

### Observations

- The proposed implementation (single HTML file, inline JS regex) is technically sound and appropriate for the problem.
- No prerequisites, no third-party APIs, no service dependencies. This is as low-risk as web apps get.
- The client-side constraint eliminates entire categories of feasibility risk: no database, no auth system, no API design, no rate limiting infrastructure needed.
- If 'y' is excluded as a consonant (contrary to the PRD's tentative default), the regex simply changes from `[bcdfghjklmnpqrstvwxyz]` to `[bcdfghjklmnpqrstvwxz]`. Zero implementation risk either way.

## Confidence Assessment

**High.** This app is technically trivial. The hard problems are definitional (what counts as a consonant) and UX (single-line vs multi-line input), not technical. No architectural unknowns, no system dependencies, no performance cliffs for the intended use case.
