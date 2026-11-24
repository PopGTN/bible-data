# Import a hymn book into the app

The Basic Bible app comes with The Believers Hymn Book and The New Believers
Hymnbook. You can add other hymn books from a file on your phone, tablet or
computer.

Don't have a file yet? See [Make your own hymn book file](making-a-hymn-book.md).
Looking for books others have published? See
[repositories](repositories.md).

## What files work

- Hymn book files in the app's JSON format: `.json`, or compressed `.json.gz`.
- Both the full format (with book details and copyright) and the simpler
  older format (just a list of hymns) are accepted.
- Up to 20 MB per file and 20,000 hymns.

PDFs, Word documents and plain text can't be imported directly. Convert
them first: [Make your own hymn book file](making-a-hymn-book.md) explains
how, including with ChatGPT or Claude.

## Import a file

1. Open the **Hymns** tab.
2. Tap the **Hymn books** button at the top right (the books icon). You can
   also tap the book's name at the top, then **Manage hymn books**.
3. Tap **Import a hymn book file**.
4. Choose the file:
   - **Android / iPhone / iPad**: the file picker opens; look in Downloads,
     Files, or Drive.
   - **Windows / Mac / Linux**: the normal Open dialog.
   - **Web browser**: the browser's file picker.
5. The app checks the file and tells you how many hymns it found.
6. **Name the book.** The name is filled in from the file; change it if you
   like. The **short name** (optional, for example `GH`) is shown at the top
   of the Hymns tab; if you leave it empty, the full name is used.
7. Tap **Import**. Tap **Open** in the message that appears to switch to the
   book, or pick it later from the book list at the top of the Hymns tab.

## Update, rename or remove a book

- **Update**: import the new version of the file. The app recognises it as
  the same book when the file has the same name (simple format) or the same
  book `id` (full format), and tells you it will replace the copy on your
  device. Your favorites are kept.
- **Rename**: in **Hymn books**, tap **⋮** next to the book → **Rename**.
- **Remove**: tap **⋮** → **Remove**. Your favorites for that book are kept in
  case you add it again.
- The books that come with the app can't be renamed or removed.

## Where imported books are kept

- Imported books are stored **only on this device**, inside the app. They are
  not uploaded anywhere or shared with other users, and they don't sync
  between your devices: import the file on each device.
- Uninstalling the app deletes them. In a web browser, clearing the site's
  data deletes them.
- Favorites, search, sorting, copy, share and print work the same as for the
  built-in books.

## If the import fails

The app shows "Could not import:" followed by one of these:

| Message | What it means | What to do |
|---|---|---|
| The file is not valid JSON. | A typo in the file: usually a missing or extra comma, or a missing `"`. | Check the file as described in [Check the file](making-a-hymn-book.md#check-the-file). |
| This file is not a hymnal. Expected a list of hymns, or an object with a "hymns" list. | It's JSON, but not a hymn book (for example a Bible or another app's file). | Make sure the file follows the [hymn book format](making-a-hymn-book.md#what-the-file-looks-like). |
| No hymns were found in this file. | The `hymns` list is empty, or no hymn has a number (`id`) and words. | Every hymn needs a whole-number `id` and at least one verse or a chorus. |
| The file is too large to be a hymnal. / The file has too many hymns. | Over the size or count limit. | Split it into several books. |
| The compressed file is damaged. | A `.gz` file that didn't compress or download properly. | Compress it again, or import the plain `.json`. |

Hymns with a missing number or no words are skipped, and if two hymns share
a number only the first is kept. If hymns seem to be missing after
importing, check those two things in your file.

## Copyright

Only import books you're allowed to use. Imported files stay on your device,
but the app shows and copies whatever is in them, so make sure the copyright
line in the file is correct. See Settings → **Legal & takedown requests** in
the app.
