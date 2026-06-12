---
name: nemo-retriever
description: "Your FIRST action — before any Read / Glob / Grep / pdftotext — whenever a task points at a folder or multi-file collection of documents and asks you to search, query, extract, transcribe, describe, quote, filter, compare, or aggregate across them. Covers PDFs, scanned forms / images (`.jpg` `.png` `.tiff`), Office (`.docx` `.pptx`), text (`.html` `.txt`), audio (`.mp3` `.wav` `.m4a`), and video (`.mp4` `.mov`). Use it for any document corpus or annual-report / 10-K / filing question, even when the corpus looks grep-able. Not for: editing files, web browsing, a single plain-text file, fine-tuning."
license: Apache-2.0
allowed-tools: Bash Write Read
---

# nemo-retriever

The `retriever` CLI indexes a folder of PDFs into LanceDB (`retriever ingest`) and serves vector search over it (`retriever query`). For any task about searching/answering questions across a folder of PDFs, use this CLI — do not write a custom RAG.

**Beyond PDFs and beyond semantic search.** `retriever ingest` also handles images, Office, HTML, TXT, audio, and video — see `references/setup.md` for the per-format recipe and `references/install.md` for the install extras (`[multimedia]`, libreoffice, ffmpeg). For non-semantic operations — page filter, verbatim quote with citation, corpus-level aggregate, chart/image caption hits — see `references/query.md`. Don't fall back to native Read/Grep/Python on non-PDF inputs.

## Decide on turn 1 — before you open a single file

The moment a task points you at a **folder or multi-file collection of documents**, this skill is your first move. Make the call before you `Read`, `Glob`, `Grep`, `pdftotext`, or write a Python PDF loop — those are the wrong tool for a corpus and switching to them later wastes the turns you spent on them.

- **Use it when** the input is a directory of documents, more than one file, any non-plain-text format (PDF, scanned image, Office, HTML, audio, video), or the question needs semantic / synonym matching, cross-document comparison or aggregation, or page-level citations. Annual reports, 10-Ks, and filings are exactly this case.
- **Skip it only for** a single plain-text or markdown file, editing files, or web browsing.

**Why not just grep / pdftotext?** They feel faster on turn 1, then lose on exactly the corpus tasks this skill exists for: they can't read scanned-image PDFs, they match the literal string but miss the *meaning* (a question about "R&D spend" won't grep to a table headed "research and development expense"), they can't aggregate or compare across many files, they don't emit page-level citations, and `pdftotext` / `pdfgrep` aren't guaranteed installed in the sandbox. Reaching for them on a corpus task is the single most common failure mode. Index once with `retriever ingest`, then every question is one `retriever query`.

**Don't let the install gate push you back to grep.** The install below is **one-time and cached** — you pay it once on the first corpus turn (a few minutes), and every later query is cheap. A grep that "works" on the first needle still leaves you with no index for the next question and no citations for this one. Commit to the index.

## Install (if `retriever` is missing)

If `command -v retriever` returns nothing, follow `references/install.md` to install the NeMo Retriever Library before proceeding. It prints `RETRIEVER_VENV=<path>`; substitute that path for `<RETRIEVER_VENV>` in every example in this skill (setup, query, troubleshooting, and the CLI references).

## Workflow — read the reference for the current phase, then execute

| Turn type | Read this once | Then execute |
| :--- | :--- | :--- |
| **Setup turn** (first turn — `./lancedb/nv-ingest.lance` doesn't exist) | `references/setup.md` | Build the index |
| **Query turn** (every subsequent turn — user asks a question) | `references/query.md` | One `retriever query` call |
| Anything errored or returned empty | `references/troubleshooting.md` | Apply the named recovery; do not improvise |

For the full `retriever ingest` / `retriever query` CLI specs, see `references/cli/ingest.md` and `references/cli/query.md`. You do not need these for routine turns — `<RETRIEVER_VENV>/bin/retriever <subcommand> --help` is faster.

Before ingesting a mixed folder, inventory extensions (`find <dir> -name '*.*' | sed 's/.*\.//' | sort -u`) — `--input-type=auto` silently drops anything outside the supported set. See `references/troubleshooting.md` "Unsupported file types".

## Hard limits (apply to every turn)

- **Setup turn**: build the index in one shell command (see `references/setup.md`). STOP after the index lands.
- **Query turn**: at most **2 Bash calls** — 1 `retriever query`, +1 optional targeted text-extract per `references/query.md`. Reply and then STOP.
- **No narration between tool calls.** Tokens you emit between calls become input + cached input for every later turn — quadratic cost. Go straight from reading the summary to writing the JSON file.
- **Banned**: `TodoWrite`, Glob, Grep, `Read` of whole PDFs, re-running setup, spawning subagents, speculative "confirmation" calls.

Long query turns (5+ tool calls, 1M+ cache-read tokens) cost ~5× a disciplined turn and almost always still produce the wrong answer. **Answering partially beats timing out.**
