# Catalog README

This folder contains machine-readable catalogs for the Basic Bible App.

## Catalog Types

- `production/bibles.json`
  Cleared or provisionally-cleared Bible download catalog for the app.
- `testing/ebible_list.json`
  Testing snapshot from eBible.org. Useful for parser and UI work, but not a
  production distribution list.
- `research/pending_review.json`
  A holding area for translations or hymnbooks that still need rights review.
- `translations.json`
  Backward-compatible alias of the production Bible catalog.

## Rights Fields

Production entries may include these extra rights fields in addition to the app
download metadata:

- `rightsStatus`
  One of:
  - `public_domain`
  - `open_license`
  - `permission_granted`
  - `unclear`
  - `copyrighted_no_distribution`
- `allowDownload`
  Whether the app may offer direct downloads from this repo.
- `allowBundling`
  Whether the app may package the content directly.
- `license`
  Short human-readable license label.
- `licenseUrl`
  URL for the license or source notice.
- `copyrightNotice`
  Plain-text copyright or public-domain note.
- `officialSourceUrl`
  Upstream source or project page.
- `licenseNotesPath`
  Path in this repo to the detailed review notes.

Unknown JSON fields are intentionally allowed so the app can safely ignore
rights metadata it does not use yet.
