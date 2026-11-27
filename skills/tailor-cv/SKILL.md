---
name: tailor-cv
description: >-
  Rewrites the English and Turkish XeLaTeX CVs for one job posting. Use when
  the user asks to tailor, adapt, match, rewrite, or update the CV, resume, or
  LaTeX for a job or data/description.txt, or when data/experiences.txt changes
  and the CV, data/cv.md, and data/resume.json must be brought back in sync.
  Facts come only from data/experiences.txt.
---

# Tailor the CV

Facts are in `data/experiences.txt`. The target job is in `data/description.txt`. The documents are `source/cv-en.tex` and `source/cv-tr.tex`, with section files in `source/sections/en/` and `source/sections/tr/`. Outputs are `output/cv-en.pdf` and `output/cv-tr.pdf`.

`data/cv.md` and `data/resume.json` are the complete machine-readable record of every fact. They are not tailored to one job and are not rewritten here; update them only when `data/experiences.txt` changes.

## Steps

1. Read `data/description.txt`. If the only text is the instruction comments, stop and tell the user to paste the job under the header. Do not invent a target role.
2. Read `data/experiences.txt`. That file is the only source of employers, dates, tools, metrics, and degrees. Select, shorten, and reorder. Do not add a fact that is not written there.
3. Rewrite `source/sections/en/*.tex`, `source/sections/tr/*.tex`, and the headings in `source/cv-en.tex` and `source/cv-tr.tex` when the job calls for a different emphasis.
   - Keep the same selection and emphasis in both languages; translate faithfully rather than adding new claims.
   - Put the matching roles, projects, and skills first.
   - Use words from the job posting only when `data/experiences.txt` already supports them.
   - Keep each CV to one or two pages.
   - Keep these commands: `\headinginline`, `\entrybig`, `\entry`, `\innerlist`, `\denseouterlist`. `\headinginline` takes the headline as its optional argument.
   - Write English dates as `Nov 2023 -- Sep 2024` and countries in full (`Munich, Germany`). Keep the established Turkish date and location style in the Turkish files.
   - `\entrybig` bolds the organization itself, so write `{NexArb}` not `{\textbf{NexArb}}`.
   - In LaTeX, write `\%`, `\&`, and `\_` for those characters.
4. Do not edit `data/experiences.txt` or `data/description.txt`.
5. From `source/`, compile each CV twice. `latexmk` needs Perl and is not used.

```bash
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-en.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-en.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-tr.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output cv-tr.tex
```

6. Reply with the roles and projects you kept, the ones you left out, and the paths `output/cv-en.pdf` and `output/cv-tr.pdf`.

## Do not

- Do not restore the template CV for Doruk Çetin. That is not this person's experience.
- Do not name a tool, certificate, degree year, or metric that `data/experiences.txt` does not state. Docker does not justify Kubernetes. "Three Oracle certifications" does not justify naming the certificates.
- Do not add a home location, a postal code, or education years. `data/experiences.txt` records none.
- Do not rewrite the job-specific cover letters in `source/letters/` unless the user asks.

## When `data/experiences.txt` changes

No job is involved, so nothing is selected or dropped. Bring every copy back in line with the file:

1. Rewrite both `source/sections/en/*.tex` and `source/sections/tr/*.tex` so the PDFs carry the new or corrected facts, newest first.
2. Rewrite `data/cv.md` and `data/resume.json` with the same facts. They hold every role, project, and degree.
3. Add newly used tools to the matching lines in both `skills.tex` files, `data/cv.md`, and `data/resume.json`, but only when a role or project in `data/experiences.txt` says they were used.
4. Compile both CVs twice, then confirm the English text layer still extracts, expecting one line back:

```bash
pdftotext -enc UTF-8 ../output/cv-en.pdf - | grep -c "Munich, Germany (remote)"
```

Zero means the font or layout broke extraction. This has happened before: `Segoe UI` maps parentheses to Arabic ligature code points in the PDF text layer.

## Bullet shape

```latex
\entrybig
{NexArb}{Munich, Germany (remote)}
{Software Developer}
{Nov 2023 -- Sep 2024}
\innerlist{
  \entry{Resolved 12 API bottlenecks against a FastAPI backend and cut latency by 35\%}
}
```
