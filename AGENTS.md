# Instructions for any coding agent

The CV workflow is the skill at `skills/tailor-cv/SKILL.md`.

When the user asks to tailor, adapt, match, rewrite, or update the CV for a job, read that file and follow it before editing. If you cannot open other files, still obey this:

- The job is only in `description.txt`. If that file is still just the instruction comments, stop and ask for the posting.
- Facts are only in `experiences.txt`. Do not invent employers, dates, tools, metrics, or degrees.
- Edit `main.tex` and `sections/*.tex` only.
- Compile with `xelatex -interaction=nonstopmode -halt-on-error main.tex` twice from the repo root.
- Do not restore the Doruk Çetin template CV.
