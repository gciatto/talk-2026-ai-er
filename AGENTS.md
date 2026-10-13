# AGENTS.md

Guidance for AI agents (and humans) working on this repository.

## What this is

Slides of two lectures by Giovanni Ciatto, within the *AI Course — Regione Emilia-Romagna 2026 (Part 2)*:

1. `llmaas` — LLM-as-a-Service: APIs, tools, and agents (27/10/2026, 4h)
2. `governance` — Sovereign and open alternatives to large LLMs (18/11/2026, 3h)

The audience is __mixed__ (managers and technicians): slides favour concepts and trade-offs,
with short code excerpts and simple _guided_ exercises (live demos), no exercise solutions walkthroughs, no quizzes.

Slides are __cherry-picked__ (copied, then trimmed) from the University course
[slides-module2](https://github.com/unibo-fc-isi-iag/slides-module2) (*Intelligent Agents*, Module 2),
whose lectures `genai` the audience has already seen: don't repeat its contents.
The structure, conventions, and shortcodes are the same as there.

Technically: a [Hugo](https://gohugo.io/) site rendered as [reveal.js](https://revealjs.com/) presentations via the
[reveal-hugo](https://github.com/cric96/reveal-hugo) theme, published at <https://gciatto.github.io/talk-2026-ai-er>.

## Running locally

```bash
git submodule update --init --recursive   # theme, shared-slides, lab-snippets are submodules
hugo serve                                # requires Hugo *extended* (SCSS compilation)
```

Then open <http://localhost:1313/talk-2026-ai-er/>. `hugo` alone builds into `build/`.

## Layout

| Path | Purpose |
|------|---------|
| `config.toml` | Hugo + reveal.js configuration |
| `content/_index.md` | Landing presentation: ToC (`#/toc`), teacher, goals, how to run the examples (`#/lab-snippets-run`) |
| `content/<lecture>/_index.md` | One lecture = one directory = one presentation, with its pictures next to it |
| `layouts/shortcodes/` | Custom shortcodes, documented in `layouts/shortcodes/INDEX.md` (event name and period: `course_name`, `academic_year`) |
| `reusable/` | Snippets imported into slides: `front.md`, `footer.md`, `back.md`, `running-example.md` (use `{{< import >}}` for the latter) |
| `static/lab-snippets/` | Git submodule: <https://github.com/unibo-fc-isi-iag/lab-snippets>, the code of all examples; edit it there, not here |
| `themes/reveal-hugo/`, `shared-slides/` | Submodules, do not edit |

## Slide conventions

The same as in slides-module2 (see its `AGENTS.md`), in short:

- `---` separates slides; `{{% section %}}` groups vertical slides; `# ` = section title, `## ` = slide title
- `_italic_` for key terms, `__bold__` for the central concept; `{{% fragment %}}` for incremental reveal
- images via `{{< image src="./pic.png" max-h="60vh" alt="..." >}}`; columns via `multicol`/`col`
- code: excerpt `lab-snippets` files with `{{% code path="static/lab-snippets/snippets/lecture_<NAME>/example<ID>/file.py" from="10" to="20" %}}`,
  link full files as `../lab-snippets/...`, and write commands as `poetry run python -m snippets -l <NAME> -e <ID>`;
  when bumping the submodule, re-check every `from`/`to` range
- when copying slides from slides-module2, copy the pictures they use too, and fix links to lectures not included here
  (point them to `https://unibo-fc-isi-iag.github.io/slides-module2/<lecture>/`)

## Don'ts

- Don't edit submodules in place; don't commit `build/`, `public/`, PDFs.
- Don't put any `index.md` / `INDEX.md` under `content/`, nor double curly braces in non-template files under `layouts/`.
- Don't rename lecture directories: their names are the public URLs.
