---
name: Pull Mixmax meeting summaries and transcripts
description: Search Meeting Copilot summaries and retrieve the full transcript for a meeting.
api: openapi/mixmax-openapi.yml
operations: [searchMeetingSummaries, getMeetingTranscript]
---

# Pull Mixmax meeting summaries and transcripts

Use this skill to retrieve meeting intelligence captured by Mixmax Meeting Copilot.

## Prerequisite
The **`mixmaxApi` feature** must be enabled for your workspace (Admin Settings).
Without it these endpoints return 403. Workspace-wide transcript access lets
admins read other members' meetings only when explicitly enabled.

## Auth
Send the API token as the `X-API-Token` header on every request.

## Steps
1. **Find meetings** — call `searchMeetingSummaries`
   (`GET /meetings/summaries/search`) with optional filters. Each result carries
   an `_id` (the meeting summary id).
2. **Get the transcript** — call `getMeetingTranscript`
   (`GET /meetings/transcripts/{id}`), passing the summary `_id` as `{id}`. The
   response is the full transcript with speaker identification and timestamps.

## Conventions
- The `{id}` for the transcript is the `_id` from the summary search response —
  not a separate transcript id.
- Rate limit is 120 requests / 60s per IP+user; honor `X-RateLimit-Reset` on 429.
- Errors are JSON `{ "message": ... }` with standard HTTP status codes.
