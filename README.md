# draft — Claude plugin

A plugin that connects Claude to [draft](https://jobsviadraft.com) so a user can tailor their résumé to a
job they saved in draft, written by their own Claude and checked by draft.

- **`.mcp.json`** — the draft connector (remote MCP, OAuth 2.1; the user signs in on draft's own consent
  screen and can disconnect in draft → Settings → Connected apps).
- **`skills/tailor-resume`** — how to use the tools and write the patch. The connector itself returns
  data only; all guidance lives here, static and reviewable.

## Tools the connector exposes

| tool | what it does | read-only |
|---|---|---|
| `draft_search_jobs` | searches the user's draft board (keyword, location, level, posting age); up to 10 jobs | yes |
| `draft_get_job` | one job's structured details and a short summary; the posting text only on request | yes |
| `draft_score_resume_for_job` | the base résumé's ATS score and matched / missing terms for one job | yes |
| `draft_get_resume_brief` | a signed, 30-minute brief: base résumé with ids, claims, matched keywords, patch schema; no contact details | yes |
| `draft_submit_resume_patch` | applies a patch, refuses anything the user never stated, scores and saves a new version | no (never deletes) |
| `draft_list_my_resumes` | the user's saved résumés, without content | yes |
| `draft_list_my_drafts` | the jobs the user queued in draft | yes |

No tool applies to a job or contacts anyone. Job-posting text is returned as data, capped, and only on request.

## Privacy

See draft's privacy policy: https://jobsviadraft.com/privacy
