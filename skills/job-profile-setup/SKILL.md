---
name: job-profile-setup
description: Build or update a job seeker's application profile, story bank, reason-for-leaving and cover-letter base — the single source of truth every later job-search step (scouting, kit prep, referrals, form filling) reads from. Use when someone starts the job-search workflow, says "set up my job profile", or needs to update salary, notice period, targets or stories. Works for any role or industry.
---

# Job profile setup

Every later stage of the job-search workflow reads from what this skill produces, so nothing is retyped per application. The goal is a profile that is **complete, true, and in the user's own voice**.

## What this skill produces

| Output | Purpose | Where it goes |
|---|---|---|
| `profile/candidate.yaml` | Machine-readable profile used by scripts and agents | Repo, **git-ignored** (personal data) |
| Application Profile page | Human-readable source of truth: standard answers, history, search rules, referral rules, answer rules, base resume | Notion page if Notion is connected; otherwise `profile/application-profile.md` (git-ignored) |
| Story Bank + Skills Map | Achievement stories and a skill → story → proof-point map used to tailor resumes and answers | Notion pages, or `profile/story-bank.md` |
| Reason for leaving | 1–3 versions by target company type, each with a written and a spoken form | Story Bank |
| Cover letter + about-me base | Templates with `{placeholders}` filled per role | Story Bank |

Templates for each are in `templates/`. The blank YAML schema is `profile/candidate.template.yaml` at the repo root.

## Ground rules

- **Facts only.** Never invent or inflate a metric, tool, title, employer, date or score. If a number isn't in the resume or stated by the user, ask or leave it out.
- **The user's draft first.** For anything written in their voice (reason for leaving, summaries, stories), ask for a rough version — bullets are fine — and refine it. Draft from scratch only if they ask.
- **One batch of questions.** Collect everything you can from existing material first, then ask only for what's missing, in one numbered list grouped by section. Don't drip questions one at a time.
- **Keep personal data private.** Phone, salary and scores go in the git-ignored files and in private pages, never in committed files. Never collect government ID numbers, bank or card details, or passwords — no application form step needs them from this profile.
- **Flag what could hurt them.** If an answer could backfire at background verification (e.g. stating current employment after a layoff, dates that don't match the resume), say so once, plainly, and record whatever they decide.

## Steps

### 1. Gather existing material
Before asking anything, look for:
- A resume (attached file, linked doc, or in a connected drive). Read it fully.
- Existing notes in connected apps (Notion, Google Drive) — search for "resume", "story bank", "profile", "cover letter", "interview".
- Anything already known from memory or past conversations in this project.

Extract: name, contact links, every role (company, title, start/end month-year, key achievements with numbers), education, skills and tools, awards.

### 2. Gap check
Compare what you have against this checklist and mark each item found, missing, or unclear:

1. **Contact and links:** email, phone with country code, current city, LinkedIn, portfolio / GitHub / personal site.
2. **Availability:** notice period, can join immediately (yes/no), earliest joining date, current employment status.
3. **Compensation:** current CTC with fixed/variable split, expected, hard floor, currency and unit (LPA, annual, monthly), whether to disclose current pay.
4. **Work authorisation:** countries authorised, whether sponsorship is needed elsewhere, open to relocation and where.
5. **History detail:** start and end month-year per role; total years and years in the target function (and from when they count it).
6. **Education detail:** degree, institute, year, score (CGPA or %) — many ATS forms require these.
7. **Targeting:** titles in priority order, lowest acceptable title, experience band to target, locations, work mode, company types or stages, preferred domains, deal-breakers, dream companies, max posting age (default 30 days).
8. **Documents:** master resume location, existing variants, base cover letter.
9. **Screening answers:** reason for leaving, why this function or career, anything they always want flagged for themselves (legal, references, diversity questions).
10. **Referrals:** networks to search in priority order (schools, past employers), tone, days to wait before applying directly (default 4).

### 3. Ask for what's missing
Send one numbered list, grouped under the headings above, showing what you already found so they can correct it. Keep questions short and offer the likely answer when you can infer it (e.g. "Total experience: 5+ years from May 2021 — right?").

Clarify these common ambiguities explicitly:
- **Notice period vs actual status** — if they've left or been laid off, ask how they want it answered and note the background-check risk once.
- **Years in function** — which role they count from.
- **Fixed vs total pay** — forms ask both.

### 4. Build the Story Bank
- Write 1 story per major achievement, 2–4 per recent role, in STAR form (situation, task, action, result) using only resume facts and what the user tells you. Give each a short code (e.g. `AGG`, `CAMP`).
- Build the **Skills Map**: a table per skill area for their function, each row `Skill | Story codes | Proof point (with numbers) | Companies or interviews that probe it`. Skill areas differ by function — for example:
  - Product: strategy, discovery, design, execution, technical depth, data and experimentation, AI, stakeholders, domain.
  - Engineering: system design, scaling, reliability, code quality, mentoring, delivery, domain.
  - Design: research, interaction, visual, systems, prototyping, collaboration.
  - Sales / customer roles: pipeline, negotiation, accounts, forecasting, enablement.
- Add a **behavioural map**: common "tell me about a time" prompts → best story and backup. Mark any prompt without a good story as "Not yet written" so the user knows what to prepare.
- Tools count only when a story shows them in use; note working-knowledge tools as such.

### 5. Reason for leaving
Ask for their rough version first. Then write up to three versions keyed to the company types they're targeting (e.g. AI-first, platform/data, consumer — or whatever split fits their search). Each version has:
- **Written** (60–110 words): delivered → what they learned they want → why this kind of company. No blame, no negativity about the previous employer.
- **Spoken** (~30 seconds).
Record which version maps to which role category so the prep step can choose automatically.

### 6. Cover letter and about-me base
From `templates/story-bank.md`:
- **About me — short** (~60 words) and **long** (~130 words).
- **Base cover letter** with `{Role}`, `{Company}`, a `{why this company}` line and a `{JD-match paragraph}` placeholder, plus 3–4 proof-point bullets from the Skills Map. Under 300 words.

Show these to the user for edits.

### 7. Write the outputs
1. Fill `profile/candidate.yaml` from the schema in `profile/candidate.template.yaml`. Leave unknowns as `null` with a `# TODO` comment, never guessed values.
2. Create the Application Profile from `templates/application-profile.md`:
   - **Notion connected:** create a page titled "Application Profile (source of truth for auto-apply)" where the user wants it (default: next to their Story Bank) and paste the base resume text into it.
   - **Otherwise:** write `profile/application-profile.md`.
3. Create or update the Story Bank pages (Skills Map, stories, Reason for leaving, Cover Letter & About Me) the same way.
4. Make sure `.gitignore` excludes `profile/candidate.yaml`, `profile/application-profile.md`, `profile/story-bank.md`, resumes and generated outputs.

### 8. Verify and hand over
- Re-read each output against the resume: every date, title and number must match exactly.
- Check the two places that most often disagree: employment dates (table vs resume text) and total years.
- Finish with: where each output lives (links or paths), and a short list of open items (TODOs, "Not yet written" stories, documents to upload).

## Updating an existing profile
When the user changes one thing (new salary floor, new target title, corrected date), edit it everywhere it appears — the YAML, the Application Profile page (table and resume text), and any story or template that quotes it — then confirm what changed in one line. Scheduled tasks pick up page changes on their next run.
