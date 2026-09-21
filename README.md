# Relationship Analysis Skill

[简体中文](README.zh.md) | English

---

## Overview

The **Relationship Analysis Skill** is a local, privacy-first AI skill that helps
you understand and improve your relationship with another person. It reads two
files you provide entirely on your own machine — a **relationship context file**
and a **chat history file** — then generates a detailed, actionable Markdown
report.

All parsing, reasoning, and report generation are performed locally. Your
personal data is never uploaded to the cloud.

---

## Key Features

1. **Local File Parsing & Compatibility**
   - Reads and parses **PDF**, **TXT**, **DOCX**, and **CSV** files locally.
   - Supports the two required inputs:
     - A relationship context file describing your relationship.
     - A chat history file containing your interactions.

2. **Contextual Baseline Analysis**
   - Extracts key relationship context: the other person's traits, personality,
     communication style, values, emotional needs, boundaries, and your stated
     goals.
   - Builds a structured baseline used to evaluate every interaction.

3. **Deep Chat History Analysis**
   - Cross-references the chat history against the baseline.
   - Evaluates style alignment, emotional tone, misunderstandings, unaddressed
     needs, positive connection points, and recurring response patterns that
     strengthen or weaken the relationship.

4. **Structured Markdown Report Generation**
   - Produces `relationship-analysis-report.md` with:
     - Executive summary
     - Relationship baseline
     - Line-item analysis of your key responses
     - Emotional timeline
     - Misunderstandings, unaddressed needs, and connection points
     - Communication pattern assessment
     - Current relationship stage summary
     - Specific, actionable recommendations
     - Step-by-step roadmap to strengthen the relationship

5. **Technical Validation & Local-Only Processing**
   - Enforces strict local-only data handling.
   - Includes a validation checklist to ensure completeness, accuracy, and
     absence of fabricated content.

---

## Privacy Guarantee

- **No cloud upload:** All file reading, parsing, and analysis run on your local
  machine. No data is sent to external APIs, cloud services, or third-party
  servers.
- **No data retention:** Extracted content is used only to generate the report
  and is not persisted elsewhere.
- **Summarization over exposure:** The report summarizes interactions rather
  than reproducing full raw chat logs.

---

## Supported File Formats

| Format | Local Parsing Method |
|--------|----------------------|
| `TXT` / `MD` | Direct text read; supports UTF-8, UTF-16, GBK/GB2312 |
| `CSV` | Local pandas/Python parser; auto-detects timestamp/sender/message columns |
| `PDF` | `pdftotext` (Poppler) or Python libraries (`pdfplumber`, `PyPDF2`) |
| `DOCX` | `python-docx` or direct `word/document.xml` extraction |

If a file is password-protected, corrupted, or in an unsupported format, the
skill will stop and ask you to provide a readable version.

---

## Required Inputs

1. **Relationship context file** — your description of the relationship, the
   other person's background, personality, communication preferences, values,
   goals, and any known sensitivities.
2. **Chat history file** — a chronological record of interactions between you
   and the other person.

Both files are required. Provide them when invoking the skill.

---

## How It Works

The skill runs a five-step pipeline:

1. **Parse Local Files** — detect format, extract text, preserve chronological
   order, and validate both files.
2. **Build Contextual Baseline** — structure the relationship context into a
   profile of the other person and explicit knowledge gaps.
3. **Analyze Chat History** — evaluate each major interaction against the
   baseline across six dimensions:
   - Style alignment
   - Emotional tone consistency
   - Misunderstandings
   - Unaddressed needs
   - Positive connection points
   - Strengthening vs. friction patterns
4. **Generate Markdown Report** — write a comprehensive, structured report named
   `relationship-analysis-report.md`.
5. **Validate Output** — run a quality checklist before delivering results.

---

## Usage

1. Place the skill file in your project:

   ```text
   relationship-analysis-skill\SKILL.md
   ```

2. Provide the two required files, for example:

   ```text
   relationship-analysis-skill\evidence_list\context.txt

   relationship-analysis-skill\evidence_list\chat_history.csv
   or
   relationship-analysis-skill\evidence_list\chat_history.txt
   ```

3. Invoke the skill in your AI assistant and ask for a relationship analysis,
   e.g.:

   ```text
   Analyze my relationship using context.txt and chat_history.csv.
   ```

4. The skill will generate:

   ```text
   relationship-analysis-skill\relationship-analysis-report.md
   ```

---

## Report Output

The generated report contains the following sections:

1. **Executive Summary** — relationship stage, top strengths, top gaps, and the
   highest-leverage next step.
2. **Relationship Baseline** — structured profile and explicit missing data.
3. **Key Response Analysis** — line-by-line assessment of your major responses
   (suitable / partially suitable / unsuitable), with reasons tied to the
   baseline.
4. **Emotional Timeline** — tone shifts and alignment points across the chat.
5. **Misunderstandings, Unaddressed Needs & Connection Points** — three labeled
   sub-sections with concrete examples.
6. **Communication Pattern Assessment** — behaviors that strengthen vs. create
   friction in the relationship.
7. **Current Relationship Stage Summary** — data-backed stage, strengths, and
   gaps.
8. **Actionable Recommendations** — specific changes to your communication
   approach, each traced to an observed gap.
9. **Step-by-Step Roadmap** — phased plan with milestones to advance the
   relationship.

---

## Dependencies

Most parsing can be done with built-in tools. For best results, you may install
local Python packages (no network required after installation):

```bash
pip install pandas python-docx pdfplumber PyPDF2
```

For PDF text extraction, Poppler's `pdftotext` is preferred when available.

---

## Validation & Quality Assurance

Before the report is finalized, the skill verifies:

- [ ] All parsing is local-only.
- [ ] Both required files are read successfully.
- [ ] The report contains all nine required sections.
- [ ] Every assessment references the baseline or a concrete chat example.
- [ ] No content is fabricated; uncertain claims are labeled as inferences.
- [ ] The report is written in the correct language.
- [ ] Recommendations are specific and actionable.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| File format unsupported | Convert to PDF, TXT, DOCX, or CSV. |
| Text appears garbled | Re-save the file as UTF-8. |
| CSV columns not recognized | Rename columns to `timestamp`, `sender`, `message`, or tell the skill the exact column names. |
| PDF cannot be read | Try `pdftotext` or install `pdfplumber`. |
| Report seems generic | Provide a more detailed relationship context file. |

---

## License

This skill is provided as-is for local relationship analysis. Use it in
accordance with your own privacy and data-handling policies.
