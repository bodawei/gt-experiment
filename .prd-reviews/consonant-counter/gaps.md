# Missing Requirements

## Summary

The Consonant Counter PRD is well-scoped for a minimal single-purpose utility and explicitly rules out many common gaps (backend, persistence, multi-language). However, several absent requirements are worth noting — some because they affect correctness (Unicode handling), some because they affect user experience (input size, copy-to-clipboard), and a few because they are standard web-app concerns that the PRD never addresses (browser compatibility, analytics, deployment target).

None of these gaps are likely to cause a production incident given the app's simplicity, but Unicode handling is a genuine correctness risk if the audience goes beyond ASCII text.

## Findings

### Critical Gaps / Questions

- **Unicode / non-ASCII input behavior is unspecified**
  The PRD says "English consonants only for v1" and non-goals list "multi-language / non-Latin alphabet support." But it doesn't specify what happens when a user pastes text containing accented characters (é, ñ, ü), emoji, or CJK characters. Should they be silently ignored (not counted)? Should the app display a warning? The regex `/[bcdfghjklmnpqrstvwxyz]/gi` handles this gracefully by simply not matching, but the user receives no signal that characters were skipped.
  *Suggested question: When non-ASCII characters are in the input, should the app silently ignore them, display a count of non-counted characters, or show a warning?*

- **No deployment or hosting target specified**
  The PRD says the app should "work in a web browser" and be a single HTML file, but says nothing about where it will be hosted. Static file server? GitHub Pages? Local filesystem (file:// protocol)? This affects whether certain browser APIs (e.g., clipboard write) will be available.
  *Suggested question: Where will this be hosted or run? Does it need to work from a local file:// URL?*

### Important Considerations

- **No copy-to-clipboard requirement (raised in Open Questions)**
  The PRD lists "Should it support copy-to-clipboard of the result?" as an open question but doesn't resolve it. This is a small but useful feature and is cheap to implement — but it's currently unspecified.

- **No breakdown / per-consonant frequency requirement (raised in Open Questions)**
  Similarly, "Should the app also display a breakdown (which consonants, how many of each)?" is open. If this is to be addressed in a v2, the architecture should be designed to accommodate it; if not, explicitly close it.

- **No favicon or page title requirement**
  Minor polish items that affect perceived quality are not mentioned. Not a blocker but should be noted if the app is to be "production ready."

- **No handling of extremely long inputs**
  What is the maximum input length the app should support? For client-side text processing, inputs of tens of thousands of characters are fine, but the PRD is silent on this.

- **No analytics or usage tracking requirement**
  This is explicitly out of scope by implication (no backend, no persistence), which is fine — but it's not explicitly stated.

- **No versioning or update strategy**
  Since there's no backend and no user accounts, this is moot in practice. But if the app will be served from a URL, there's no mention of cache-busting or how updates will reach users.

### Observations

- The explicit non-goals section does excellent work of preventing scope creep. The absence of auth, persistence, and backend requirements is intentional and well-documented.
- The "single-file deliverable preferred" constraint preemptively resolves many deployment and dependency gaps.
- Rate limiting, abuse prevention, and multi-tenancy are all correctly non-issues for this app given the client-side, no-backend design.
- Admin tooling, audit logging, and compliance requirements are out of scope by nature — no data is processed server-side.

## Confidence Assessment

**Medium-High.** The gap surface is small because the app is intentionally minimal. The most significant gap is Unicode/non-ASCII behavior, which is a correctness concern, not just a polish concern. Deployment target and clipboard support are worth resolving before implementation starts but are not blockers. Most standard "missing requirements" categories (auth, multi-tenancy, data migration, rate limiting) are correctly inapplicable here.
