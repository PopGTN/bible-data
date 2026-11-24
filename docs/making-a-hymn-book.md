# Make your own hymn book file

The Basic Bible app can show any hymn book you have as a file: your church's
own book, an old public-domain hymnal, or a children's songbook. This guide
shows how to make that file. When it's ready,
[import it into the app](importing-hymn-books.md), or
[publish it in a repository](repositories.md) so others can download it.

There are two ways to make the file:

- **[Option A: by hand](#option-a-write-it-by-hand)**: start from the
  template and type or paste the hymns. Best for short books, or for fixing
  and finishing a file.
- **[Option B: with ChatGPT or Claude](#option-b-let-chatgpt-or-claude-convert-a-pdf)**:
  give an AI chat the PDF and a ready-made message, and it writes the file
  for you. Best for long books; you still need to check its work.

> **Only make files from books you're allowed to copy.** Most hymns written
> before about 1930 are in the public domain, but a hymn *book* can still be
> under copyright (its selection, numbering and edits), and newer hymns
> usually belong to their author or publisher. For your own use, importing
> your file is your responsibility. To share it, you need the rights holder's
> permission unless everything in it is public domain or openly licensed.

## What the file looks like

A hymn book file is a text file in JSON, a common format for data. It has
two parts: **`book`**, the details shown on the book's About page, and
**`hymns`**, the list of hymns.

```json
{
  "format": "hymnal",
  "schemaVersion": 1,
  "book": {
    "id": "sunday-hymns",
    "title": "Sunday Hymns",
    "shortTitle": "SH",
    "copyright": "Public domain.",
    "rightsStatus": "public_domain"
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

The rules, in short:

- **`id`** is the hymn's number in the book: a whole number, no quotes.
- **`title`** is usually the hymn's first line.
- **`verses`** is a list of verses; each verse is a list of lines,
  `["line one", "line two"]`. Don't number the verses; the app does.
- **`chorus`** holds the chorus **once**. The app shows it after verse 1, or
  after every verse if the reader turns that on. No chorus: `[]`.
- **`author`**, **`meter`**, **`tuneName`**: fill in if you have them,
  otherwise `""`.
- Optional extras:
  - `notes`: `["Repeat the last two lines."]`, shown under the title
  - `alternateChoruses`: `[{"label": "Chorus for tune Sankey", "lines": [...]}]`,
    shown dimmed
  - `copyright`: a hymn's own copyright line
- Plain text only, no HTML.

Every field is described in [repositories.md](repositories.md#step-1-write-the-hymn-books),
and the exact definition is [`schemas/hymnal-v1.schema.json`](../schemas/hymnal-v1.schema.json).

The app also opens the simpler older format, a list of hymns without the
`book` part (`[ {"id": 1, …}, {"id": 2, …} ]`). The full format is better:
it keeps the title and copyright with the book.

---

## Option A: write it by hand

1. **Download the template**:
   [`templates/hymn-book-template.json`](../templates/hymn-book-template.json)
   (on GitHub, click **Download raw file**). It contains two example hymns.
2. **Open it in a plain-text editor**, not a word processor (Word or Google
   Docs add hidden formatting):
   - Windows: Notepad, or the free [VS Code](https://code.visualstudio.com/)
   - Mac: TextEdit in plain-text mode (Format → Make Plain Text), or VS Code
   - Android: any "text editor" app; a computer is much easier
3. **Fill in `book`**: a short `id` (lowercase letters, numbers and dashes,
   like `sunday-hymns`), the title, a short title for the top of the Hymns
   tab, and the copyright line.
4. **Replace the example hymns with yours.** Copy one hymn block (from `{` to
   its matching `},`) for each new hymn and change it. Watch the punctuation:
   - every line of lyrics is in "double quotes";
   - items in a list are separated by commas, **but the last one has no
     comma** after it;
   - a `"` inside a line must be written as `\"`. Curly quotes (“ ”) and
     apostrophes (') are fine as they are.
5. **Check the file** (see [Check the file](#check-the-file)), then
   [import it](importing-hymn-books.md).

Tip: you don't have to finish the book first. Import it after the first few
hymns to see how it looks, then keep adding and import again: the app
replaces the old copy.

---

## Option B: let ChatGPT or Claude convert a PDF

An AI chat can read a hymn book PDF and write the file for you. It's fast,
but it **does make mistakes**: missing lines, words "corrected" to modern
spelling, a chorus repeated after every verse. Always check the result.

### What you need

- The hymn book as a PDF. A scan works if the pages are clear; a PDF with
  selectable text works best.
- [ChatGPT](https://chatgpt.com) or [Claude](https://claude.ai). Free
  accounts work, but they limit file size and how much they write per reply,
  so expect to work in batches.
- The chat template: [`templates/ai-hymn-book-prompt.md`](../templates/ai-hymn-book-prompt.md).

> You are sending the book's text to the AI company. Only do this with books
> that are public domain, or where the rights holder has agreed.

### Steps

1. **Open the chat template** and copy **Message 1**.
2. **Fill in the blanks**: everything in `[SQUARE BRACKETS]`: the book's
   title, short title, publisher, year, copyright line, and which hymns to do
   first. Start small, for example **hymns 1 to 30**.
3. **Start a new chat**, paste the message, **attach the PDF** (the paperclip
   or **+** button), and send.
   - If the PDF is too big to attach, split it into smaller PDFs (for
     example 30–50 pages each) and attach the part that contains the hymns
     you asked for.
4. **Save the reply.** Copy the JSON from the code block (most chats have a
   **Copy** button on the block) into a new plain-text file, and save it as
   `my-hymn-book.json`.
5. **Do the next batch.** In the same chat, send **Message 2** with the next
   numbers (for example 31 to 60). It replies with just the new hymns.
   Paste them into your file:
   - find the `]` near the very end of the file (the end of the `hymns`
     list),
   - type a comma after the last hymn's closing `}`,
   - paste the new hymns after that comma, before the `]`.
6. Repeat step 5 until the book is done. If the chat gets slow or starts
   forgetting the rules, start a fresh chat with Message 1 (asking for the
   next numbers) and paste its hymns the same way.
7. **Check the file** (below). Optionally send **Message 3** first, to have
   the AI compare its work with the PDF.
8. [Import it into the app](importing-hymn-books.md) and read through a few
   hymns.

### What to check in AI output

- **Numbering**: no hymns missing or doubled. The reply lists what it
  skipped; follow that up.
- **`[?]`**: words the AI couldn't read. Search the file for `[?]` and fill
  them in from the book.
- **Changed words**: old spellings like "Saviour", "thee", "o'er" should stay
  as printed.
- **Chorus**: printed once in `chorus`, not pasted into every verse.
- **Line breaks**: one entry per printed line.
- **Extras**: no page numbers, headers or "Chorus:" labels inside the lyrics.

---

## Check the file

Before importing, make sure the file is valid JSON:

- Paste it into a JSON checker such as [jsonlint.com](https://jsonlint.com)
  (only for public-domain books, since this sends the text to that site), or
  open it in VS Code, which underlines mistakes in red.
- The usual mistakes: a missing comma between two hymns, an extra comma
  after the last item in a list, or a missing `"`.
- For a full check against the format, validate it with
  [`schemas/hymnal-v1.schema.json`](../schemas/hymnal-v1.schema.json) (for
  example at [jsonschemavalidator.net](https://www.jsonschemavalidator.net)).

The app also checks the file when you import it, and tells you what's wrong
(see [Import problems](importing-hymn-books.md#if-the-import-fails)).

Optional: compress the finished file to `.json.gz` (for example with
`gzip my-hymn-book.json` on Mac/Linux, or 7-Zip on Windows). It becomes
about five times smaller, and the app imports it the same way.
