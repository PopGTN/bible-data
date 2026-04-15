# bible-data

Data repository for the Basic Bible App.

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
  licenses/
    README.md
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

## Current Production Catalog

The compatibility catalog remains at:

`catalog/translations.json`

It mirrors the current production Bible catalog so existing app code can keep
working while the repo structure evolves.
