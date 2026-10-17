# API and data flow

## Device activation
1. Operator generates a code server-side (script for the first device; later from an active device).
2. App sends `POST /api/admin/session/activate` with the code and device info; server checks throttling, locks and consumes the code in a transaction, stores a hashed token and returns the token once.
3. App stores the token with secure storage and sends `Authorization: Bearer <token>` on later calls; every call is verified and slides expiry.

## Endpoint groups (`/api/admin`)
session, sessions, activation-codes, dashboard, activity, events, products (+inventory, images), shop, content (overview, preview, pages, collections, images), translate, health.

## Public data
Server components read published data directly from Postgres. JSON endpoints `/api/public/{events,products,shop}` (60 s cache) and `/api/health` exist; `/api/cron/health` requires `Authorization: Bearer $CRON_SECRET`.

## Error contract
JSON `{ error: { code, message, details? } }`; validation errors carry field issues the app maps to form fields; 401 returns the app to activation.
