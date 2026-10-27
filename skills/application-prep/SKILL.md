---
name: application-prep
description: Builds an application kit for each strong lead in the Job Leads database — tailored resume, cover letter, drafted form answers, reason-for-leaving choice and referral message drafts — from the user's Application Profile and Story Bank. Use when running the scheduled prep after the job scout, or when the user says "prep my top leads" or "make kits for these roles".
---

# Application prep

Turns strong leads into ready-to-use application kits. It prepares only: it never applies, submits, sends messages, signs in or creates accounts. Scheduled runs are unattended: make reasonable choices, never stop to ask, and finish.

## 0. Sources of truth
- **Application Profile** — standard answers, employment and education tables, answer-writing rules, referral rules and default referral messages, and the base resume text. If anything here conflicts with it, the profile wins.
- **Story Bank** — the Skills Map (skill → story → proof point), individual stories, the Reason for leaving versions, and the Cover letter & About me base.
- **Job Leads** database and a **kits folder** in the user's drive.
See `skills/job-profile-setup` for how these are built.

## 1. Pick leads
Leads with Fit score ≥ 7, Prep empty, Status New or Shortlisted. Highest score first, at most 8 per run. Mark leads below 7 with an empty Prep as "Skipped - below bar".

## 2. For each lead
1. **Read the job** with free page fetches only. Keep any paid scraper's credits for the scout.
   - If the link is a search link, sits behind a login, or won't load, find the same role on the company's own careers page or ATS and read that; if found, update the lead's Link.
   - Partial description only (e.g. from an alert email): build the kit anyway and set Prep = "Prepped - needs login".
   - Nothing readable: Prep = "Form unreadable", reason in Gaps / risks.
   - Hard mismatch (far above the experience band, a domain the user lacks entirely, pay clearly under the floor): Prep = "Skipped - below bar", reason in Gaps / risks.
2. **Map the JD:** its top 5–7 requirements, each matched to the best story code and proof point from the Skills Map. Note honest gaps.
3. **Tailored resume:** in the kits folder, create a subfolder `<Company> — <Role>` and a document `<Name>_Resume_<Company>`. Start from the base resume; rewrite the headline and summary for the role, lead with the most relevant bullets, and use the JD's vocabulary where it's true. Same facts only — never invent or inflate a metric, tool, title, employer or date. Dates come from the profile's employment table. One page of content.
4. **Cover letter** (when the form asks for one, or for top-tier / 8+ roles): `<Name>_Cover_Letter_<Company>` from the base template, every placeholder filled, under 300 words, facts from the profile and Story Bank only.
5. **Form answers:** draft every custom question found (or, if none were visible: why this company, why this role, a relevant achievement) following the profile's answer rules — first person, plain, 40–120 words for short fields, under 250 for essays, no clichés.
6. **Reason for leaving:** pick the version whose company type matches the role (the mapping is on the Reason for leaving page). Use the written version as is (trim only for a character limit) and add the spoken version for interview prep. If none fits, list it for the user.
7. **Referral drafts:** fill the default referral messages from the profile with role, company, job link and a one-line "why this role" from the JD. Leave the person's name and the shared connection as placeholders. Don't look anyone up or contact anyone.
8. **Write the kit** onto the lead's page under "Application kit":
   - Standard fields from the profile (contact, links, notice period, joining, compensation, work authorisation, experience).
   - Reason for leaving — which version and why, in a few words.
   - JD match — requirement → story → proof point, plus gaps.
   - Custom questions with drafted answers.
   - Needs <user> — anything the profile reserves for them, and anything uncertain.
   - Referral — networks to search at this company in the profile's priority order, then the drafted connection note and message.
9. **Update the lead:** Resume and Cover letter links, Prep = "Prepped" (or "Prepped - needs login"). Leave Status alone.

## 3. Report
If email is connected, send the user a short email: "Application kits ready — <date> — <N> roles", listing each prepped role (score, company, role, prep status, link) and anything skipped with the reason. End with: "Say 'apply to my prepped roles' to fill the forms; you review and submit."
