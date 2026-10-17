# Deployment overview

- **Hosting:** Vercel (Next.js, framework auto-detected); Supabase (Postgres + Storage). The live deployment was **not** inspected in this review.
- **Schema:** eight forward-only SQL migrations applied in order; back up before migrating.
- **Configuration:** public site URL (needed at build time), server-only database URL, storage URL and service key, cron secret, optional translation key. Values are never stored in the app or this repository.
- **Content changes** need no redeploy (ISR + on-demand revalidation).
- **Cron:** `vercel.json` registers a daily health check.
- **Android:** EAS profiles `preview` (APK) and `production` (AAB); the app embeds only the public API base URL. No EAS build was run here.
- **Documented risks:** database role without RLS bypass (pages look empty; health check flags it), missing site URL at build (wrong canonicals; flagged), ~4.5 MB request limit.
