# LinkedIn identity: same-interface dual implementation with a hard go/no-go date

> **Status (2026-07-14): Go.** LinkedIn Developer Portal approval landed before the go/no-go date; `LinkedInLive` is the production path. `LinkedInSimulated` remains mandatory for the automated test suite, as decided below.

The Dual-Source Trust Gateway requires LinkedIn identity, but "Sign In with LinkedIn using OpenID Connect" access depends on LinkedIn Developer Portal approval — an external process outside our control, with the demo (M8) fixed on 3 August 2026. We decided the backend exposes one `IdentityProvider` interface with two implementations selected by environment variable: `LinkedInLive` (real OIDC) and `LinkedInSimulated` (an in-app consent screen carrying a visible "Simulated for demo" watermark, offering 3–5 preset professional profiles and returning OIDC-shaped data). The Flutter client is unaware which is active. Go/no-go: if approval has not landed by **24 July 2026**, we switch permanently to Simulated and do not revisit — no new external dependencies in the final week. `LinkedInSimulated` is built unconditionally regardless of approval, because the automated test suite (a separately graded deliverable) must run against the mock, never against live LinkedIn.

## Consequences

- The simulated path is honestly labelled in the UI, demo, and report; passing it off as live integration would be an academic-integrity risk.
- Trust Score consumes LinkedIn tenure identically from either implementation.
