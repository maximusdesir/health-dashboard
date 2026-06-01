Summarize a source and save it as a formatted note in your vault.

**Source:** `$ARGUMENTS`

Vault path: set `VAULT_PATH` env var, or defaults to `~/vault`.

---

## Step 1 — Detect Source Type

| Pattern | Type | Destination |
|---|---|---|
| `youtube.com`, `youtu.be` | YouTube | `$VAULT_PATH/2 Sources/YouTube/` |
| `.mp3`, `.mp4`, `.m4a`, `podcast`, RSS, `spotify.com/episode`, `overcast.fm` | Podcast | `$VAULT_PATH/2 Sources/Podcasts/` |
| Ends in `.md` or local file path | Local | `$VAULT_PATH/1 Notes/` or `$VAULT_PATH/2 Sources/` |
| Any other URL | Article | `$VAULT_PATH/2 Sources/Articles/` |

---

## Step 2 — Extract Content

**YouTube:**
```bash
yt-dlp -J "$ARGUMENTS" 2>/dev/null
yt-dlp --write-auto-sub --write-sub --sub-langs "en.*" --sub-format vtt --skip-download -o '/tmp/yt_transcript' "$ARGUMENTS" 2>/dev/null
ls /tmp/yt_transcript* 2>/dev/null
```
Read the `.vtt` file. Strip VTT formatting to get clean transcript text. If no transcript, use description + title from JSON.

**Podcast:**
Try yt-dlp first. If that fails, fetch the URL directly.

**Article:**
Use the WebFetch tool to retrieve the page content.

**Local file:**
Use the Read tool.

---

## Step 3 — Summarize

Produce:
1. **Title** — clean, descriptive (no "Summary of…" prefix)
2. **Author/Creator** — channel, author, publication
3. **Key Ideas** — 4–8 bullets of the most important concepts
4. **Notes** — 2–4 paragraphs with nuance, context, and examples
5. **Quotes** — 2–5 notable quotes (with timestamps for video/audio)
6. **My Take** — leave blank
7. **Tags** — 3–6 lowercase hyphenated tags

---

## Step 4 — Write the Note

```markdown
---
title: TITLE
type: TYPE  # youtube | podcast | article | book | course
source: ORIGINAL_URL_OR_PATH
author: AUTHOR_OR_CREATOR
date: TODAY_DATE
tags: [TAG1, TAG2, TAG3]
status: read
---

# TITLE

## Key Ideas

- 
- 

## Notes



## Quotes

> 

## My Take

_To be filled in._

---
*Captured: MONTH DAY, YEAR*
```

Filename: use the title, sanitized (replace `/`, `:`, `?`, `*`, `"`, `<`, `>`, `|` with `-`).

---

## Step 5 — Report Back

Tell the user:
- File path created
- Title and author
- One-line summary of content

Clean up `/tmp/yt_transcript*`.
