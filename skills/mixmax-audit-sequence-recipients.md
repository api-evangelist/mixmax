---
name: List sequences and page through their recipients
description: Enumerate the sequences you can access and page through each sequence's activated recipients.
api: openapi/mixmax-openapi.yml
operations: [listSequences, listSequenceRecipients]
---

# List sequences and page through their recipients

Use this skill to audit outreach: which sequences exist and who is enrolled.

## Auth
Send the API token as the `X-API-Token` header on every request.

## Steps
1. **List sequences** — call `listSequences` (`GET /sequences`). Filter with
   `name` (substring match) and `folder` (`shared`, `personal`, or a specific
   `folderId`); pass `expand=stages` to include stage detail. Each item has an `id`.
2. **Page recipients** — call `listSequenceRecipients`
   (`GET /sequences/{id}/recipients`) for a sequence. Only **activated**
   recipients are returned (drafts are excluded).

## Pagination note (important)
`listSequenceRecipients` does **not** use the standard `results`/`next` envelope.
It returns a **bare array**. Page with `limit` + `offset`: first page
`limit=50`, next `offset=50`, then `offset=100`, and so on. You are capped at
**10,000 total records**, so at `offset=9999` you may only request `limit=1`.

## Conventions
- Every other collection uses cursor pagination (`results`/`next`/`hasNext`).
- Rate limit is 120 requests / 60s per IP+user; back off on 429 via `X-RateLimit-Reset`.
- Errors are JSON `{ "message": ... }` with standard HTTP status codes.
