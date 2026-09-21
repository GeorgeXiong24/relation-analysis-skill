---
name: "relationship-analysis"
description: "Analyzes local relationship-context and chat-history files to generate a detailed relationship report. Invoke when user wants communication/relationship analysis or improvement guidance."
---

# Relationship Analysis

Analyze a user's relationship with another individual by cross-referencing two
user-provided **local** files — a relationship context file and a chat history
file — and produce a structured, actionable Markdown report.

## Mandatory Constraints (Read First)

1. **Local-only processing.** All file reading, parsing, and analysis MUST happen
   on the local machine. NEVER upload, transmit, or send user data to any cloud
   service, external API, or third-party tool. If a parsing method would require
   network/cloud access, reject it and use a local alternative instead.
2. **No fabrication.** Only report facts and patterns actually present in the two
   files. Never invent quotes, dates, or events. Mark uncertain inferences as
   "inference" or "likely".
3. **Privacy by default.** Do not log, copy, or persist extracted file content
   anywhere other than the generated report. Do not print raw chat contents into
   the final report unnecessarily; summarize instead.
4. **Language matching.** Write the final report in the language of the user's
   request (or of the input files if no explicit language is given).

## Input Requirements

The user provides exactly two files (any supported format):

1. **Relationship context file** — the user's own description of their
   relationship with the other person. May include how they met, current stage,
   goals, known preferences, and any background.
2. **Chat history file** — a record of interactions between the user and the other
   person (messaging transcript, email log, etc.).

If either file is missing, ask the user for it before proceeding. If the user
provides only one file or provides files in an unsupported format, ask for a
supported version (PDF, TXT, DOCX, CSV).

## Step 1 — Parse Local Files

Detect each file's format by extension first, then by content sniffing. Use the
correct local method below.

### TXT / MD / plain text
Read directly with the `Read` tool. Handle common encodings (UTF-8, UTF-16,
GBK/GB2312) — if the text looks garbled, re-read with a different encoding.

### CSV
Read with the `Read` tool. For large files, use a local `python`/`pandas` snippet
via `RunCommand` (no network). Identify columns representing timestamp, sender,
and message; adapt to any column naming/schema.

### PDF
Extract text locally, in order of preference:
1. `pdftotext -layout <file> -` (Poppler utility) if available.
2. Python `pdfplumber` or `PyPDF2` via a local script.
Do NOT use online/cloud PDF conversion services.

### DOCX
Extract text locally:
1. Python `python-docx` to read paragraphs and tables.
2. Fallback: unzip the `.docx` and parse `word/document.xml`.

### General rules
- Never assume a parser's output is perfect; visually verify key fields.
- Preserve chronological order for chat data. If no timestamps exist, keep the
  original file order and state this assumption in the report.
- If a file is password-protected or corrupt, report it and ask the user to
  provide an unprotected copy.

## Step 2 — Contextual Baseline Analysis

From the **relationship context file**, extract and structure a baseline profile.
Cover, as available:

1. **Basic facts** — who the other person is, the nature of the relationship
   (romantic, friendship, family, professional), duration, current stage.
2. **Personality & traits** — temperament, values, interests, communication
   preferences, emotional needs.
3. **Speaking style & communication patterns** — tone (formal/casual), verbosity,
   humor, responsiveness, preferred channels/timing.
4. **Explicit goals / concerns** the user stated about the relationship.
5. **Known sensitivities or boundaries** the user described.

Record gaps explicitly (e.g., "context file does not describe X"). This baseline
is the reference standard against which every chat interaction is evaluated.

## Step 3 — Deep Chat History Analysis

Cross-reference the **chat history** against the baseline from Step 2. For every
major interaction, evaluate:

1. **Style alignment** — does the user's tone, formality, and pacing match the
   other person's preferences established in the baseline?
2. **Emotional tone consistency & appropriateness** — is the emotional register
   suitable for the moment (e.g., empathy in distress, lightness in casual talk)?
3. **Misunderstandings** — where intent and interpretation diverged, and why.
4. **Unaddressed needs** — signals from the other person that the user missed or
   under-responded to.
5. **Positive connection points** — moments of genuine rapport, validation, or
   shared meaning.
6. **Response patterns** — recurring behaviors that strengthen the relationship
   vs. create friction (e.g., deflecting, over-explaining, one-upping, active
   listening).

## Step 4 — Generate Structured Markdown Report

Write the report to a `.md` file in the current working directory, named
`relationship-analysis-report.md`. Structure it as follows:

1. **Executive Summary** — 3–5 sentences: relationship stage, top 3 strengths,
   top 3 gaps, and the single highest-leverage next step.
2. **Relationship Baseline** — structured profile of the other person (from
   Step 2), with explicit note of any missing information.
3. **Key Response Analysis** — a table or per-item breakdown of the user's key
   responses in the chat, each with: quote/summary, context, and an assessment of
   whether it was **suitable / partially suitable / unsuitable** for the
   relationship, with a one-line reason tied to the baseline.
4. **Emotional Timeline** — notable interaction-by-interaction tone shifts,
   flagging any misalignment or escalation/de-escalation points.
5. **Misunderstandings, Unaddressed Needs & Connection Points** — three labeled
   sub-sections from Step 3.
6. **Communication Pattern Assessment** — a list of patterns that strengthen the
   relationship, and a list of patterns that create friction, each backed by at
   least one concrete example from the chat.
7. **Current Relationship Stage Summary** — data-backed summary: current stage,
   key strengths, key gaps.
8. **Actionable Recommendations** — specific, concrete changes to the user's
   communication approach. Each recommendation must be directly traceable to an
   observed gap and phrased as an action ("Do X instead of Y", "When Z happens,
   try …").
9. **Step-by-Step Roadmap** — an ordered, phased plan to advance the relationship
   from its current state, with clear milestones and checkpoints.

Formatting: use headings, bullet lists, and tables. Keep quotes short (paraphrase
long messages). Every claim must cite the source file section or line where
possible.

## Step 5 — Validation & Quality Checklist

Before finishing, verify:

- [ ] All parsing was local-only; no cloud service was used at any step.
- [ ] Both files were read successfully and in the correct format.
- [ ] The report includes all nine sections above.
- [ ] Every assessment references either the baseline or a concrete chat example.
- [ ] No fabricated content; uncertainties are labeled as inference.
- [ ] The report is written in the correct language and saved as
      `relationship-analysis-report.md`.
- [ ] Recommendations are specific and actionable (not generic advice).

If any checklist item fails, correct the report before delivering. Deliver a
brief summary of findings alongside the path to the generated report file.
