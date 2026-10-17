# Security and Privacy

Findings from reading the code; no external audit or penetration test was performed, and no security guarantee is claimed.

## Implemented controls
- **Authentication:** single-use 16-character activation codes (15-minute TTL, hash stored); random device tokens (hash stored); server-side verification on every admin request; revocation; 90-day sliding expiry; failed-activation throttling (8 per 15 minutes per hashed client address).
- **Authorization boundary:** the app never talks to the database or storage directly and contains no server secret.
- **Database:** row-level security enabled on application tables; public roles limited to reading published, visible rows; draft columns not readable by public roles.
- **Input handling:** Zod validation; images decoded, metadata stripped, re-encoded; storage bucket restricted to WebP and 4 MB.
- **Responses:** admin API responses use `no-store`; preview pages use `noindex` metadata, are uncached and disallowed in `robots.txt`; preview tokens are random, stored hashed, and valid for 30 minutes.
- **Logging hygiene:** tests assert that health output and error paths do not leak connection strings or keys.
- **Mobile:** Android `allowBackup` disabled; audio-recording and overlay permissions blocked in app config.

## Known limitations
- Without an optional CA certificate, the database TLS connection encrypts traffic but does **not** verify the server certificate (`rejectUnauthorized: false`). Supplying `DATABASE_CA_CERT` enables verification.
- Activation throttling relies on the `x-forwarded-for` header supplied by the hosting platform.
- Any activated device has full administrative rights; there are no roles.
- No rate limiting beyond activation is implemented in the application code.
- Not tested: behavior on a real device, a live Supabase project, or after secret rotation.

## Privacy
- The public site exposes the owner's chosen contact details, which are editable. They are **not reproduced** in this repository.
- Image metadata (EXIF/GPS) is stripped from uploads.
- No analytics or tracking code was identified in the reviewed source; this was a search of the manifests and components, not a network audit.

## What this repository omits
Source code, environment files, keys, connection strings, database dumps, real tokens, personal contact details and unredacted screenshots.