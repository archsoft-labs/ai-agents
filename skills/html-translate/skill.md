---
name: html-translate
version: 1.0.0
author: Ricardo de Luna Galdino
description: >
  Translates an HTML file into any target language the user requests,
  producing an A4-formatted translated HTML file.
  TRIGGER this skill whenever the user's intent is to translate an HTML file —
  regardless of how they phrase it. The user may mention a file path or describe
  the files conversationally. They may name the language formally ("French"), colloquially
  ("espanhol"), or as a locale code ("pt-br", "zh-CN").
---

# Scripting Language

Any automation or script required must be written in **Python 3**. For executing commands or
shell operations, use **Git Bash** (Bash tool). Do not use PowerShell or JavaScript for scripting.
When the task can be done directly (e.g., writing HTML output), prefer doing it directly rather
than generating an unnecessary script.

## Temporary file cleanup

Any temporary files created during processing (e.g., intermediate scripts like `process_course.py`,
scratch files, helper scripts) **must be deleted immediately after use**, before reporting results
to the user. Do not leave temporary files in the user's working directory.

# Goal

Translate the provided clean HTML file(s) into the user's target language and produce one
A4-ready translated HTML file per input.

---

# Step 1 — Resolve Intent Before Acting

Before doing any file work, resolve two things from the user's message and conversation context:

## 1a. Target language

Extract the target language from whatever the user said. Accept any form:
- Locale codes: `pt-br`, `zh-CN`, `fr`, `ja`
- Full names in any language: "French", "espanhol", "japonês", "alemão", "中文"
- Colloquial: "portuguese", "brasileiro", "chinese simplified"

Normalize to a lowercase kebab-case file suffix (examples: `pt-br`, `fr`, `ja`, `de`, `zh-cn`).

If the language cannot be determined from context, ask once: *"What language should I translate to?"*

## 1b. Target files

Extract the file target from the user's message. Accept any form:
- Explicit path: `docs/article.html`, `"my file.html"`
- Glob pattern: `posts/*.html`, `**/*.html`
- Conversational reference: "the HTML in my downloads folder", "that file I just mentioned"
- The user's IDE selection or a file they pasted/attached

If a glob is provided, expand it to a concrete file list. If no files match, report that and stop.

If no file can be determined from context, ask once: *"Which HTML file (or files) should I translate?"*

**Never ask for both at once if one is already clear.** Resolve what you can from context, ask only
for what is genuinely missing. Do not ask again after the user answers.

---

# Step 2 — Process Each File

Process each resolved file independently through the steps below.

## 2a. Translation

Translate the entire content into the target language — every sentence, heading, caption,
and list item. Nothing may be omitted or summarized. Use natural, fluent prose that reads as if
written by a native speaker. Vocabulary should be clear and accessible to non-specialists.

Translated sentences must be cohesive, logically connected, and easy to understand. Each sentence
must flow naturally into the next — avoid literal word-for-word translations that sound awkward or
unnatural in the target language. The reader should never need to re-read a sentence to grasp its
meaning.

After translating, review the full text for logical coherence and natural flow before writing the
output file.

## 2b. Output file naming

Derive the base name from the input filename: lowercase, spaces → hyphens
(e.g., `"my-article.html"` → base `my-article`).

Produce one file in the same directory as the source:
- `[base-name]-[language-suffix].html` — full translation
  (e.g., `my-article-pt-br.html`, `my-article-fr.html`, `my-article-ja.html`)

Image references (`<img src="…">`) must remain unchanged.

## 2c. Styling and formatting

- Carry over all styles from the source file without modification.
- If the source file lacks A4 print styles, add them: use `@page` and `@media print` to enforce
  A4 paper size, and add a CSS-driven print footer with pagination in the format:
  `[current page] / [total pages]`.

---

# Step 3 — Confirm and Report

After processing all files, give the user a concise summary:
- List each output file created.
- If any file failed or produced no translatable content, explain why.

Do not ask for confirmation before starting unless a glob matches an unexpectedly large number of
files (more than 20). In that case, list the matches and ask before proceeding.
