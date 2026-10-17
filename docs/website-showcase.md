# Website showcase

Visual evidence is pending ([screenshots/REQUIRED.md](../screenshots/REQUIRED.md)). This page lists what the reviewed code implements.

| Page | EN route | FA route | Notes |
| --- | --- | --- | --- |
| Home | `/` | `/fa` | Statement, numbered areas, highlights (editable) |
| About | `/about` | `/fa/about` | Biography paragraphs, "at a glance" facts, portrait slot |
| Experience | `/experience` | `/fa/experience` | Orchestras and education lists |
| Performances | `/performances` | `/fa/performances` | Numbered list, media archive links |
| Gallery | `/gallery` | `/fa/gallery` | Order-derived layout; alt text and credit per language |
| Tickets | `/tickets`, `/tickets/[slug]` | `/fa/tickets...` | External ticket links; revalidates every 60 s |
| Shop | `/shop`, `/shop/[slug]` | `/fa/shop...` | Listings only (no checkout); can be hidden |
| Contact | `/contact` | `/fa/contact` | Editable details and social links |

Cross-cutting: `lang`/`dir` per locale, hreflang, sitemap, robots, manifest, JSON-LD, skip link, reduced-motion CSS. No formal accessibility or performance audit was run.
