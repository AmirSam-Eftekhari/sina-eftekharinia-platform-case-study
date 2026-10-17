# Testing and validation

Run on a copy of the project snapshot in an isolated Linux sandbox; original files untouched.

| Check | Result |
| --- | --- |
| `npm ci` (web, mobile) | Succeeded |
| Web `tsc --noEmit` | Passed |
| Web `vitest run` | 10 files, **144 / 144 passed** (≈88 s) |
| Mobile `tsc --noEmit` | Passed |
| Mobile `vitest run` | **32 passed**, 1 skipped (live-server contract test needs a running server and activation code) |
| Web `next build` with `ALLOW_BUILD_WITHOUT_DATABASE=1` and a placeholder site URL | Succeeded; built-in content only |
| Mobile `expo export --platform android` | Succeeded (JavaScript/Hermes bundle) |

Web tests cover auth, events, products/inventory, schema, health parsers, translation, content, publishing/draft isolation, SEO/contact/media and date handling, using PGlite (embedded Postgres).

## Not performed
Native Android/EAS build, installation on a device or emulator, live Supabase/Vercel/Google Translate runs, the Node end-to-end scripts in `tests/e2e`, the contract test, browser testing, Lighthouse, accessibility audit, load testing.
