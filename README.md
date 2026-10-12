# Papers-Analyzer

English | [中文](README.zh-CN.md)

An AI skill that turns research paper PDFs into a **structured Excel table** in one shot: upload one or more papers, and it extracts key information paper-by-paper — one row per paper, a fixed 15-column bilingual (Chinese/English) header, clickable links, ready to read.

## What problem this solves

When doing a literature review, writing a thesis proposal, or preparing for a group meeting, the usual pain points are:

- You have to read a dozen papers one by one and manually copy title / year / venue / method / link into a comparison table — very time-consuming;
- Hand-filled tables are error-prone: conclusions from one paper end up in another's row, years and venues get mixed up, links get wrong;
- Different people organize the same papers into tables with different column names and levels of detail, making cross-paper comparison impossible.

Papers-Analyzer standardizes this: a fixed 15-column schema, one row per paper, and every cell must be traceable to the paper's own text. If a piece of information is not in the paper, the cell stays empty — never fabricated.

## Features

- **Fixed 15-column extraction** (row 1 = Chinese header, row 2 = English header, data starts at row 3):

| # | Column | Content |
|---|--------|---------|
| 1 | Paper Title | Original title as printed in the paper |
| 2 | Year | Publication year (conference year for conference papers) |
| 3 | Venue | Journal or conference name as printed (left empty if it's an arXiv preprint) |
| 4 | Link | URL to the paper itself (arXiv/DOI/publisher; empty if not stated in the PDF) |
| 5 | Research Objectives | What the paper aims to solve, grouped by theme |
| 6 | Motivations | Limitations of existing methods and the research gap, listed as Motivation#N |
| 7 | Key Innovations and Why | Each innovation numbered (a1. a2.…) with a "Why:" rationale |
| 8 | Method: Pros & Cons | Numbered list of strengths and weaknesses, each backed by the paper |
| 9 | Article Structure | The paper's real section outline, one sentence per section |
| 10 | Model Architecture | Text description of model components (no diagram extraction) |
| 11 | Model Diagram | Left empty on purpose — paste your own screenshot here (width 80) |
| 12 | Open Source Code | Repository URL stated by the paper, if any |
| 13 | Dataset Link | Dataset names used in experiments (URL attached if the paper gives one) |
| 14 | Future Development Directions | Natural extensions of the proposed method (prefer the authors' own Future Work) |
| 15 | Proposed Innovations Based on This Paper | Cross-paradigm / cross-task ideas grounded in the paper's stated limitations |

- **Batch papers into one table**: process multiple papers at once, all in a single `.xlsx`, one row per paper, in the order you provide;
- **Output language follows you**: the header is always bilingual two-row; analysis cells follow the language of your prompt (Chinese prompt → Chinese cells, English prompt → English cells). Titles, venue names, and URLs stay in the paper's original language;
- **Excel ready to read**: link and code columns are clickable hyperlinks; Latin text uses Times New Roman, Chinese uses SimSun (宋体).
- **Three iron rules**:
  1. Only fill what the paper actually says — every cell must be traceable;
  2. Leave empty if not found — no fabrication, no guessing, no "N/A" placeholders;
  3. Never pad the count — if the paper has one innovation, write one row.

**Not supported**: extracting the model architecture **diagram image**. You screenshot it yourself from the paper; the table only contains a text description. Scanned/image PDFs need OCR first — out of scope.

## Installation

This project is an **AI skill**, not a CLI tool. No `pip install`, no Git required:

1. Copy the whole `Papers-Analyzer/` folder into your AI assistant's skill directory (e.g. `.user_skills/`, or the equivalent folder on a platform that supports Skills);
2. The runtime needs **Python 3.8+** with `openpyxl` (writes Excel) and either `pdfplumber` or `PyMuPDF` (extracts PDF text):

   ```bash
   pip install openpyxl pdfplumber
   ```

3. No further configuration. Just start asking as described below.

Directory layout:

```
Papers-Analyzer/
├── SKILL.md               # Skill definition and full workflow
└── agents/
    └── openai.yaml        # Agent manifest (display name, default prompt)
```

## Usage

Once the skill is in place, just describe the task in chat — no commands to remember.

**Option 1: upload PDF files**

```
Turn these papers into an Excel table
(attach the PDF files)
```

**Option 2: give a paper URL**

```
Extract the paper at https://.../paper.pdf into a 15-column table
```

**Option 3: Chinese prompt → Chinese cells**

```
帮我把这几篇论文整理成 Excel 表格
```

The AI follows the workflow in `SKILL.md`: read PDF → extract column by column → generate Excel → verify every cell against the source before delivery.

**Input**: one or more PDF files (local paths), or direct PDF links (publisher site, arXiv, etc.). Text-based PDFs only.

**Output**: a single `.xlsx` file, bilingual header, one row per paper. English prompts produce `paper_info_table_YYYYMMDD.xlsx`; Chinese prompts produce `论文信息表_YYYYMMDD.xlsx`.

## Project structure

```
.
├── README.md                        # This file (English)
├── README.zh-CN.md                  # Chinese version
├── LICENSE                          # MIT
└── Papers-Analyzer/                 # The skill itself
    ├── SKILL.md                     # Skill definition and workflow
    └── agents/
        └── openai.yaml              # Agent manifest
```

## License

MIT
