---
name: tailor-cv
description: >-
  Rewrites this XeLaTeX CV for one job posting. Use when the user asks to
  tailor, adapt, match, rewrite, or update the CV, resume, or LaTeX for a job,
  role, or description.txt. Facts come only from experiences.txt.
---

# Tailor the CV

Facts are in `experiences.txt`. The target job is in `description.txt`. The document is `main.tex` and `sections/*.tex`. Output is `main.pdf`.

## Steps

1. Read `description.txt`. If the only text is the instruction comments, stop and tell the user to paste the job under the header. Do not invent a target role.
2. Read `experiences.txt`. That file is the only source of employers, dates, tools, metrics, and degrees. Select, shorten, and reorder. Do not add a fact that is not written there.
3. Rewrite `sections/*.tex` and the heading in `main.tex` when the job calls for a different emphasis.
   - Put the matching roles, projects, and skills first.
   - Use words from the job posting only when `experiences.txt` already supports them.
   - Keep the CV to one or two pages.
   - Keep these commands: `\headinginline`, `\entrybig`, `\entry`, `\innerlist`, `\denseouterlist`.
   - In LaTeX, write `\%`, `\&`, and `\_` for those characters.
4. Do not edit `experiences.txt` or `description.txt`.
5. From the repo root, compile twice. `latexmk` needs Perl and is not available.

```bash
xelatex -interaction=nonstopmode -halt-on-error main.tex
xelatex -interaction=nonstopmode -halt-on-error main.tex
```

6. Reply with the roles and projects you kept, the ones you left out, and the path `main.pdf`.

## Do not

- Do not restore the template CV for Doruk Çetin. That is not this person's experience.
- Do not name a tool, certificate, degree year, or metric that `experiences.txt` does not state. Docker does not justify Kubernetes. "Three Oracle certifications" does not justify naming the certificates.

## Bullet shape

```latex
\entrybig
{\textbf{NexArb}}{Munich, DE (remote)}
{Software Developer}
{2023/11 -- 2024/09}
\innerlist{
  \entry{Resolved 12 API bottlenecks against a FastAPI backend and cut latency by 35\%}
}
```
