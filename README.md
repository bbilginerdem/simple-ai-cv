
# Simple-CV

XeLaTeX CV for Behzat Bilgin Erdem. Layout is based on the Simple-CV template.

## Tailor it to a job

1. Paste the job description into `description.txt`, under the header. Delete the previous job first.
2. Leave `experiences.txt` as the fact list. Add a new role there when your history changes. Do not put the job description in that file.
3. Open this folder in the agent and say:

```
Follow skills/tailor-cv/SKILL.md
```

4. Open `main.pdf`.

Tools that load `AGENTS.md` (Cursor, Codex, and others) already know to follow that skill. A local model that cannot see the folder needs the same sentence plus the contents of `skills/tailor-cv/SKILL.md`, `description.txt`, `experiences.txt`, `main.tex`, and `sections/`.

The skill keeps the PDF to one or two pages, changes wording only where `experiences.txt` already supports it, and leaves both input files untouched.

## Compile by hand

XeLaTeX, from the repo root. Run it twice so the page numbers settle. Perl is not installed, so `latexmk` will fail.

```bash
xelatex -interaction=nonstopmode -halt-on-error main.tex
xelatex -interaction=nonstopmode -halt-on-error main.tex
```

Theme color is `\theme` at the top of `main.tex`. Contact icons come from the fontawesome package.

## License

Available under [the MIT license](https://opensource.org/licenses/MIT), but all the data is owned by me.
