# Engineering Decisions and Challenges

Each entry is limited to what the code, its comments and its tests substantiate. "Options considered" is given only where the repository documents an alternative. No native Android build issues (for example ABI problems) are recorded because the reviewed snapshot contains no native Android project and no build logs.

## 1. Drafts must be unreachable from the public site
- **Context / problem:** Editors need to save work without it appearing publicly, in two languages independently.
- **Solution:** Published and draft content are separate columns; one row per language per entry. Public database roles have column-level `SELECT` grants that exclude draft columns. The public reader never selects them; only the signed preview path does.
- **Trade-off:** More columns and more migration complexity than a single `status` flag.
- **Verification:** Integration tests assert that public roles cannot read draft columns and cannot read unpublished entries (`site-publishing` suite, run against a real Postgres engine via PGlite).
- **Remaining limits:** Verified on PGlite, not on a live Supabase project.

## 2. Behavior when the database is unavailable
- **Problem:** A silent fallback to built-in content could publish stale or empty pages.
- **Solution:** A production build fails if the database cannot be read (explicit opt-in `ALLOW_BUILD_WITHOUT_DATABASE=1`); at runtime the last good cached page continues to be served; admin calls return a generic error without connection details.
- **Verification:** Tests cover the no-database, runtime-failure and opt-in cases and check that connection strings are not leaked. A production build with the opt-in flag completed in this review.
- **Remaining limits:** Behavior on Vercel under a real outage was not tested.

## 3. Bilingual content model
- **Problem:** English and Persian content must be editable, publishable and omittable independently, and list numbering must not drift.
- **Solution:** Per-language rows keyed by stable item ids; explicit `position`; numbers derived from order; an entry appears in a language only if its primary field is filled in that language; saved-empty optional fields are omitted.
- **Trade-off:** An entry can exist in one language only (the app flags this).
- **Verification:** `site-content` tests (37 passing).

## 4. Secret-free mobile authentication
- **Problem:** An APK cannot safely hold server credentials, and a password screen was not wanted.
- **Solution:** Single-use, 15-minute activation codes create revocable device sessions. Only hashes are stored server-side; the token is kept with `expo-secure-store`; the server re-verifies every request; failed activations are throttled per hashed client address.
- **Verification:** Auth tests (7 passing).
- **Remaining limits:** No external security review or penetration test. See [SECURITY-AND-PRIVACY.md](SECURITY-AND-PRIVACY.md).

## 5. Image handling under serverless limits
- **Problem:** Serverless request bodies are limited (~4.5 MB) and uploads are untrusted.
- **Solution:** The app compresses before upload; the server rejects over 4 MB, decodes the bytes (does not trust headers or filenames), strips metadata, bounds pixel count, resizes and re-encodes to WebP; the storage bucket independently restricts type and size.
- **Verification:** Event poster test covers validation, re-encoding, EXIF stripping, size cap, replace and delete.
- **Remaining limits:** An image uploaded and then abandoned before saving is not garbage-collected.

## 6. Mobile ↔ backend contract
- **Problem:** Keep the app's client and forms compatible with the API.
- **Solution:** Shared field limits and validation messages; a contract test that drives the app's real API client and form modules against a running server.
- **Verification:** The contract test is **skipped** unless `E2E_API_BASE_URL` and an activation code are provided, so it did not run in this review. Separate Node end-to-end scripts exist for the CMS and Shop modes and were not run.

## 7. Health monitoring of SEO and infrastructure
- **Solution:** Pure parsers (canonical, hreflang, JSON-LD, robots, sitemap, icons, manifest) tested against fixtures, plus runners for website, SEO and cloud groups; flags a database role that cannot bypass RLS, which would otherwise silently return empty pages.
- **Verification:** `health` suite (19 passing).

## 8. Translation as an adapter
- Provider interface with a Google Translate v2 adapter; protected names/URLs are wrapped so they survive translation; without a provider the API reports "not configured" and never invents text; results are suggestions requiring review.
- **Verification:** Request/response handling tested with mocked fetch. **Not verified against the live Google API.**
