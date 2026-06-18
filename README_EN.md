# Job Research Report Skill

[![Codex Skill](https://img.shields.io/badge/Codex-Skill-6366f1)](https://github.com/openai/codex)
[![Python](https://img.shields.io/badge/Python-3.8+-3776ab)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

A Codex Skill that researches a company`s job position via **web search** and automatically generates a professionally formatted A4 PDF report.

## Features

-   **One-click research** — Just provide a company name and position title to kick off the full workflow
-   **Web search powered** — Multi-round search covers company overview, JD breakdown, salary data, and skill requirements
-   **Professional PDF** — Standard A4 layout with Chinese document formatting (SimSong body, SimHei headings)
-   **Flexible sources** — Codex auto-selects optimal search sources by default; specific sites can be pinned by the user
-   **Structured tables** — Salary data presented in tables by city, experience level, and job rank
-   **Citations** — References auto-recorded in GB/T 7714 format

## Report Contents

| Section | Content |
|------|------|
| Cover | Title, company · position, research date |
| I. Company Overview | (1) Industry — (2) Scale — (3) Development Stage — (4) Culture |
| II. Job Description | Summary, duties, requirements |
| III. Salary Range | By city, by experience, by rank (with tables) |
| IV. Skills | Hard skills, soft skills |
| References | All search sources cited |

## Quick Start

### Prerequisites

- Python 3.8+
- [Codex CLI](https://github.com/openai/codex) installed

### Install the Skill

1. Copy the `job-research-report` directory into `$CODEX_HOME/skills/`:

   ```bash
   cp -r job-research-report ~/.codex/skills/
   ```

2. Restart Codex — the skill auto-registers on launch.

### Usage

Type any of the following in Codex:

```text
Research the Product Manager position at ByteDance, generate a PDF report

Look up backend developer salaries at Tencent and output a research report in PDF

Research the User Operations role at Xiaohongshu, limit search to BOSS Zhipin, generate a report
```

Codex will automatically:

1. Parse the company name and position
2. Perform multi-round web search
3. Build structured JSON data
4. Call `scripts/generate_report.py` to produce the PDF
5. Output: `{Company}_{Position}_调研报告.pdf`

## Project Structure

```
job-research-report/
│
├── SKILL.md                       # Core skill instructions (workflow definition)
├── agents/
│   └── openai.yaml               # Skill UI metadata
└── scripts/
    ├── generate_report.py         # PDF report generator (ReportLab)
    └── sample.json               # Sample input data
```

## PDF Formatting

| Property | Spec |
|------|------|
| Paper | A4 |
| Margins | Top 3.7cm / Bottom 3.5cm / Left 2.8cm / Right 2.8cm |
| Report title | 22pt SimHei (bold), centered |
| Section headers | 16pt SimHei, left-aligned |
| Body text | 12pt SimSong, first-line indent 2 chars, 1.5× spacing |
| Page numbers | 9pt Times New Roman, bottom center |

## Customization

### Tweak PDF Styles

Edit style constants in `scripts/generate_report.py`:

```python
SZ_TITLE = 22      # Cover title font size
SZ_H1 = 16         # H1 font size
SZ_BODY = 12       # Body font size
TOP_M = 3.7 * cm   # Top margin
```

### Add New Data Dimensions

1. Add corresponding search steps in `SKILL.md`
2. Extend the JSON schema with new fields
3. Add rendering logic in the `generate()` function of `generate_report.py`

## FAQ

**Q: Chinese characters appear garbled in the PDF?**
A: Ensure SimSun (宋体) and SimHei (黑体) fonts are installed. These are pre-installed on Windows.

**Q: Salary data seems inaccurate?**
A: Salary data comes from real-time web search and may fluctuate. The research date is noted on the report cover for reference.

**Q: Can I pin a specific search source?**
A: Yes. Specify the website in your prompt, e.g. "Limit search to BOSS Zhipin".

## Tech Stack

- [ReportLab](https://www.reportlab.com/) — Python PDF generation library
- Codex Skill Framework — Skill runtime environment

## License

MIT
