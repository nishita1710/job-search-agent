---
name: job-scout
description: Nightly job scout — searches job boards, company careers feeds, LinkedIn and job alerts for roles that fit the user's Application Profile, scores them, writes new leads to the Job Leads database and emails a digest. Use when running the scheduled scout, or when the user says "scout jobs", "find new roles" or "run the job search". Works for any role or industry.
---

# Job scout

Finds fresh roles that match the user's profile, scores them, and writes the good ones to the **Job Leads** database. It reads; it never applies, messages, signs in or changes anything on a job site.

Scheduled runs are unattended: make reasonable choices, never stop to ask a question, and finish the run.

## 0. Read the profile first
Read the user's **Application Profile** (see `skills/job-profile-setup`). It holds target titles, experience band, locations, salary floor, company tiers, charters (domains they're strongest in), and the search settings below. Everything in this skill is driven by that page; if anything here conflicts with it, the profile wins.

## 1. What counts as a relevant role
Build these filters from the profile:
- **Titles:** include the target titles and close variants; exclude titles below the lowest acceptable level, leadership titles above the target band, and adjacent-but-different roles (for a product manager, e.g. product analyst, product marketing, product owner, project/program manager). Drop internships.
- **Experience:** keep roles whose required range overlaps the target band ±1 year; drop anything that clearly requires more than the band's upper limit + 2 years.
- **Location:** home city and remote-in-country first; other cities in the country lower; abroad only if the profile allows relocation, with "visa sponsorship needed" noted when the profile says so. "Remote (US)" or similar country-restricted remote roles count as that country, not as open remote.
- **Posting age:** keep roles first posted within the profile's max age (default 30 days). Reposts older than that by their original date are dropped. Unknown date: keep only if the page still accepts applications.
- **Unnamed employers** ("hiring for a leading…"): keep only strong fits, and flag them.

## 2. Where to look
Use free sources first. Never get around a site that refuses access (no curl, raw HTTP scripts, mirrors or caches). If a source fails, skip it and note it in the digest.

### A. Paid scraping (optional, strict budget)
If the user has a Firecrawl (or similar) plan, use it only for the few listing pages the profile names under `search.firecrawl_pages` — typically the boards that block free fetching but serve good public listings (e.g. Naukri, Instahyre).
- Plain markdown only (`formats: ["markdown"]`, `onlyMainContent: true`, `maxAge: 0`). No AI-extraction formats, structured-data capabilities or paid search — they cost several times more.
- Hard cap: `search.firecrawl_max_calls_per_run` (default 4). Sum `creditsUsed` and report it. If one call costs more than 2 credits, stop using it for the run.
- Parse the markdown yourself: title, company, location, experience, salary, posted date, link.

### B. Free feeds and search (WebFetch / WebSearch)
- **Company ATS feeds** for target companies (rotate about a third per night):
  - Greenhouse `https://boards-api.greenhouse.io/v1/boards/<token>/jobs`
  - Lever `https://api.lever.co/v0/postings/<company>?mode=json`
  - Ashby `https://api.ashbyhq.com/posting-api/job-board/<company>`
  - SmartRecruiters `https://api.smartrecruiters.com/v1/companies/<company>/postings?q=<title>`
  Skip a slug quietly if it returns 404.
- **Big-company career sites** with public search (e.g. `amazon.jobs/en/search.json?base_query=<title>&loc_query=<country>&sort=recent`), or `site:` web searches for others.
- **Y Combinator jobs:** `https://www.ycombinator.com/jobs/role/<role-slug>` via WebFetch. Its location filters don't work — filter locations yourself.
- **Wider-net portals via `site:` search**, one query each, from `search.site_search_portals` (e.g. cutshort.io, hirist.tech, foundit.in, in.indeed.com, shine.com, timesjobs.com, glassdoor.co.in). Search results can be days old, so:
  1. Keep only single job postings that pass the title filter (skip "1,200 jobs" category pages).
  2. Verify each with WebFetch; drop it if it's gone, closed, filled or older than the max age.
  3. If the portal refuses the fetch, look for the same role on the company's own careers page. Use that link if found; otherwise keep it only if the snippet shows a recent date, and mark it "unverified — portal blocked".

### C. The user's own browser (optional, read-only)
If the user has a browser connected (e.g. Claude's built-in browser) and is signed in, use it for boards that need a real browser or sign-in, listed under `search.browser_pages`. Rules:
- One tab, a pause of about 6–8 seconds after each load, read page text only.
- Never click Apply / Easy Apply / Save / Follow / Join / Message, fill forms, sign in or out, or change settings.
- On any sign-in wall, CAPTCHA, security check or "unusual activity" warning, stop that site for the run and note it. Never try to get past it.
- **LinkedIn:** cap at `search.linkedin_max_searches` (default 5) first-page searches a night, past 24 hours (`f_TPR=r86400`). LinkedIn's AI search ignores the text `location` parameter — use `geoId` (e.g. Bengaluru 105214831, India 102713980, UAE 104305776; look others up once and store them in the profile). Check the results header shows the right place. The result cards don't expose job links: find each kept role on the company's careers page, or fall back to a LinkedIn search link for that exact title and company, flagged as such. Never open individual LinkedIn job pages or page through results.
- **Community channels** the user follows (e.g. a WhatsApp job channel): read the last ~2 days only, never post or react.
If the browser is unreachable, skip section C and say so.

### D. Job-alert emails (backup)
If Gmail is connected, search the last 2 days of alerts from the job sites in the profile, plus recruiter outreach. Use them to catch roles the direct searches missed (LinkedIn alerts carry direct job links). Never modify, label or archive email.

## 3. Remove duplicates
Check the Job Leads and Applications databases. Skip a role if the same link, or the same company with a near-identical title, already exists. Merge duplicates within tonight's findings, keeping the company's own careers/ATS link where you have one.

## 4. Score each role (1–10)
Start at 5, then adjust using the profile:
- +2 strong charter (domain) match
- +1 top-tier target company, or second-tier with a senior title
- +1 preferred location
- +1 experience range sits inside the target band
- +1 pay shown at or above the floor (or the level clearly implies it)
- −2 weak charter
- −1 secondary location
- −1 unnamed employer
- −1 pay shown or implied below the floor
- −2 deep domain expertise the user lacks
Cap at 10. Keep 6+. Treat placeholder salaries (e.g. "₹1") as not disclosed.

## 5. Write to Job Leads
For each kept role, create a row with: Role, Company, Fit score, Status = New, Tier, Company type, Segment (company stage or "Abroad"), Charter, Location, Experience asked, Comp (or "Not disclosed"), Source, Link, Why it fits (1–2 lines tied to a specific part of the user's background), Gaps / risks (one line, including the portal name for "Web" sources and any flags above), Posted, Found on (today). Leave Prep, Resume and Cover letter empty for the prep step. The database schema is in `docs/notion-setup.md`.

## 6. Send the digest
If email is connected, send the user one short email: "Job Scout — <date> — <N> new roles", grouped by segment, highest score first, each with score, role, company, location, comp, why-it-fits phrase, source and link. Add one line per source with found/kept counts, any skipped sources with the reason, and paid-scraper credits used. If nothing qualified, still send a short note with the counts.

Finish with a 2–3 line summary of sources checked and skipped, credits used, roles screened, roles added, and the top 3.
