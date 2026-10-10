# jobsviadraft — Claude plugin

Connects Claude to [draft](https://jobsviadraft.com), a workspace for job seekers. With it, Claude can find jobs
on your draft board, score your résumé against a job, and tailor your résumé for a job. draft then checks that
the tailored résumé adds nothing you never stated, and saves it to your account.

It is for people looking for work, preparing their own applications. It does not screen, rank or assess anyone.

## What is in this plugin

- **`.mcp.json`** — the draft connector, a remote MCP server at `https://jobsviadraft.com/mcp`. You sign in on
  draft's own page (OAuth 2.1); your draft password never goes to Claude.
- **`skills/tailor-resume`** — how Claude uses the tools and writes the rewrite. The connector itself returns
  data only; all guidance is in this Skill, static and readable here.

Nothing in this plugin runs on your computer, and it sends nothing anywhere except to the connector above.

## The tools, and whether each reads or writes

| Tool | What it does | Reads or writes |
|---|---|---|
| `draft_search_jobs` | Searches the jobs on your draft board by keyword, location, level and posting age; up to 10 | Reads |
| `draft_get_job` | One job's details and a short summary; the posting's text only when asked for | Reads |
| `draft_score_resume_for_job` | Your base résumé's ATS score against one job, with matched and missing terms | Reads |
| `draft_get_resume_brief` | What Claude needs to tailor your résumé for one job: your résumé's bullets (no contact details), your claimed skills, the job's matched keywords | Reads |
| `draft_submit_resume_patch` | Checks the rewritten bullets and saves the tailored résumé as a new version | Writes (never deletes or overwrites) |
| `draft_list_my_resumes` | Your saved résumés by job, score and date — not their content | Reads |
| `draft_list_my_drafts` | The jobs you queued in draft | Reads |

No tool applies to a job, contacts an employer or sends anything on your behalf.

## The check on every résumé

draft saves a tailored résumé only if it adds no skill, employer, title, date, years of experience or seniority
that your base résumé or your claimed skills do not already contain. A refused résumé is not saved, and Claude is
told each reason.

## What is stored, and for how long

- A saved résumé stays in your draft account like any other draft résumé.
- While checking a résumé, draft keeps the request's details, then empties them **one day** after the request is
  finished.
- Each request is recorded — which tool, which job, the outcome — never the résumé's text. The record is kept for
  **12 months** and you can see it in draft under Settings → Integrations → Connected apps.

## Who processes your content

Your own Claude processes what it reads and writes, **under your own Claude account and Anthropic's terms**.
draft calls no AI model for these résumés, and **does not use connector content to train any model.**

## AI-assisted applications

Résumés written this way are AI-assisted. **Some employers restrict AI-assisted applications — check theirs
before you apply.** Read every résumé before you send it.

## Disconnect

In draft: **Settings → Integrations → Connected apps → Disconnect** — the next request is refused at once. You
can also remove the connector in Claude's settings.

## Contacts

- Documentation: https://jobsviadraft.com/docs/claude
- Privacy policy: https://jobsviadraft.com/privacy
- Help: support@jobsviadraft.com · Privacy: privacy@jobsviadraft.com · Security: security@jobsviadraft.com

## Licence

MIT — see `LICENSE`.
