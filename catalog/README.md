# Translation Catalog README

This folder contains a template JSON catalog for downloadable Bible files.

The goal is to let the app fetch a translation list from GitHub and support
multiple artifact formats for the same translation, including:

- `sqlite`
- `usfx`
- `osis`
- `zefania`

## Files

- `translations.json`
  The catalog template the app can eventually download from GitHub.

## Recommended GitHub Repo Layout

Keep the catalog in a separate data repo from the Flutter app.

Example:

```text
bible-data/
  catalog/
    translations.json
  kjv.sqlite
  asv.sqlite
  web.sqlite
  English/
    eng-kjv2006_usfx.xml
    eng-web.usfx.xml
    asv_osis.xml
  Zefania/
    sample_zefania.xml
```

Recommended raw catalog URL pattern:

```text
https://raw.githubusercontent.com/<your-org-or-user>/bible-data/main/catalog/translations.json
```

## Catalog Structure

Each entry in `translations.json` represents one logical translation.

A translation can expose one or more downloadable artifacts.

Example shape:

```json
{
  "id": "kjv",
  "name": "King James Version",
  "language": "en",
  "languageName": "English",
  "description": "Classic English Bible",
  "enabled": true,
  "displayOrder": 10,
  "artifacts": [
    {
      "format": "sqlite",
      "downloadUrl": "https://raw.githubusercontent.com/PopGTN/bible-data/main/kjv.sqlite",
      "fileName": "kjv.sqlite",
      "parserVersion": 4,
      "schemaVersion": 1,
      "sizeBytes": 12345678,
      "sha256": "real_sha256_here",
      "isPreferred": true
    },
    {
      "format": "usfx",
      "downloadUrl": "https://raw.githubusercontent.com/PopGTN/bible-data/main/English/eng-kjv2006_usfx.xml",
      "fileName": "eng-kjv2006_usfx.xml",
      "parserVersion": 4,
      "sizeBytes": 23456789,
      "sha256": "real_sha256_here",
      "isPreferred": false
    }
  ]
}
```

## Field Guide

Top-level catalog fields:

- `version`
  Catalog schema version. Bump this only when the JSON shape changes.
- `updatedAt`
  Timestamp for when the catalog was last updated.
- `translations`
  List of downloadable translation entries.

Translation entry fields:

- `id`
  Stable unique ID used by the app. Do not change this once released.
- `name`
  User-facing translation name.
- `language`
  Short language code like `en`, `es`, `fr`.
- `languageName`
  Friendly display name such as `English`.
- `description`
  Short user-facing description.
- `enabled`
  Set to `false` to hide a translation without deleting it.
- `bundledByDefault`
  Marks translations that ship inside the app by default. For this app, `kjv`
  should stay `true` so first launch always has a guaranteed local Bible even
  before downloads are configured.
- `displayOrder`
  Sort order for the UI.
- `artifacts`
  Downloadable files for this translation.

Artifact fields:

- `format`
  One of:
  - `sqlite`
  - `usfx`
  - `osis`
  - `zefania`
- `downloadUrl`
  Direct raw GitHub URL to the file.
- `fileName`
  File name the app can use when saving or displaying metadata.
- `parserVersion`
  Version of the parser output expected by the app.
- `schemaVersion`
  Only needed for `sqlite` artifacts. This helps detect incompatible DB files.
- `sizeBytes`
  Optional but recommended. Useful for showing download size later.
- `sha256`
  Optional but recommended. Useful for integrity checks.
- `isPreferred`
  Marks the artifact the app should choose first when multiple artifacts exist.

## Recommended Artifact Preference

Use this order in the app when multiple artifacts are available:

1. `sqlite`
2. `usfx`
3. `osis`
4. `zefania`

Why:

- `sqlite` is fastest to install and load.
- XML formats are still valuable as source/fallback artifacts.
- Keeping all supported source formats in the catalog helps with portability and debugging.

## Default Local KJV Rule

The app should keep `kjv` bundled locally by default.

That means:

- `kjv` should remain available in the app assets
- `kjv` should remain the default first-run translation in the app
- remote catalog entries for `kjv` are optional download/install artifacts, not
  a replacement for the bundled local fallback

Recommended rule:

- keep `kjv` as `bundledByDefault: true`
- keep every other translation as `bundledByDefault: false` unless you
  intentionally ship it in the app package too

## Current Repo Notes

- The app currently reads the remote catalog from:
  `https://raw.githubusercontent.com/PopGTN/bible-data/main/catalog/translations.json`
- Current live files in this repo use root-level SQLite artifacts plus
  language-folder XML artifacts.
- Remote "open without downloading" in the app is currently session-only and
  uses supported XML artifacts. Persisted downloads still prefer `sqlite` when
  available and supported.

## How To Add A New Translation

1. Add the source file or SQLite file to your data repo.
2. Generate the raw GitHub download URL.
3. Add a new translation entry to `translations.json`.
4. Fill in:
   - `id`
   - `name`
   - `language`
   - `description`
   - one or more `artifacts`
5. Set `enabled` to `true` only after the file URL is verified.
6. Update `updatedAt`.

## How To Add Another Artifact To An Existing Translation

Example: add a `sqlite` file for a translation that already has `usfx`.

1. Upload the new file.
2. Add a new object in that translation’s `artifacts` list.
3. Mark the best artifact with `isPreferred: true`.
4. Keep the translation `id` unchanged.

## Rules That Will Save You Trouble

- Do not change a released translation `id`.
- Do not use spaces in filenames if you can avoid it.
- Keep one logical translation as one catalog entry.
- Prefer adding a new artifact instead of duplicating the whole translation.
- Use `enabled: false` when testing or temporarily hiding broken entries.
- Keep raw GitHub URLs direct and predictable.

## SQLite Notes

SQLite downloads are supported by this catalog shape and are recommended when
you want better install speed and faster repeat loads.

For `sqlite` artifacts:

- include `schemaVersion`
- include `parserVersion`
- update them when the app storage format changes

If the app schema changes and old SQLite files become incompatible, publish a
new SQLite file and update the catalog metadata.

## XML Notes

For XML artifacts:

- `usfx`, `osis`, and `zefania` can all live in the same catalog
- the app can fall back to XML parsing if no SQLite artifact exists

## Publishing Checklist

Before turning `enabled` on:

1. Confirm the raw GitHub URL downloads the right file.
2. Confirm the file format matches the `format` field.
3. Confirm `parserVersion` and `schemaVersion` are correct.
4. Add real `sizeBytes` and `sha256` when possible.
5. Test the entry in the app.

## Suggested Next App Work

To fully use this catalog in the app, the next engineering steps are:

1. Add a Dart model for the remote catalog.
2. Add a catalog fetch service.
3. Prefer `sqlite` artifacts when available.
4. Fall back to XML artifacts when needed.
5. Merge remote catalog entries into the Versions screen.
