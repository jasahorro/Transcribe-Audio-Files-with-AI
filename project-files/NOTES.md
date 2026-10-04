# Project files

| File | What it is |
|---|---|
| `srtSubtitles.srt` | Subtitles from my **enhanced** transcription job (custom vocabulary + vocabulary filter) |
| `filler-words-filter.txt` | Vocabulary filter, **improved after the project** (one word per line, added `uh-huh`) |
| `custom-vocabulary.txt` | Custom vocabulary in table format, **rebuilt after the project** (added `forbidden → Forbidden`) |

## What the subtitles taught me
- ✅ Custom vocabulary worked: "Player A" is capitalized, "repositories" is spelled correctly.
- ✅ The vocabulary filter worked: a filler word was masked as `***` (0:41).
- ❌ "Uh-huh" was not filtered. **Filters match whole words only**, so `uh` doesn't catch `uh-huh`.
- ❌ "403 forbidden" stayed lowercase. Transcribe likely output the number `403`, so the phrase `four-or-three-forbidden` never matched. Adding `forbidden` as its own phrase should fix it.
- 📝 Vocabulary filter files should list **one word per line**.

The improved files weren't re-run because the AWS resources were already cleaned up.
