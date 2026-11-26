# Instructions for any coding agent

The CV workflow is the skill at `skills/tailor-cv/SKILL.md`.

When the user asks to tailor the CV for a job, or to update it after a change to `experiences.txt`, read that file and follow it before editing. If you cannot open other files, still obey this:

- The job is only in `description.txt`. If that file is still just the instruction comments, stop and ask for the posting.
- Facts are only in `experiences.txt`. Do not invent employers, dates, tools, metrics, or degrees.
- For a job, edit `main.tex` and `sections/*.tex` only.
- `cv.md` and `resume.json` are the machine-readable copies of the same facts. Tailoring for one job does not change them; a change to `experiences.txt` does.
- Compile with `xelatex -interaction=nonstopmode -halt-on-error main.tex` twice from the repo root.
- Do not restore the Doruk Çetin template CV.
