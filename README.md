# Job search agent

An end-to-end, human-in-the-loop job search for Claude. Every night it finds roles that fit you across 20+ sources, prepares a tailored application kit for the best ones, and in the morning fills the application forms for you — stopping before every step that should be yours.

It works for any role or industry: your profile and story bank decide what it searches for and how it writes.

## How it works

| # | Step | Who | When |
|---|---|---|---|
| 1 | Set up your profile | You + `job-profile-setup` | Once, then keep current |
| 2 | Scout new roles | `job-scout` (scheduled) | Nightly, 12:00 AM |
| 3 | Prep application kits | `application-prep` (scheduled) | Nightly, 12:50 AM |
| 4 | Review leads and kits | You | Morning |
| 5 | Find referrers | Planned | On demand |
| 6 | Approve each referral message | You | Before anything sends |
| 7 | Fill application forms | `apply-prepped-roles` | When you ask |
| 8 | Review and submit | You | Final click |
| 9 | Track and follow up | `apply-prepped-roles` | After you submit |

### Where the scout looks
- **Free public feeds:** company ATS boards (Greenhouse, Lever, Ashby, SmartRecruiters), big-company career sites, Y Combinator jobs.
- **Free web search, verified:** Cutshort, Hirist, Foundit, Indeed, Shine, TimesJobs, Glassdoor and any other portals you list.
- **Your own browser, read-only and capped:** LinkedIn (5 searches a night), Wellfound, IIMJobs, regional boards, community job channels.
- **Optional paid scraping** for a few listing pages that block free fetching, with a hard per-night budget.
- **Your job-alert emails** as a backup.

### What each kit contains
A tailored resume and cover letter in your drive; drafted answers to the form's questions; the right version of your reason for leaving; a requirement-by-requirement match to your stories; and referral messages ready to personalise.

## Repository layout

```
profile/
  candidate.template.yaml     # blank profile + search settings (commit this)
  candidate.yaml              # your filled copy (git-ignored)
skills/
  job-profile-setup/          # step 1
    SKILL.md
    templates/
      application-profile.md
      story-bank.md
  job-scout/SKILL.md          # step 2
  application-prep/SKILL.md   # step 3
  apply-prepped-roles/SKILL.md# steps 7–9
docs/
  notion-setup.md             # Job Leads and Applications database schemas
  scheduling.md               # setting up the nightly runs, sign-ins, costs
```

## Getting started
1. **Add the skills** in `skills/` to Claude.
2. **Connect** Notion and Google Drive (required), Gmail (recommended), and optionally Firecrawl.
3. **Create the Notion databases** described in `docs/notion-setup.md`.
4. **Build your profile:** say "set up my job profile" and attach your resume.
5. **Schedule the nightly runs** as described in `docs/scheduling.md`.
6. **Each morning:** read the digest, then say "apply to my prepped roles".

## Guardrails
- **Facts only.** Resumes, cover letters and answers use only what's in your profile and stories; nothing is invented or inflated.
- **You stay in control.** Claude never clicks Submit, never sends a message without your approval, never signs in, enters passwords or solves CAPTCHAs.
- **Respectful of sites.** Read-only browsing, capped LinkedIn searches, no getting around blocks, and it stops on any security check.
- **Private by default.** Your filled profile, generated pages, resumes and outputs are git-ignored. Never commit salary, phone numbers, scores or ID numbers.
