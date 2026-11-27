# Instructions for any coding agent

The CV and cover-letter workflow is the skill at `skills/tailor-cv/SKILL.md`.

When the user asks to tailor outputs for a job, or to update them after a change to `data/experiences.txt`, read that skill and follow it before editing. If you cannot open other files, still obey this:

- The job is only in `data/description.txt`. If that file is still just the instruction comments, stop and ask for the posting.
- Facts are only in `data/experiences.txt`. Do not invent employers, dates, tools, metrics, degrees, certifications, locations, or role-specific years of experience.
- A changed `data/description.txt` or a request to "tailor my outputs" makes all four job-facing source documents stale—both CVs and both cover letters—and every generated PDF and TXT deliverable in `output/`.
- For a job, edit `source/cv-en.tex`, `source/cv-tr.tex`, `source/sections/en/*.tex`, `source/sections/tr/*.tex`, `source/letters/letter-en.tex`, `source/letters/letter-en.txt`, `source/letters/letter-tr.tex`, and `source/letters/letter-tr.txt`. Keep the English and Turkish versions aligned.
- `data/cv.md` and `data/resume.json` are the complete machine-readable copies of the same facts. Tailoring for one job does not change them; a change to `data/experiences.txt` does.
- Remove generated files from `output/` except `.gitkeep`, then compile all four documents twice from `source/` with `-output-directory=../output`: `cv-en.tex`, `cv-tr.tex`, `letters/letter-en.tex`, and `letters/letter-tr.tex`. After compiling, create `output/cv-en.txt` and `output/cv-tr.txt` with `pdftotext -enc UTF-8 -raw -nopgbrk`, filtering out `Page X of Y` lines, and copy `source/letters/letter-en.txt` and `source/letters/letter-tr.txt` into `output/`.
- Do not restore the Doruk Çetin template CV.
