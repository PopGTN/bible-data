# Chat template: turn a hymn book PDF into a Basic Bible hymn book file

Use this with ChatGPT, Claude or a similar AI chat. How to use it, and what to
check afterwards, is explained in
[`docs/making-a-hymn-book.md`](../docs/making-a-hymn-book.md).

1. Copy **Message 1** below.
2. Replace everything in `[SQUARE BRACKETS]` with your book's details.
3. Paste it into a new chat, **attach the PDF** (or the pages you want), and
   send.
4. For the next batch of hymns, send **Message 2** in the same chat.

Only do this with a book you are allowed to copy (public domain, or you have
the publisher's permission): you are sending its text to the AI service.

---

## Message 1: start (copy everything inside the box)

```text
I'm turning a hymn book into a JSON file for the Basic Bible app. The attached
PDF is the hymn book. Convert it into JSON exactly in the format below.

Book details:
- Title: [FULL BOOK TITLE]
- Short title: [SHORT NAME, e.g. BHB]
- Publisher: [PUBLISHER, or leave empty]
- Year: [YEAR, or leave empty]
- Language code: [en]
- Copyright line: [e.g. "Public domain." or the book's own notice]
- Rights: [public_domain / permission_granted / open_license]

Output format (one object for the whole book):

{
  "format": "hymnal",
  "schemaVersion": 1,
  "book": {
    "id": "[short-id-with-dashes]",
    "title": "...",
    "shortTitle": "...",
    "publisher": "...",
    "year": 1900,
    "language": "en",
    "copyright": "...",
    "rightsStatus": "public_domain"
  },
  "hymns": [
    {
      "id": 1,
      "title": "First line of the hymn",
      "author": "",
      "meter": "",
      "tuneName": "",
      "verses": [
        ["line 1 of verse 1", "line 2 of verse 1"],
        ["line 1 of verse 2", "line 2 of verse 2"]
      ],
      "chorus": [],
      "notes": []
    }
  ]
}

Rules:
1. Copy the words exactly as printed. Do not modernise spelling or
   punctuation, fix grammar, fill in missing words, or add anything that is
   not in the book.
2. "id" is the hymn's number in the book, as a whole number (no quotes).
3. "title" is the first line of the hymn, without the full stop or comma at
   the end, unless the book prints a separate title.
4. Each verse is a list of lines, one entry per printed line. Do not number
   the verses.
5. A chorus or refrain goes in "chorus" ONCE, even if the book repeats it
   after every verse. No chorus: "chorus": [].
6. "author", "meter" (e.g. "8.7.8.7.D" or "C.M.") and "tuneName": copy them
   if the book prints them, otherwise use "".
7. Leave out page numbers, running headers and footers, music, index pages,
   verse numbers, and labels such as "Chorus:" or "Refrain".
8. Singing instructions such as "Repeat the last two lines" or "Sung to the
   tune Yarmouth" go in "notes" as a short sentence, never inside the lyrics.
9. An optional or alternative chorus goes in "alternateChoruses" as
   [{"label": "Optional chorus", "lines": [...]}] instead of "chorus".
10. If a hymn has its own copyright notice, copy it into "copyright" for that
    hymn, exactly as printed.
11. If a hymn's printed words already capitalise the whole first word (like
    "AMAZING grace"), write it normally ("Amazing grace").
12. If you cannot read a word, write [?] in its place. Never guess.
13. Plain text only: no HTML, no Markdown, no comments inside the JSON.

This time, do hymns [FIRST NUMBER] to [LAST NUMBER] only.

Reply with the JSON in one code block. After the code block, list:
- the number of the last hymn you finished,
- any hymns you skipped or could not read, and why,
- any hymn where you used [?].
```

---

## Message 2: next batch (send in the same chat)

```text
Next: hymns [FIRST NUMBER] to [LAST NUMBER], same rules.
This time reply with ONLY the hymn objects, separated by commas, with no
"format" or "book" part and no surrounding [ ] — I'm adding them to the end
of the "hymns" list myself. Then list the last hymn you finished, anything you
skipped, and any [?].
```

---

## Message 3 (optional): check your own work

```text
Check the hymns you gave me against the PDF: verse count, line breaks, the
chorus, and any changed words. List every difference you find; don't
rewrite the JSON.
```
