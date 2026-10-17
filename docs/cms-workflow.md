# CMS workflow

1. **Open** a page or list in the app; first open seeds the database from the built-in content.
2. **Edit** English and Persian fields independently; optionally request a translation suggestion (review required; nothing is saved automatically).
3. **Save** writes a draft. The public site is unchanged.
4. **Preview** opens a signed link (30 minutes, `noindex`, uncached).
5. **Publish** (per language) copies drafts to published and triggers revalidation. **Discard** drops the draft; a **restore** action also exists for pages (its exact semantics were not verified).
6. Reordering and deleting entries apply immediately. Numbering updates automatically.
7. Sitemap `lastModified` follows publish timestamps; only published gallery photos appear in it.

Visibility rule: an entry shows in a language only if its main field is filled in that language.
