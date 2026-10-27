# Notion setup

The workflow uses two Notion databases and a few pages. Create them once; every skill reads and writes them by name.

## Pages
- **Application Profile** — from `skills/job-profile-setup/templates/application-profile.md`.
- **Story Bank** — with child pages: Story Bank Skills Map, one page per story, Reason for leaving, Cover Letter & About Me (Base). Template: `skills/job-profile-setup/templates/story-bank.md`.

## Job Leads database
One row per role the scout finds.

| Property | Type | Values / notes |
|---|---|---|
| Role | Title | Job title |
| Company | Text | |
| Fit score | Number | 1–10 |
| Status | Select | New, Shortlisted, Applied, Skipped |
| Prep | Select | Prepped, Prepped - needs login, Form unreadable, Skipped - below bar |
| Tier | Select | Tier 1 — dream, Tier 2 — strong, Tier 3 — volume |
| Company type | Select | Big tech / MNC, Listed tech, Series C+ startup, Other |
| Segment | Select | Global MNC / Big Tech, Listed tech, Late-stage private (Series D+), Growth-stage (Series B-C), Early-stage, Abroad |
| Charter | Select | Your domains, e.g. Payments / Fintech, Data / Platform, B2B SaaS, AI product, Growth, Other |
| Location | Text | |
| Experience asked | Text | |
| Comp | Text | As shown, or "Not disclosed" |
| Source | Select | LinkedIn, Naukri, Instahyre, Career page, Web, WhatsApp (edit to match your sources) |
| Link | URL | Prefer the company's own careers / ATS link |
| Why it fits | Text | 1–2 lines tied to your background |
| Gaps / risks | Text | One line: portal name, sponsorship, unverified, etc. |
| Posted | Date | |
| Found on | Date | |
| Resume | URL | Filled by application-prep |
| Cover letter | URL | Filled by application-prep |

The application kit itself is written into each lead's page body.

## Applications database
One row per application you actually submit.

| Property | Type | Values / notes |
|---|---|---|
| Company | Title | |
| Role | Text | |
| Link | URL | |
| Stage | Select | To apply, Applied, Recruiter screen, HM screen, Onsite / loop, Team match, Offer, Rejected, Withdrawn |
| Applied | Date | |
| Channel | Select | Referral, Portal, Recruiter inbound, Direct to HM |
| Referrer | Text | |
| Tier | Select | Same as Job Leads |
| Comp band | Text | |
| Charter | Text | |
| Next action | Text | |
| Next action date | Date | |
| Notes | Text | |

## Drive
A folder for application kits (one subfolder per company and role). The prep skill writes tailored resumes and cover letters there.
