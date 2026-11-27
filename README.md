# Simple-CV

A bilingual English/Turkish XeLaTeX CV that an AI agent tailors for one job at a time, using only the experience recorded in this repository. The project also includes job-specific cover letters and machine-readable copies of the full CV record.

## Repository layout

```text
.
├── AGENTS.md                  # Agent entry point and guardrails
├── README.md
├── LICENSE
├── data/
│   ├── experiences.txt        # Only source of truth for CV facts
│   ├── description.txt        # Current target job posting
│   ├── cv.md                  # Complete machine-readable CV
│   └── resume.json            # Complete structured CV
├── source/
│   ├── cv-en.tex              # English CV entry point
│   ├── cv-tr.tex              # Turkish CV entry point
│   ├── simplecv.sty           # Shared layout and styling
│   ├── sections/
│   │   ├── en/                # English CV content
│   │   └── tr/                # Turkish CV content
│   └── letters/
│       ├── letter-en.tex      # Job-specific English cover letter
│       ├── letter-en.txt      # Plain-text copy
│       ├── letter-tr.tex      # Job-specific Turkish cover letter
│       └── letter-tr.txt      # Plain-text copy
├── output/                    # Generated PDFs and LaTeX auxiliary files
└── skills/
    └── tailor-cv/
        └── SKILL.md           # Instructions the agent follows
```

`data/experiences.txt` is the only source of employers, dates, roles, tools, metrics, and degrees. `data/description.txt` holds the one job currently being targeted. The two LaTeX CVs are tailored per job; `data/cv.md` and `data/resume.json` always hold the complete record.

Generated PDFs and auxiliary files are ignored by Git. The `output/` folder is kept in the repository so builds have a stable destination.

## Prerequisites

1. Install an XeLaTeX distribution: MiKTeX on Windows, MacTeX or TeX Live on macOS and Linux.
2. Install a system sans font. `Noto Sans` is the default in both CV entry points; `Rubik`, `Liberation Sans`, and `Carlito` also work.
3. Open this folder in an AI coding agent that loads `AGENTS.md` (Qoder, Cursor, Codex, Claude Code, and similar). `AGENTS.md` points it at the skill, so no further setup is needed.
4. Optionally install `pdftotext` if you want to run the text-extraction check described below.

`latexmk` needs Perl and is not used here.

## Build the documents

Run these commands from the repository root. Each document is compiled twice so page references and layout settle.

### English and Turkish CVs

```bash
cd source
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-en.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-en.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-tr.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-tr.tex
cd ..
```

The CVs are A4 documents and should remain within two pages each.

### Cover letters

```bash
cd source
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output letters/letter-en.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output letters/letter-en.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output letters/letter-tr.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output letters/letter-tr.tex
cd ..
```

The included letters are specific to the posting currently in `data/description.txt`. Update or replace them for each new application; the CV-tailoring skill does not rewrite them unless asked.

## Tailor the CV to a job

1. Paste the full posting into `data/description.txt`, under its comment header, replacing the previous job.
2. Tell the agent: `Tailor my CV for this job.` For a model that cannot see the folder, send `Follow skills/tailor-cv/SKILL.md` with the contents of `data/experiences.txt`, `data/description.txt`, `source/cv-en.tex`, `source/cv-tr.tex`, and both `source/sections/` language folders.
3. The agent selects the roles, projects, and skills that answer the posting, rewrites both language versions with the same emphasis, compiles them, and reports what it kept and what it dropped.
4. Open `output/cv-en.pdf` and `output/cv-tr.pdf`. If the emphasis is wrong, say what to stress and let it rebuild; the facts do not change.

What the skill refuses to do:

- Add an employer, date, tool, metric, or degree that `data/experiences.txt` does not state. Docker does not justify Kubernetes.
- Borrow wording from the posting unless the recorded experience already supports it.
- Rewrite `data/experiences.txt` or `data/description.txt`.
- Change the complete machine-readable record for a single job.
- Go past two pages, or start when `data/description.txt` still contains only its comment header. In that case it stops and asks for the posting.

## When your history changes

1. Add or correct the role in `data/experiences.txt`, including city, dates, tools, and any number you can defend in an interview.
2. Tell the agent: `Update the CV files from data/experiences.txt.`
3. It refreshes both `source/sections/` language folders, `data/cv.md`, and `data/resume.json` together, then recompiles both CV PDFs.

To check that the English PDF still has a clean text layer, run this from `source/`:

```bash
pdftotext -enc UTF-8 ../output/cv-en.pdf - | grep -c "Munich, Germany (remote)"
```

The expected count is `1`. Zero means the font or layout broke extraction.

## Outputs

| File | Who it is for |
| --- | --- |
| `output/cv-en.pdf` | Recruiters and hiring managers. English, up to two A4 pages |
| `output/cv-tr.pdf` | Recruiters and hiring managers. Turkish, up to two A4 pages |
| `output/letter-en.pdf` | English cover letter for the current application |
| `output/letter-tr.pdf` | Turkish cover letter for the current application |
| `data/cv.md` | Applicant forms and AI chat assistants that read text instead of a PDF. Standard section names, full URLs, `Nov 2023 - Sep 2024` dates |
| `data/resume.json` | Structured parsers. One entry per role, project, and degree, with `highlights` arrays and `YYYY-MM` dates |

## Layout switches

The main options are at the top of both `source/cv-en.tex` and `source/cv-tr.tex`:

- `\theme` for the accent color: `black`, `RedViolet`, `ForestGreen`, `MidnightBlue`, `Blue`.
- `\cvfont` for the typeface. Leave it empty for the Computer Modern serif. Avoid `Segoe UI`: its PDF text layer maps parentheses to Arabic ligature code points, so `(remote)` stops extracting correctly.
- `\documentclass[a4paper,11pt]` for paper size and body size. Switch to `letterpaper` for US applications.
- The headline under the name is the optional first argument of `\headinginline`.

Shared macros and styling live in `source/simplecv.sty`.

## License

The code is under [the MIT license](https://opensource.org/licenses/MIT). The CV data stays mine.
