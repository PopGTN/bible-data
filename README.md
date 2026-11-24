# bible-data

Data repository for the Basic Bible App.

''
https://raw.githubusercontent.com/PopGTN/bible-data/main/catalog/testing/ebible_list.json
''


This repo now separates:

- production-ready catalogs that are intended to drive in-app downloads
- testing/research catalogs that are useful for development but should not be
  treated as legally cleared distribution lists
- per-translation license notes extracted from the source files when possible

## Layout

```text
bible-data/
  catalog/
    translations.json
    production/
      bibles.json
    testing/
      ebible_list.json
    research/
      pending_review.json
  docs/
    repositories.md       (how repositories work / make your own)
    making-a-hymn-book.md (by hand or with ChatGPT/Claude)
    importing-hymn-books.md
  templates/
    hymn-book-template.json
    ai-hymn-book-prompt.md
  schemas/
    repo-index-v1.schema.json
    hymnal-v1.schema.json
  licenses/
    README.md
    hymnals/              (permission records per hymn book)
    bibles/
      asv.md
      kjv.md
      kjvcpb.md
      web.md
  English/
    ...
  BiblesDatabases/
  Hymn Books/
```

## Safety Rules

- `catalog/production/bibles.json` is the source of truth for content that you
  currently believe you can legally bundle or offer for download.
- `catalog/testing/ebible_list.json` is a testing snapshot and must not be
  treated as a cleared production catalog.
- `catalog/research/pending_review.json` is for sources you are investigating.
- If rights are unclear, do not mirror or distribute the content from this repo
  through the app. Prefer metadata-only listing, official-source links, or
  user-import flows until the rights are confirmed.

## Hymnbooks

Hymnbooks need extra care. Permission to include a hymnbook in the app is only
safe if that permission actually covers the included works and your intended
distribution method. A hymnbook can include:

- public-domain songs
- copyrighted songs
- copyrighted arrangements
- copyrighted translations
- compilation or editorial rights

If the permission is only for the book as a compilation, that may still not
clear every song for app distribution. Track rights per hymnbook and, when
needed, per song.

- `catalog/production/hymnals.json` is the **official hymnal repository** the
  app downloads hymn books from. A book is listed only once its rights are
  recorded in `licenses/hymnals/`. (The Believers Hymn Book and The New
  Believers Hymnbook are bundled inside the app instead, by permission of
  John Ritchie Ltd; their records are in `licenses/hymnals/`.)
- [`docs/making-a-hymn-book.md`](docs/making-a-hymn-book.md): make a hymn
  book file by hand or with ChatGPT/Claude (templates in `templates/`).
  [`docs/importing-hymn-books.md`](docs/importing-hymn-books.md): import it
  into the app. The app links to both from its Hymn books screen.
- [`docs/repositories.md`](docs/repositories.md) explains how hymnal
  repositories work and how anyone can publish their own. The app links to
  it from its repository screen. File formats are defined in `schemas/`.
- Full hymn texts are **not** stored in this public repo. The working files in
  `hymnbooks/` are local only and must stay untracked.
- Takedown requests use the issue template in `.github/ISSUE_TEMPLATE/`.

## Current Production Catalog

The compatibility catalog remains at:

`catalog/translations.json`

It mirrors the current production Bible catalog so existing app code can keep
working while the repo structure evolves.
