# Builder Plaza — Ubiquitous Language

## Implementation tiers

- **Live Feature** — implemented with real logic and real data. Uses an actual external API or our own backend. Demoable end-to-end without hardcoded results.
- **Mocked Feature** — the full frontend UI is built, but the data behind it is hardcoded, simulated, or served from a stub. Exists because the external dependency (e.g. a partner-API approval process) costs application/approval time, not coding time. Every Mocked Feature is explicitly labelled as simulated in the report and demo.

## Core domain terms

- **Builder** — a user role: someone making a product and publishing its progress. Owns Project Cards, posts to the Growth Plaza and Maintainer Request Market. (Absorbs the research personas of indie builder, maintainer, agent-native builder.)
- **Collaborator** — a user role: someone looking to join or contribute to projects. Consumes discovery, plain-language credibility summaries, Match Reasons; initiates Controlled DMs.
- **Founder** — a user role: someone hiring or shortlisting talent (canonical name for "Recruiter / Founder / Project Owner"). Uses search, shortlists, and Trust Score detail.
- **Primary Role** — a per-user preference field (`builder | collaborator | founder`) chosen at onboarding and switchable in settings. It selects which navigation shell and home screen the app renders; it is *not* an identity or permission boundary. Identity is established solely by the Dual-Source Trust Gateway, and all data belongs to the same user_id regardless of the active role.

- **Project Card** — a builder-authored card representing one product-in-progress: description, stage, current needs. The primary unit of discovery in the plaza.
- **Intent Badge** — a first-class, visible field stating what a user is currently open to (e.g. "open to co-founder", "maintenance help wanted"). Distinct from profile bio.
- **Match Reason** — the human-readable explanation attached to every recommendation. No match is shown without one.
- **Trust Score** — composite credibility score from GitHub contribution signals, LinkedIn tenure, Stripe revenue badge (mocked), and in-platform peer review.
- **Dual-Source Trust Gateway** — the sign-up gate requiring GitHub + LinkedIn binding before a profile exists.
- **Controlled DM** — contact that can only be initiated through a structured collaboration request; raw contact details are never exposed.
- **Request Market** — the single board of Role Postings, browsable by Collaborators. Renamed from "Maintainer Request Market" when Founder team-role postings were unified into it.
- **Role Posting** — one posting on the Request Market; `posting_type ∈ {maintainer, team_role}`. A maintainer posting (by a Builder) describes a repository help-wanted role with an access tier; a team_role posting (by a Founder) is a structured recruitment card requiring stage, tech stack, and commitment period.
- **Verified Activity Timeline** — a profile tab rendering the user's stored, verified GitHub events chronologically. Contains no self-declared content; the raw material for Ownership Evidence.
- **Ownership Evidence** — a tab on a candidate's Trust Score page showing commit continuity and repository roles, derived entirely from verified GitHub data. Answers the Founder question "does this project really belong to this person?"
- **Growth Plaza** — the feed of auto-generated "product growth" updates derived from real code activity.