# Instructions for any coding agent

The CV workflow is the skill at `skills/tailor-cv/SKILL.md`.

When the user asks to tailor the CV for a job, or to update it after a change to `data/experiences.txt`, read that skill and follow it before editing. If you cannot open other files, still obey this:

- The job is only in `data/description.txt`. If that file is still just the instruction comments, stop and ask for the posting.
- Facts are only in `data/experiences.txt`. Do not invent employers, dates, tools, metrics, or degrees.
- For a job, edit `source/cv-en.tex`, `source/cv-tr.tex`, `source/sections/en/*.tex`, and `source/sections/tr/*.tex` only. Keep the English and Turkish versions aligned.
- `data/cv.md` and `data/resume.json` are the complete machine-readable copies of the same facts. Tailoring for one job does not change them; a change to `data/experiences.txt` does.
- Compile both CVs twice from `source/` with `-output-directory=../output`: `cv-en.tex` and `cv-tr.tex`.
- Do not restore the Doruk Çetin template CV.
