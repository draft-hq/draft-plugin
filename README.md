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
| `list_my_drafts` | the user's saved jobs | yes |
| `get_generation_brief` | a single-use brief: base résumé with ids, claims, matched keywords, patch schema; no contact details | yes |
| `submit_resume` | applies a patch, checks it adds nothing the user never stated, scores and saves it | no (never deletes) |
| `score_resume` | scores text against a job; saves nothing | yes |
| `get_my_resumes` | the user's saved résumés, without content | yes |

No tool applies to a job or contacts anyone.

## Privacy

See draft's privacy policy: https://jobsviadraft.com/privacy
