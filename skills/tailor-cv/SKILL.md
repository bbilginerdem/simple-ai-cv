---
name: tailor-cv
description: >-
  Rewrites the English and Turkish XeLaTeX CVs and cover letters for one job
  posting. Use when the user asks to tailor, adapt, match, rewrite, or update
  the CV, resume, application, letter of intent, cover letter, or output files
  for a job or data/description.txt, or when data/experiences.txt changes and
  the CVs, letters, data/cv.md, and data/resume.json must be brought back in
  sync. Facts come only from data/experiences.txt.
---

# Tailor the CV and cover letters

Facts are in `data/experiences.txt`. The target job is in `data/description.txt`. The CV documents are `source/cv-en.tex` and `source/cv-tr.tex`, with section files in `source/sections/en/` and `source/sections/tr/`. The cover-letter documents are `source/letters/letter-en.tex` and `source/letters/letter-tr.tex`, with matching plain-text copies at `source/letters/letter-en.txt` and `source/letters/letter-tr.txt`. Outputs are `output/cv-en.pdf`, `output/cv-tr.pdf`, `output/letter-en.pdf`, and `output/letter-tr.pdf`.

A request to tailor the CV, outputs, application, letter of intent, or cover letter—or a changed `data/description.txt`—means all four job-facing PDFs are stale. Update the English and Turkish CVs and the English and Turkish cover letters together.

`data/cv.md` and `data/resume.json` are the complete machine-readable record of every fact. They are not tailored to one job and are not rewritten here; update them only when `data/experiences.txt` changes.

## Steps

1. Read `data/description.txt`. If the only text is the instruction comments, stop and tell the user to paste the job under the header. Do not invent a target role.
2. Read `data/experiences.txt`. That file is the only source of employers, dates, tools, metrics, and degrees. Select, shorten, and reorder. Do not add a fact that is not written there.
3. Rewrite `source/sections/en/*.tex`, `source/sections/tr/*.tex`, and the headings and metadata in `source/cv-en.tex` and `source/cv-tr.tex` when the job calls for a different emphasis.
   - Keep the same selection and emphasis in both languages; translate faithfully rather than adding new claims.
   - Put the matching roles, projects, and skills first.
   - Use words from the job posting only when `data/experiences.txt` already supports them.
   - Keep each CV to one or two pages.
   - Keep these commands: `\headinginline`, `\entrybig`, `\entry`, `\innerlist`, `\denseouterlist`. `\headinginline` takes the headline as its optional argument.
   - Write English dates as `Nov 2023 -- Sep 2024` and countries in full (`Munich, Germany`). Keep the established Turkish date and location style in the Turkish files.
   - `\entrybig` bolds the organization itself, so write `{NexArb}` not `{\textbf{NexArb}}`.
   - In LaTeX, write `\%`, `\&`, and `\_` for those characters.
4. Rewrite both cover-letter `.tex` files and both plain-text `.txt` copies for the same posting.
   - Keep the English and Turkish letters aligned and each to one page.
   - Use the employer, role, and responsibilities stated in `data/description.txt`. Do not invent a recipient name, city, postal address, or department that the posting does not state.
   - Reuse the same supported selection and emphasis chosen for the CVs. A letter may explain relevance, but it may not assert an unrecorded skill, certification, tool, or amount of role-specific experience.
   - Keep the plain-text copies consistent with the `.tex` letters in substance.
5. Do not edit `data/experiences.txt` or `data/description.txt`.
6. From `source/`, remove generated files from `output/` except `.gitkeep`, then compile every document twice. `latexmk` needs Perl and is not used.

```bash
find ../output -maxdepth 1 -type f ! -name '.gitkeep' -delete
for doc in cv-en cv-tr letters/letter-en letters/letter-tr; do
  xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output "$doc.tex"
  xelatex -interaction=nonstopmode -halt-on-error -output-directory=../output "$doc.tex"
done
```

7. Reply with the roles and projects you kept, the ones you left out, and the paths `output/cv-en.pdf`, `output/cv-tr.pdf`, `output/letter-en.pdf`, and `output/letter-tr.pdf`.

## Do not

- Do not restore the template CV for Doruk Çetin. That is not this person's experience.
- Do not name a tool, certificate, degree year, metric, or location that `data/experiences.txt` or `data/description.txt` does not state. Docker does not justify Kubernetes. "Three Oracle certifications" does not justify naming the certificates.
- Do not convert overall years of software experience into years of experience in a specific requested discipline. Five-plus years of software development is not automatically five-plus years of database administration.
- Do not leave a previous employer, role, date, subject line, or body paragraph in any output.
- Do not add a home location, a postal code, or education years. `data/experiences.txt` records none.

## When `data/experiences.txt` changes

No job is involved, so nothing is selected or dropped. Bring every copy back in line with the file:

1. Rewrite both `source/sections/en/*.tex` and `source/sections/tr/*.tex` so the PDFs carry the new or corrected facts, newest first.
2. Rewrite `data/cv.md` and `data/resume.json` with the same facts. They hold every role, project, and degree.
3. Add newly used tools to the matching lines in both `skills.tex` files, `data/cv.md`, and `data/resume.json`, but only when a role or project in `data/experiences.txt` says they were used.
4. If the current cover letters cite changed facts, update both `.tex` and `.txt` letter versions as well.
5. Clean `output/`, compile all four documents twice, then confirm the English CV text layer still extracts, expecting one line back:

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
