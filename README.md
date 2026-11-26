# Simple-CV

A CV that an AI agent rewrites for one job at a time, using only the experience you have recorded. You supply two text files; the agent produces the PDF and the machine-readable copies.

## How it works

Three files drive everything:

| File | What it holds | Who edits it |
| --- | --- | --- |
| `experiences.txt` | Every fact: employers, cities, dates, roles, tools, metrics, degrees | You. It is the only source of truth |
| `description.txt` | The one job posting you are answering right now | You. Replace it for each job |
| `skills/tailor-cv/SKILL.md` | The rules an agent follows while rewriting the CV | Rarely |

Everything else is generated from those three: `main.tex` and `sections/*.tex` (the document), `main.pdf` (the output), `cv.md` and `resume.json` (the machine-readable record). Hand-edit them only to change layout or wording, never to add a fact.

## Set up once

1. Install an XeLaTeX distribution: MiKTeX on Windows, MacTeX or TeX Live on macOS and Linux.
2. Install a system sans font. `Noto Sans` is the default in `main.tex`; `Rubik`, `Liberation Sans`, and `Carlito` also work.
3. Open this folder in an AI coding agent that loads `AGENTS.md` (Qoder, Cursor, Codex, Claude Code, and similar). `AGENTS.md` points it at the skill, so no further setup is needed.
4. Check the toolchain from the repo root, twice so the page count settles:

```bash
xelatex -interaction=nonstopmode -halt-on-error main.tex
xelatex -interaction=nonstopmode -halt-on-error main.tex
```

You should get a two-page `main.pdf`. `latexmk` needs Perl and is not used here.

## Tailor the CV to a job

1. Paste the full posting into `description.txt`, under its comment header, replacing the previous job.
2. Tell the agent: `Tailor my CV for this job.` For a model that cannot see the folder, send `Follow skills/tailor-cv/SKILL.md` with the contents of `experiences.txt`, `description.txt`, `main.tex`, and `sections/`.
3. The agent reads the posting, selects the roles, projects, and skills that answer it, rewrites `sections/*.tex`, compiles, and reports what it kept and what it dropped.
4. Open `main.pdf`. If the emphasis is wrong, say what to stress and let it rebuild; the facts do not change.

What the skill refuses to do:

- Add an employer, date, tool, metric, or degree that `experiences.txt` does not state. Docker does not justify Kubernetes.
- Borrow wording from the posting unless your recorded experience already supports it.
- Rewrite `experiences.txt` or `description.txt`.
- Go past two pages, or start when `description.txt` still contains only its comment header. In that case it stops and asks for the posting.

## When your history changes

1. Add or correct the role in `experiences.txt`, including city, dates, tools, and any number you can defend in an interview.
2. Tell the agent: `Update the CV files from experiences.txt.`
3. It refreshes `sections/*.tex`, `cv.md`, and `resume.json` together and recompiles `main.pdf`.

## Outputs

| File | Who it is for |
| --- | --- |
| `main.pdf` | Recruiters and hiring managers. Two A4 pages |
| `cv.md` | Applicant forms and AI chat assistants that read text instead of a PDF. Standard section names, full URLs, `Nov 2023 - Sep 2024` dates |
| `resume.json` | Structured parsers. One entry per role, project, and degree, with `highlights` arrays and `YYYY-MM` dates |

`cv.md` and `resume.json` hold the complete record and are not tailored per job; the PDF is.

## Layout switches

All at the top of `main.tex`:

- `\theme` for the accent color: `black`, `RedViolet`, `ForestGreen`, `MidnightBlue`, `Blue`.
- `\cvfont` for the typeface. Leave it empty for the Computer Modern serif. Avoid `Segoe UI`: its PDF text layer maps parentheses to Arabic ligature code points, so `(remote)` stops extracting correctly.
- `\documentclass[a4paper,11pt]` for paper size and body size. Switch to `letterpaper` for US applications.
- The headline under the name is the optional first argument of `\headinginline` in `main.tex`.

## License

The code is under [the MIT license](https://opensource.org/licenses/MIT). The CV data stays mine.
