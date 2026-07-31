---
name: Search and read Mixmax contacts
description: Find contacts across Mixmax, Google directory, and Salesforce, then read a specific contact record.
api: openapi/mixmax-openapi.yml
operations: [queryContacts, listContacts, getContact]
---

# Search and read Mixmax contacts

Use this skill to locate a person and pull their contact record from Mixmax.

## Auth
Send the API token on every request as the `X-API-Token` header (or the
`apiToken` query parameter). Create the token under Settings > Integrations; it
is shown only once.

## Steps
1. **Search broadly** — call `queryContacts` (`POST /contacts/query`) with the
   search terms. This searches across Mixmax, Google directory, and Salesforce
   (contacts, leads, accounts, opportunities), so it is the widest net.
2. **Or list Mixmax-only contacts** — call `listContacts` (`GET /contacts`) with
   `search`, `sort`, and `expand` (e.g. `expand=notes,groups`) when you only want
   people already in Mixmax.
3. **Read one** — call `getContact` (`GET /contacts/{id}`) with the `_id` returned
   from the search.

## Conventions
- Collections are cursor-paginated: read `results` and follow the `next` cursor
  while `hasNext` is true (default `limit` 50).
- `contacts` endpoints are **deprecated** — prefer `queryContacts` for new work
  and expect Mixmax to steer you toward newer surfaces.
- Respect the rate limit: 120 requests / 60s per IP+user; back off on HTTP 429
  using the `X-RateLimit-Reset` header.
- Errors return plain JSON `{ "message": ... }` with a standard HTTP status.
