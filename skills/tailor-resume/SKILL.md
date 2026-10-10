---
name: tailor-resume
description: Tailor the user's résumé to one of their saved jobs in draft, using only facts already in their résumé, and save it to their draft account. Use when the user asks to tailor, rewrite or write a résumé for a job they have in draft.
---

# Tailor a résumé with draft

draft holds the user's base résumé, the skills they have claimed and the jobs they have saved. Its tools
give you the data; you write; draft checks what you wrote and saves it. draft makes no model call of its
own — the writing is yours, on the user's own account.

## The flow

1. **Find the job.** If the user named a draft job id, use it. Otherwise call `list_my_drafts` and pick
   the job they mean — ask if more than one fits.
2. **Get a brief.** Call `get_generation_brief` with the `job_id`. The brief holds:
   - `resume` — the base résumé, every bullet, skills row and summary with an `id`;
   - `claims` — skills and action verbs the user has said they have;
   - `keywords.matched` / `keywords.claimed` / `keywords.verbs` — the job's terms the résumé already
     supports;
   - `output_schema` — the exact shape of the patch;
   - `allow_summary`, `max_attempts`, `expires_at` (30 minutes).
3. **Write a patch, not a résumé.** Return only what changes, keyed by the brief's ids:
   - `bullets`: `{ id, text }` to rewrite one, `{ id, drop: true }` to remove one (keep at least one per
     entry);
   - `skills`: a row's existing values, reordered or trimmed — never a value the row does not have;
   - `skills_order`: every row id of a skills section, once each;
   - `summary`: only when `allow_summary` is true or the résumé already has one.
4. **Save it.** Call `submit_resume` with the `brief_id` and the patch.

## How to write it

- **Use only facts already in the résumé.** Emphasise, reorder and rephrase. Never add a skill, tool,
  employer, title, date, number, years of experience, seniority or achievement that is not already there.
- Prefer the terms in `keywords.matched` and `keywords.claimed` where they truthfully describe the work.
  A term that is in neither the résumé nor the claims is refused.
- Each bullet: one line, an action verb first, at most 400 characters, plain text (`**bold**` allowed).
- Never put contact details (email, phone, links) in a patch. draft writes the header itself.

## When draft refuses

A refusal saves nothing and lists each reason in plain words, with the term or bullet id involved.
Tell the user the reasons plainly, fix exactly those points, and submit again with the same `brief_id`.
A brief allows `max_attempts` attempts (3); after that, or after 30 minutes, get a new brief.

## Reporting the result

Say what was saved: the job, the base score → the tailored score, and the link to open it in draft.
If the tailored score is below the base, say so — the user may prefer the base résumé. If `score_resume`
or the result says a job could not be scored, report "could not be scored", never a poor fit. The résumé
was written with AI assistance: remind the user to read it before sending it to an employer.

## What draft will not do

There is no tool to apply to a job, contact an employer or send anything on the user's behalf.
