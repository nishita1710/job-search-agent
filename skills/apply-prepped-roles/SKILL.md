---
name: apply-prepped-roles
description: Fills job application forms for prepped leads in the user's own browser from their application kits, uploads the tailored resume, and stops before Submit. Use when the user says "apply to my prepped roles" or similar.
---

# Apply to prepped roles

Turns application kits (built by `application-prep`) into filled-in forms in the user's browser.

**Hard rule: never click Submit, Apply or Send.** Fill the form, then stop and hand over; the user reviews and submits. Never create accounts, sign in, enter passwords or one-time codes, or solve CAPTCHAs — if one is needed, leave the tab at that step and tell the user.

## Sources of truth
- **Application Profile** — standard answers, employment and education tables, compensation, notice period, work authorisation, portfolio link, answer rules. If a kit and the profile differ, the profile wins.
- **Reason for leaving** page — the kit names which version was chosen.
- **Job Leads** and **Applications** databases; the **kits folder** in the user's drive.

## Steps

1. **List what's ready.** Leads with Prep "Prepped" or "Prepped - needs login" and Status not Applied or Skipped. Show a short numbered list (company, role, score, prep status, form type, referral in progress?) and ask which to do. Default: all "Prepped", highest score first. At most 5 per session so each gets a proper review.

2. **Referral check first.** If a referral request for that company is in progress and the wait in the profile (default 4 days) hasn't passed, ask whether to wait or apply now. Applying first can stop a later referral from being credited.

3. **For each role the user wants to apply to now:**
   1. Read the lead's Application kit and re-read the profile's standard answers.
   2. Export the tailored resume (and cover letter, if the form takes one) as PDF from the drive, named `<Name>_Resume_<Company>.pdf`, and put it in a folder on the user's computer (ask once which; suggest `~/Downloads/Job Applications`).
   3. Open the apply link in a new tab in the user's preferred browser.
   4. Fill every field from the kit and profile: contact details, links (public portfolio URL with https://), notice period and joining, current and expected compensation (with the profile's line for other currencies), work authorisation, employment and education details, total experience, reason for leaving (the chosen written version), how they heard about the role (the lead's real source), and each custom question with its drafted answer — verbatim unless a character limit forces a trim. Read the page back to confirm each value landed.
   5. File uploads: use the browser's upload tool, or the desktop file dialog if computer use is enabled; otherwise tell the user exactly where the file is and ask them to attach it.
   6. Leave every "Needs <user>" question blank and list them.
   7. Stop on the review or submit screen. Report: company and role, which tab, what's filled, what still needs the user (including uploads), and anything uncertain.

4. **After the user says it's submitted:** set the lead's Status to Applied, and add an Applications row — Company, Role, Link, Stage = Applied, Applied = today, Channel = Referral (with the referrer's name) or Portal, Tier, Comp band, Next action = "Follow up", Next action date = today + 7, and a note linking the lead. If they skip a role, set Status = Skipped.

## Answer rules
- Only facts from the profile, Story Bank and Skills Map. Never invent numbers, tools, titles or dates.
- For a question the kit doesn't cover, draft an answer from those sources, show it to the user before typing it, and add it to the kit.
- Legal and consent questions, references, diversity questions and any salary figure not in the profile are the user's to answer.

## When things go wrong
- Sign-in wall, one-time code or CAPTCHA: stop there, keep the tab open, tell the user, move to the next role.
- Page won't load or the role is closed: set Status = Skipped with a note, and tell the user.
- Browser unreachable: say so, and offer to continue later or hand over the kit to fill by hand.
