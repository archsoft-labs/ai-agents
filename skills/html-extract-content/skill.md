---
name: html-extract-content
version: 1.0.0
author: Ricardo de Luna Galdino
description: >
  Extracts the main content from one or more HTML files, handles image renaming,
  and produces one clean A4-formatted HTML file per input.
  TRIGGER this skill whenever the user's intent is to extract and clean HTML content.
  The user may mention a file path, a glob pattern, or describe the files conversationally.
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

Extract the main content from the provided HTML file(s), normalize image references, and
produce one clean A4-ready HTML file per input.

---

# Step 1 — Resolve Target Files

Extract the file target from the user's message. Accept any form:
- Explicit path: `docs/page.html`, `"my file.html"`
- Glob pattern: `posts/*.html`, `**/*.html`
- Conversational reference: "the HTML in my downloads folder", "that file I just mentioned"
- The user's IDE selection or a file they pasted/attached

If a glob is provided, expand it to a concrete file list. If no files match, report that and stop.

If no file can be determined from context, ask once: *"Which HTML file (or files) should I process?"*

Do not ask again after the user answers.

Do not ask for confirmation before starting unless a glob matches an unexpectedly large number of
files (more than 20). In that case, list the matches and ask before proceeding.

---

# Step 2 — Process Each File

Process each resolved file independently through the steps below.

## 2a. Content identification

Before extracting, analyze the HTML structure to identify what the main content is.
Use the following signals, in order of reliability:

1. **Semantic tags**: `<main>`, `<article>`, `<section>` with a meaningful heading
2. **Common content patterns**: the largest contiguous block of text-heavy HTML, typically
   containing `<h1>` or `<h2>` followed by paragraphs, lists, images, or code blocks
3. **Structural clues**: high text-to-markup ratio, absence of link-heavy navigation patterns,
   presence of a clear opening heading
4. **Common wrapper patterns**: `id` or `class` values containing words like `content`, `post`,
   `body`, `entry`, `text`, `main`

If after this analysis the main content can be identified with reasonable confidence, proceed.

If the content boundaries are genuinely ambiguous — for example, the page has multiple candidate
sections of similar size and no clear semantic structure — stop and ask the user:
*"I found multiple content candidates in this file. Which section should I extract?
  [describe each candidate briefly, e.g., 'A: the top section about X', 'B: the section about Y']"*

Do not guess when ambiguous. Wait for the user's answer before continuing.

Once the main content is identified, isolate strictly its heading and body.
Discard everything else: navigation, sidebars, footers, headers, banners, cookie notices, and ads.

## 2b. Image extraction and renaming

- Collect all `<img>` elements that belong to the extracted content.
- Save each image into a folder named exactly `images/`, co-located with the source file.
- Rename each image using this exact pattern, preserving the original extension:
  `[source-filename-in-kebab-case]-image-001.png`, `…-image-002.jpg`, etc.
- Update all `<img src="…">` references in the output file to point to the renamed images in
  `images/`.

## 2c. Output file naming

Derive the base name from the original filename: lowercase, spaces → hyphens
(e.g., `"My Page.html"` → `my-page`).

Produce one file in the same directory as the source:
- `[base-name].html`

## 2d. Styling and formatting

- Preserve only CSS rules directly tied to the extracted content: fonts, heading sizes,
  paragraph spacing, inline code, lists, and content-internal tables. Discard everything else
  (layout grids, sidebar styles, navigation, ad blocks, third-party classes).
- Force `background-color: #FFFFFF` on the body and ensure strong text contrast. The result should
  look like a clean, print-ready study handout.
- Use `@page` and `@media print` to enforce A4 paper size.
- Add a CSS-driven print footer with pagination in the format: `[current page] / [total pages]`.

---

# Step 3 — Confirm and Report

After processing all files, give the user a concise summary:
- List each output file created.
- Note the image count per file, if any.
- If any file failed or produced no extractable content, explain why.
