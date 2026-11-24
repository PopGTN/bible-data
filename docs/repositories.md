# Hymnal repositories: how they work and how to make your own

The Basic Bible app can download hymn books from **repositories**, much like
F-Droid installs apps from repositories. A repository is just a JSON file on
the web (the *index*) that lists hymn books and where to download them. Anyone
can publish one; app users add it by pasting its address.

- The app comes with one repository, the **official repository**, run by the
  app's developer:
  `https://raw.githubusercontent.com/PopGTN/bible-data/main/catalog/production/hymnals.json`
- Users add other repositories in the app under
  **Hymns → Hymn books → Manage repositories → Add repository**.
- Hymn books from the official repository are only listed once their rights
  are cleared. Books from other repositories are the responsibility of whoever
  runs that repository (see [Rights and takedowns](#rights-and-takedowns)).

## How the app uses a repository

1. The user pastes the index address. The app downloads it, checks that it is
   a repository index, and shows its name, maintainer and your disclaimer
   next to the app's own third-party notice. The user must tick "I understand"
   before it is saved.
2. The books in your index appear under **Hymn books → Available from
   repositories**, with their publisher, hymn count and your repository's
   name.
3. When the user taps **Download**, the app fetches the book, checks its size
   and SHA-256 checksum, makes sure it parses as a hymn book, and stores it on
   the device. It then works offline like any other book.
4. The app reloads your index the first time the user opens the library or
   repository screen after starting the app, and whenever they refresh it
   (pull down, or the refresh button). It keeps the last good copy for
   offline use. Books you list under `withdrawn` are deleted from users'
   devices at that point.

## Make your own repository

You need two kinds of file, both plain JSON, hosted anywhere that serves files
over **https**:

```
my-hymn-repo/
├── index.json            ← the repository index (one per repository)
└── hymnals/
    ├── sunday-hymns.json.gz
    └── childrens-songs.json
```

A free way to host it is a public GitHub repository: the "raw" address of
`index.json` (`https://raw.githubusercontent.com/<you>/<repo>/main/index.json`)
is what users paste into the app. GitHub Pages, a church website, or any
static file host works too.

### Step 1: Write the hymn books

New to this? [Make your own hymn book file](making-a-hymn-book.md) walks
through it step by step, by hand or with ChatGPT/Claude, using the
[template](../templates/hymn-book-template.json).

Each book is one JSON file, plain or gzipped (`.json.gz`, which is much
smaller). Use the **hymnal v1 format**: book details plus the hymns.

```json
{
  "format": "hymnal",
  "schemaVersion": 1,
  "book": {
    "id": "sunday-hymns",
    "title": "Sunday Hymns",
    "shortTitle": "SH",
    "publisher": "Grace Chapel",
    "year": 1932,
    "language": "en",
    "copyright": "Public domain.",
    "rightsStatus": "public_domain",
    "takedownContact": "mailto:hymns@example.org"
  },
  "hymns": [
    {
      "id": 1,
      "title": "Amazing grace! how sweet the sound",
      "author": "John Newton (1779)",
      "meter": "C.M.",
      "tuneName": "New Britain",
      "verses": [
        ["Amazing grace! how sweet the sound", "That saved a wretch like me!"],
        ["'Twas grace that taught my heart to fear,", "And grace my fears relieved;"]
      ],
      "chorus": []
    }
  ]
}
```

**Book fields** (`book`, all optional except `id` and `title`):

| Field | Meaning |
|---|---|
| `id` | Short id, letters/digits/dashes. Must match the entry id in your index. |
| `title`, `shortTitle` | Full title; short title for the app bar (e.g. `BHB`). |
| `publisher`, `publisherAddress`, `publisherUrl` | Shown on the book's About page. |
| `edition`, `year`, `isbn`, `language` | Shown on the About page. `language` is a code such as `en`, `es`. |
| `copyright` | Copyright line. Shown under every hymn and added to every copy, share and print. |
| `permissionNotice` | Credit line if you have permission, e.g. "Used by permission of …". Shown and copied like `copyright`. |
| `rightsStatus` | `public_domain`, `open_license`, `permission_granted`, `unclear` or `copyrighted_no_distribution`. |
| `takedownContact` | `mailto:` address or `https:` page where rights holders can reach you. Used by the book's "Report a copyright concern" button. |

**Hymn fields:**

| Field | Meaning |
|---|---|
| `id` | The hymn's number in the book (a whole number). Duplicates: only the first is kept. |
| `title` | Usually the first line. If empty, the first line is used. |
| `author`, `meter`, `tuneName` | Shown under the title (users can hide them). Searchable. |
| `verses` | List of verses; each verse is a list of lines. |
| `chorus` | Lines of the chorus. The app shows it after verse 1, or after every verse if the user chooses. |
| `addedChorus` | An optional chorus, shown dimmed as "Optional chorus". |
| `alternateChoruses`, `optionalVerses` | Labelled optional parts: `[{"label": "Chorus for tune Sankey", "lines": [...]}]`. Shown dimmed with their label. |
| `notes` | Short notes shown under the title, e.g. `["Third line sung twice with the tune Yarmouth."]`. |
| `copyright` | This hymn's own copyright line, if it differs from the book's. |

Plain text only: no HTML. Simple tags like `<i>…</i>` are stripped. Unknown
fields are ignored, so you can keep extra data in the file.

The app also accepts the **legacy format**, a bare list of hymns
(`[{"id": 1, "title": …, "verses": …}, …]`) with no `book` block. It works,
but then the app knows nothing about the book's publisher or copyright, so
prefer the v1 format for anything you publish.

Limits: up to 20 MB per file and 20,000 hymns per book.

Machine-readable definition: [`schemas/hymnal-v1.schema.json`](../schemas/hymnal-v1.schema.json).

### Step 2: Write the index

```json
{
  "format": "basic-bible-repo",
  "schemaVersion": 1,
  "repo": {
    "id": "grace-chapel",
    "name": "Grace Chapel Hymns",
    "description": "Public-domain hymn books used at Grace Chapel.",
    "maintainer": "Grace Chapel media team",
    "homepage": "https://example.org/hymns",
    "takedownContact": "mailto:hymns@example.org",
    "disclaimer": "Only public-domain books are listed here."
  },
  "hymnals": [
    {
      "id": "sunday-hymns",
      "title": "Sunday Hymns",
      "publisher": "Grace Chapel",
      "language": "en",
      "year": 1932,
      "hymnCount": 412,
      "rightsStatus": "public_domain",
      "copyrightNotice": "Public domain.",
      "downloadUrl": "https://raw.githubusercontent.com/example/hymns/main/hymnals/sunday-hymns.json.gz",
      "sizeBytes": 183442,
      "sha256": "3f5a…",
      "schemaVersion": 1,
      "updatedAt": "2026-11-23"
    }
  ],
  "withdrawn": [],
  "bibles": []
}
```

| Field | Meaning |
|---|---|
| `format` | Must be `"basic-bible-repo"`. |
| `repo.name` | Required. Shown to users. |
| `repo.id` | Short id. Used to keep books from different repositories apart. |
| `repo.takedownContact` | Where rights holders can reach you (`mailto:` or `https:`). Strongly recommended. |
| `repo.disclaimer` | Your own note, shown when someone adds your repository. |
| `hymnals[].id`, `title`, `downloadUrl` | Required for each book. `downloadUrl` must be `https://`. |
| `hymnals[].sha256` | SHA-256 of the file exactly as served. Recommended: the app refuses a file whose checksum doesn't match. |
| `hymnals[].sizeBytes` | File size; files over the limit are refused before downloading. |
| `hymnals[].rightsStatus` | Books marked `unclear` or `copyrighted_no_distribution` are listed but **cannot be downloaded**. |
| `withdrawn` | Ids of books you have taken down. They disappear from the list and are deleted from devices that installed them. |
| `bibles` | Reserved for Bible translations in a later version; leave it empty. |

Machine-readable definition: [`schemas/repo-index-v1.schema.json`](../schemas/repo-index-v1.schema.json).

To get the checksum and size of a file:

```sh
sha256sum hymnals/sunday-hymns.json.gz   # Linux;  macOS: shasum -a 256 …
wc -c < hymnals/sunday-hymns.json.gz
```

Recompute both every time you change a book file, and bump `updatedAt`.

### Step 3: Test it

1. Check both files are valid JSON (any JSON validator, or the schemas above).
2. Optional, before publishing: serve the folder on your own computer
   (`python3 -m http.server 8000`) and add
   `http://localhost:8000/index.json` in the app. Plain `http` is only accepted
   for `localhost`.
3. Publish, then add the real `https://` address in the app and download a
   book.

## Rights and takedowns

**You are responsible for what your repository distributes.** Before listing
a book, make sure it is in the public domain, under an open licence, or that
you have the rights holder's permission to distribute it. Many hymn books are
still under copyright even when most of their hymns are old: the collection,
its numbering and edits, and any 20th-century hymns usually belong to a
publisher or author.

- Set `rightsStatus` honestly. Use `unclear` while you're still checking; the
  app will not offer those books for download.
- Fill in `takedownContact` on the repository and the books, and answer
  requests promptly.
- To take a book down, remove it from `hymnals` **and** add its id to
  `withdrawn`. Devices that installed it remove it on their next refresh.

When users add your repository, the app tells them it is run by a third
party, not by the app's developer, and that the developer does not host,
review or distribute its content.

### Reporting a problem with the official repository

If you hold the rights to content shipped with the app or listed in the
official repository, open a
[takedown request](https://github.com/PopGTN/bible-data/issues/new?template=takedown.md).
For a book from someone else's repository, use "Report a copyright concern"
on that book's About page in the app, which contacts that repository's owner.
