# Scheduling the nightly runs

The scout and prep steps run on their own as two **scheduled tasks** in Claude. Ask Claude:

> Set up two scheduled tasks: run the job-scout skill every night at 12:00 AM, and run the application-prep skill every night at 12:50 AM.

Run prep about 50 minutes after the scout so it picks up that night's leads.

## Prompts to use
Each run starts fresh with no memory of earlier conversations, so the prompt has to say everything:

**Job scout**
> Run the job-scout skill. This is an unattended nightly run: make reasonable choices, never stop to ask a question, and finish the run. Read my Application Profile first; it's the source of truth.

**Application prep**
> Run the application-prep skill. This is an unattended nightly run: make reasonable choices, never stop to ask a question, and finish the run. Do not use any paid scraping tool in this run.

## What each run needs
| Run | Connectors | Needs your computer awake? |
|---|---|---|
| Job scout | Notion, Gmail (optional), Firecrawl (optional) | Only for browser sources (LinkedIn, Wellfound, IIMJobs, community channels). Without it, the scout skips those and says so. |
| Application prep | Notion, Google Drive, Gmail (optional) | No |

## Sign-ins
- **Searching** needs sign-in only for LinkedIn and any community channels, in the browser Claude uses (e.g. the Claude desktop app's built-in browser, which keeps its own sign-ins separate from Chrome).
- **Applying** needs you signed in to each portal you apply through. Claude never enters passwords; it stops at a sign-in page and asks you.

## Costs
- Free: web search, page fetches, public ATS feeds, YC jobs, your browser.
- Paid scraping (e.g. Firecrawl) is optional. With plain-markdown fetches of 4 listing pages a night, expect roughly 120 credits a month; the digest reports what each night used.
