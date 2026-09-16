# Import speedy-import-joy into this project

The repository is public and is itself a Lovable app: an AI video studio for YouTube-style channels (sign-in, Sources, Chat, Studio, Channels), backed by a database, media storage, and a scheduled publishing job.

## What gets rebuilt here

1. **Pages and app shell** — sign-in page, and the signed-in area: Sources (add YouTube channels/notes, analyse videos), Chat (AI ideas), Studio (scripts, scene editor, video production), Channels (auto-post settings). Same look, same navigation.
2. **Accounts** — email/password plus Google sign-in, with a profile row created automatically on sign-up.
3. **Database and storage** — the full schema (projects, sources, source videos, ideas, scripts, videos, channels, posts) with per-user access rules, plus a private `media` bucket for generated images/audio/video, each user limited to their own folder.
4. **AI features** — idea generation, scripts, images and voiceover through Lovable AI, unchanged.
5. **YouTube reading** — channel/video lookup and transcripts, which need no external key.
6. **Scheduled publishing** — the every-15-minutes job that posts due videos to configured channel webhooks, with its protected endpoint and token.

## Order of work

1. Turn on Lovable Cloud (database, accounts, storage, server code).
2. Copy the application code and dependencies from the repository.
3. Apply the database schema, access rules, storage bucket, and the scheduler job.
4. Turn on email and Google sign-in.
5. Verify: build, sign-up creates a profile, each page loads with no errors, one create/read/update/delete round trip in Studio.

## Not included

- **Existing data** — the repository ships no data export, so this starts empty. If you have a CSV or JSON export from the original app, send it after the preview and it will be imported.
- **Live scheduled posting** — the job calls the published address, so it only takes effect once this project is published.

## Technical notes

- Target stack matches the source: TanStack Start v1, React 19, Tailwind v4, shadcn components; server logic as server functions plus one public webhook route under `/api/public/hooks/scheduled-videos`.
- Supabase project ID/keys from the source `.env` are not reused; this project gets its own Cloud credentials.
- Lovable AI models are kept as-is (`openai/gpt-6-astra`, `lovable/image-fast`, `google/gemini-2.5-flash-tts`) via `LOVABLE_API_KEY`.
- No third-party API keys are required.
